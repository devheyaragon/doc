# Aqara FP300 — Guide d'installation

---

## 1. Présentation du matériel

Le FP300 est un capteur multifonction alimenté par batterie combinant :

- **Radar mmWave 60 GHz** — détection de présence statique
- **Capteur PIR** — déclenchement rapide lors des premiers mouvements
- **Capteurs de température, d'humidité et d'éclairement**

La communication s'effectue via **Zigbee** (fonctionnalités complètes via Zigbee2MQTT) ou Matter-over-Thread (exposition limitée des paramètres). Utiliser toujours le **mode Zigbee** avec Z2M pour un contrôle de configuration complet.

**Spécifications de détection :**
- Portée maximale de détection : **6 m**
- Angle de champ : **120°**
- Autonomie batterie : ~1 an (variable selon les intervalles de scrutation et l'activité de la pièce)

---

## 2. Installation physique

### 2.1 Méthodes de montage

| Méthode | Hauteur max. | Remarques |
|---|---|---|
| **Adhésif** | 2 m | Fourni dans la boîte ; réversible |
| **Magnétique** | 2 m | Idéal pour repositionner lors des tests |
| **Vis et chevilles** | Aucune limite | Obligatoire au-dessus de 2 m |

> **Conseil :** Utiliser la fixation magnétique lors de l'installation initiale et des tests. Ne fixer définitivement qu'après validation de la couverture.

### 2.2 Recommandations de hauteur

| Hauteur | Exigence |
|---|---|
| **1,4 – 1,8 m** | Optimal pour montage mural/en angle |
| **~2,0 m** | Incliner légèrement le capteur vers le bas en direction de la zone d'activité |
| **> 2,0 m** | Vis/chevilles obligatoires ; l'angle doit compenser la hauteur |
| **Plafond** | Possible mais portée réduite ; déconseillé pour les pièces > 15 m² |

### 2.3 Stratégie de positionnement

**Le montage en angle est fortement préféré** au montage mural plat. Une position en angle avec le capteur orienté vers le centre de la pièce offre la couverture la plus large et minimise les zones mortes aux extrémités.

**Éviter de placer le capteur :**
- À proximité de bouches de climatisation ou de ventilation
- Près de ventilateurs de plafond ou de purificateurs d'air
- Face à de grandes surfaces métalliques ou en verre (les réflexions provoquent des fausses détections)
- Dans des positions où le cône de détection traverse les murs vers des pièces adjacentes

### 2.4 Zone morte en champ proche

La détection en dessous de **1 m** du capteur est peu fiable. En tenir compte lors du positionnement — ne pas placer le capteur là où des occupants se trouvent régulièrement directement devant lui à courte distance.

### 2.5 Recommandation avant fixation définitive

Avant de fixer définitivement le capteur, tester la position avec du ruban de masquage ou la fixation magnétique pendant **24 à 48 heures**. Cela permet d'identifier les angles morts et les sources d'interférences avant de valider l'emplacement final.

---

## 3. Couplage avec Zigbee2MQTT

1. Dans l'interface Z2M, cliquer sur **« Autoriser la jonction »**
2. Maintenir le bouton du capteur appuyé pendant **5 secondes** jusqu'au clignotement de la LED — cela déclenche le mode de couplage
3. L'appareil apparaît dans Z2M sous le nom **`Aqara PS-S04D`**
4. Le renommer dans l'interface Z2M (ex. `fp300_salon`)
5. Vérifier dans l'onglet **Exposes** que tous les paramètres sont visibles : `presence`, `temperature`, `humidity`, `illuminance`, `detection_range`, etc.

---

## 4. Configuration — Ordre correct

> ⚠️ **Toujours appliquer tous les paramètres avant de lancer le Spatial Learning.** Le capteur calibre sa référence de pièce vide en fonction de la configuration active. Modifier les paramètres après l'apprentissage dégrade la précision et nécessite une nouvelle session.

---

### Important : Comprendre le comportement de l'interface Z2M

**Le FP300 est un appareil sur batterie en mode veille.** Lorsque vous modifiez une valeur de paramètre dans l'onglet Exposes de Z2M, la modification est **immédiatement envoyée** à l'appareil, mais le capteur ne la traite que lors de son prochain réveil — ce qui peut prendre **plusieurs minutes** selon l'activité de la pièce.

**Pour forcer un retour immédiat après modification des paramètres :**
1. Se rendre près du capteur
2. **Appuyer une fois sur le bouton** (pression courte)
3. L'appareil se réveille immédiatement et traite toutes les commandes en attente en quelques secondes

L'interface Z2M se met à jour automatiquement dès que le capteur répond. Aucun bouton « Appliquer » n'est requis — chaque basculement/menu déroulant envoie instantanément sa propre commande.

---

### Étape 1 — Définir le mode de détection

| Paramètre Z2M | Valeur recommandée | Remarques |
|---|---|---|
| `presence_detection_options` | `mmwave` | Élimine les fausses détections dues au PIR |
| `pir_detection_interval` | `60` sec | Uniquement pertinent en mode `both` ; limite le taux de redéclenchement PIR |

Utiliser `both` uniquement si une réaction lumineuse très rapide (< 1 sec) est une exigence absolue. Dans tous les autres cas, `mmwave` fournit des données de présence plus fiables.

---

### Étape 2 — Configurer la sensibilité

| Paramètre Z2M | Valeur | Remarques |
|---|---|---|
| `motion_sensitivity` | Voir profils de pièces ci-dessous | **Contrôle la sensibilité du radar mmWave** (pas le PIR) — réglage fondamental de la détection |
| `ai_sensitivity_adaptive` | `ON` | Permet l'auto-optimisation dans le temps |

> **Clarification sur le nom du paramètre :** Malgré son nom `motion_sensitivity`, ce paramètre contrôle la **sensibilité du radar mmWave** pour la détection de présence statique. Il reste le paramètre de réglage principal même lorsque `presence_detection_options` est réglé sur `mmwave` uniquement.

---

### Étape 3 — Définir le délai d'absence

| Paramètre Z2M | Plage | Remarques |
|---|---|---|
| `absence_delay_timer` | 10 – 300 sec | Voir profils de pièces ci-dessous |

Ce timer définit combien de temps après le dernier mouvement détecté le capteur attend avant de déclarer la pièce vide. Une valeur trop courte provoque de fausses absences lorsque les occupants sont assis et immobiles.

---

### Étape 4 — Restreindre la portée de détection

Les 24 bandes `detection_range_X` (chacune représentant **0,25 m**) sont par défaut à `true`, ce qui signifie que le capteur analyse la totalité des 0 à 6 m, y compris à travers les murs. Restreindre à la profondeur réelle de la pièce.

**Calcul :**
- Diviser la profondeur maximale occupée (en mètres) par 0,25 → nombre de bandes à activer
- Toujours désactiver `detection_range_0` à `detection_range_3` (zone morte 0–1 m près du mur de montage)
- Désactiver toutes les bandes au-delà de la limite de la pièce

**Exemple — pièce de 4 m, capteur en angle :**

| Bandes | Portée | Valeur |
|---|---|---|
| `detection_range_0` → `_3` | 0 – 1 m | `false` (zone morte) |
| `detection_range_4` → `_15` | 1 – 4 m | `true` |
| `detection_range_16` → `_23` | 4 – 6 m | `false` (au-delà de la pièce) |

---

### Étape 5 — Activer la détection d'interférences IA

| Paramètre Z2M | Valeur | Remarques |
|---|---|---|
| `ai_interference_source_selfidentification` | `ON` | Apprend à ignorer ventilateurs, climatisation, convection |

---

### Étape 6 — Lancer le Spatial Learning

C'est toujours la **dernière étape**, après avoir sauvegardé tous les autres paramètres.

1. **Vider complètement la pièce**
2. Dans l'interface Z2M → onglet Exposes → `spatial_learning` → **« Start Learning »**
3. Rester hors de la pièce pendant au moins **60 secondes**
4. Aucune confirmation n'apparaît dans l'interface ou dans les logs — c'est le comportement attendu
5. Le capteur continue un apprentissage autonome en arrière-plan indéfiniment après le déclenchement initial

> Si la pièce était occupée lors de sessions d'apprentissage précédentes, ces références sont contaminées. Chaque nouvelle session écrase entièrement la précédente.

---

## 5. Profils par type de pièce

### 🛋️ Salon

Scénario : occupation prolongée, souvent assis et immobile ; télévision, ventilateur ou climatisation possible.

| Paramètre | Valeur |
|---|---|
| `presence_detection_options` | `mmwave` |
| `motion_sensitivity` | `medium` |
| `ai_sensitivity_adaptive` | `ON` |
| `ai_interference_source_selfidentification` | `ON` |
| `absence_delay_timer` | `90` sec |
| Portée de détection | Limiter à la profondeur de la zone assise (typiquement 3–5 m) |
| Position de montage | Angle, 1,4–1,8 m, orienté vers canapé/zone assise |

---

### 🛏️ Chambre à coucher

Scénario : occupant endormi avec mouvements minimes ; éviter absolument les fausses absences pendant le sommeil.

| Paramètre | Valeur |
|---|---|
| `presence_detection_options` | `mmwave` |
| `motion_sensitivity` | `high` |
| `ai_sensitivity_adaptive` | `ON` |
| `ai_interference_source_selfidentification` | `ON` |
| `absence_delay_timer` | `180` sec |
| Portée de détection | Limiter à la distance du lit (typiquement 2–4 m) |
| Position de montage | Angle, 1,4–1,7 m, orienté directement vers le lit |

> **Remarque :** Le FP300 ne prend **pas** en charge la surveillance du sommeil (détection respiratoire/micro-mouvements). Cette fonctionnalité est exclusive à l'Aqara FP2. La haute sensibilité permet la détection d'occupants très immobiles, mais n'est pas équivalente à la surveillance du sommeil.

---

### 🚿 Salle de bain / WC

Scénario : visites courtes ; allumage/extinction rapide requis ; capteur d'humidité utile pour l'automatisation de la ventilation.

| Paramètre | Valeur |
|---|---|
| `presence_detection_options` | `both` |
| `motion_sensitivity` | `low` |
| `ai_sensitivity_adaptive` | `ON` |
| `ai_interference_source_selfidentification` | `ON` |
| `absence_delay_timer` | `30` sec |
| Portée de détection | Étroite — limiter à la profondeur de la pièce (typiquement 1–2,5 m) |
| Position de montage | Angle haut au-dessus de la porte, orienté vers l'intérieur |

> **Bonus :** Utiliser le capteur d'humidité intégré pour déclencher un extracteur d'air lorsque `humidity > 70 %`, indépendamment de la détection de présence.

---

### 🚶 Couloir / Hall

Scénario : transit court uniquement ; risque élevé de détection à travers les murs vers les pièces adjacentes.

| Paramètre | Valeur |
|---|---|
| `presence_detection_options` | `both` |
| `motion_sensitivity` | `low` |
| `ai_sensitivity_adaptive` | `ON` |
| `ai_interference_source_selfidentification` | `OFF` |
| `absence_delay_timer` | `15` sec |
| Portée de détection | Correspondre exactement à la longueur du couloir ; aucune marge |
| Position de montage | Mur de fond orienté dans la longueur, ou angle haut |

---

### 💼 Bureau / Espace de travail

Scénario : une personne assise à un bureau pendant de longues périodes ; automatisation de l'éclairage fréquente.

| Paramètre | Valeur |
|---|---|
| `presence_detection_options` | `mmwave` |
| `motion_sensitivity` | `medium` |
| `ai_sensitivity_adaptive` | `ON` |
| `ai_interference_source_selfidentification` | `ON` |
| `absence_delay_timer` | `60` sec |
| Portée de détection | Limiter à la distance du bureau (typiquement 1,5–3 m) |
| Position de montage | Angle à côté ou au-dessus du bureau, orienté vers le poste de travail |

---

## 6. Référence rapide de sensibilité

| Profondeur de pièce | Sensibilité recommandée |
|---|---|
| < 4 m | `low` |
| 4 – 7 m | `medium` |
| > 7 m | `high` |

---

## 7. Le bouton

| Action | Fonction |
|---|---|
| **Pression courte unique** | Force un réveil immédiat ; traite les commandes Z2M en attente instantanément |
| **Maintien 5+ secondes** | Réinitialisation usine / nouveau couplage (aussi pour basculer Zigbee ↔ Thread) |
| **10 pressions courtes** | Force la reconnexion au réseau |

> **Astuce :** Après avoir modifié des paramètres dans Z2M, appuyer une fois sur le bouton pour forcer un retour immédiat au lieu d'attendre plusieurs minutes que l'appareil se réveille naturellement.

---

## 8. Dépannage

| Symptôme | Cause probable | Solution |
|---|---|---|
| Détections fantômes la nuit | PIR réagit aux variations thermiques ou aux courants d'air | Passer en mode `mmwave` uniquement ; activer `ai_interference_source_selfidentification` |
| Fausses absences en position assise immobile | Sensibilité trop faible ou timer trop court | Augmenter `motion_sensitivity` ; porter `absence_delay_timer` à ≥ 60 sec |
| Détections à travers les murs | Portée de détection non restreinte | Désactiver les bandes `detection_range` au-delà de la limite de la pièce |
| Fausses détections persistantes après configuration complète | Spatial Learning effectué avec la pièce occupée | Relancer le Spatial Learning avec la pièce complètement vide |
| Pas de détection près du capteur | Zone morte en champ proche (< 1 m) | Repositionner le capteur ; maintenir `detection_range_0` – `_3` désactivés |
| Le capteur signale « absent » immédiatement après départ | `absence_delay_timer` trop court | Augmenter à la valeur appropriée selon le type de pièce |
| Les paramètres Z2M ne se mettent pas à jour dans l'interface | L'appareil est en veille et n'a pas encore traité la commande | Appuyer une fois sur le bouton pour forcer un réveil et un traitement immédiats |
