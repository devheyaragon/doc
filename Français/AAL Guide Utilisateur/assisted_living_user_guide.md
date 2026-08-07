# Moniteur de maintien à domicile — Guide utilisateur

**Version 1.1 · Août 2026**

---

## Fonctionnement

Le Moniteur de maintien à domicile surveille toute inactivité inattendue dans le logement. Il apprend les habitudes quotidiennes normales du résident, puis déclenche une alerte si aucun mouvement n'est détecté à un moment où le modèle prévoit que quelqu'un devrait être présent. Pour une explication plus complète du fonctionnement de l'apprentissage du modèle de prédiction, consultez la documentation principale du *Service de domotique prédictive*, §3.

La détection d'anomalies nécessite à elle seule au moins **7 jours** de données de capteurs collectées avant de devenir active — une exigence distincte, plus courte, propre à cette fonctionnalité, différente de la fenêtre d'entraînement de 14 jours du modèle de prédiction principal décrite dans ce document. Ne soyez pas surpris que ces deux chiffres diffèrent ; ils répondent à des questions différentes.

Vous interagissez avec le système de deux manières :

- **L'interface web de suivi (Monitor)** — pour consulter l'état, ajuster les paramètres et gérer le Mode Vacances
- **Node-RED** — pour recevoir les alertes MQTT et agir en conséquence

---

## L'interface web

### Paramètres de détection

Ces valeurs, en lecture seule, sont visibles dans le panneau **Paramètres** :

| Paramètre | Valeur par défaut | Signification |
|---|---|---|
| Seuil de prévision | 0,70 | Confiance minimale que quelqu'un devrait être présent |
| Seuil de silence | 30 min | Durée d'inactivité avant qu'une anomalie soit signalée |
| Santé du capteur | 2 h | Absence prolongée déclenchant une alerte URGENCE |
| Limitation des alertes | 60 min | Écart minimal entre deux alertes répétées pour la même pièce |
| Fenêtre d'activité croisée | 30 min | Depuis combien de temps *une autre* pièce surveillée doit avoir montré une activité pour retenir une alerte URGENCE (voir ci-dessous) |
| Sujet MQTT | `aragon/assisted_living/anomaly` | Où les alertes sont publiées |

### Historique des événements

Le panneau **Événements** affiche un journal horodaté de toutes les alertes d'anomalie, incluant la pièce, la gravité et la durée du silence.

### Mode Vacances

Activez le Mode Vacances chaque fois que le résident s'absente du logement, afin d'éviter les fausses alertes.

**Pour activer :**

1. Ouvrez la section **Maintien à domicile** dans l'interface web
2. Basculez **Mode Vacances → ON**
3. Saisissez éventuellement un motif (par ex. *« Vacances – retour le 20 mars »*)

**Pour désactiver :**

- Basculez **Mode Vacances → OFF** lorsque le résident est de retour

> ⚠️ Si le Mode Vacances n'est **pas** activé, qu'une pièce surveillée reste silencieuse plus de **2 heures**, **et qu'aucune autre pièce surveillée n'a montré d'activité récente non plus**, le système déclenchera **bien** une alerte URGENCE. Si une autre pièce a montré une activité récente, l'alerte est retenue à la place — voir « Fonctionnement de la détection » ci-dessous pour comprendre pourquoi, et comment le vérifier malgré tout.

---

## Fonctionnement de la détection

Le système vérifie deux conditions toutes les quelques minutes pour chaque pièce surveillée :

1. **Prévision ≥ 0,70** — le modèle d'IA prévoit que quelqu'un est présent en ce moment
2. **Silence ≥ 30 min** — aucun mouvement n'a été détecté depuis au moins 30 minutes

Si **les deux** conditions sont réunies simultanément, une alerte est publiée sur MQTT.

Si le silence se prolonge au-delà de **2 heures**, le système vérifie un point supplémentaire avant d'escalader : est-ce qu'**une autre pièce surveillée** a montré une activité réelle au cours des 30 dernières minutes ? Si c'est le cas, le silence prolongé de cette pièce est considéré comme attendu — le résident étant actif ailleurs dans le logement constitue en soi une preuve de sa sécurité — et une alerte de faible gravité `info` est publiée à la place (voir ci-dessous), et non une alerte URGENCE. Ce n'est que si aucune pièce surveillée n'a montré d'activité récente, nulle part, que la gravité passe à **URGENCE**, quel que soit le score de prévision.

Les alertes répétées pour une même pièce — quelle que soit leur gravité — sont limitées à **une par heure** afin d'éviter la saturation. Les alertes URGENCE et `info` sont limitées indépendamment l'une de l'autre, de sorte qu'une série d'alertes `info` retenues ne peut jamais retarder une véritable alerte URGENCE.

---

## Alertes MQTT dans Node-RED

### Sujet

```
aragon/assisted_living/anomaly
```

> Notez le tiret bas (underscore) dans `assisted_living` — une faute fréquente ailleurs est `assistedliving`, qui ne recevra alors silencieusement aucune alerte.

### Exemple de charge utile — URGENCE

> **Remarque :** le champ `message` est toujours généré en anglais par le système, quelle que soit la langue de cette documentation — il n'est pas traduit. L'exemple ci-dessous reflète donc exactement ce que vous recevrez réellement dans Node-RED.

```json
{
  "alert_type": "absence_anomaly",
  "severity": "emergency",
  "room": "Bedroom",
  "forecast_probability": 1.0,
  "minutes_silent": 121.0,
  "threshold_used": 0.7,
  "silence_threshold_minutes": 30,
  "detection_method": "extended_absence_no_holiday",
  "timestamp": "2026-08-06T21:14:02.331Z",
  "message": "[EMERGENCY] Expected activity in Bedroom but no motion detected for 121 minutes."
}
```

### Exemple de charge utile — alerte retenue (`info`)

Publiée à la place d'une alerte URGENCE lorsque le Mode Vacances ou la corroboration inter-pièces explique le silence :

```json
{
  "alert_type": "absence_anomaly_suppressed",
  "severity": "info",
  "room": "Bedroom",
  "forecast_probability": 1.0,
  "minutes_silent": 121.0,
  "detection_method": "extended_absence_activity_elsewhere",
  "timestamp": "2026-08-06T21:14:02.331Z",
  "message": "[INFO] Bedroom silent for 121 minutes, but recent activity was confirmed in another monitored room — no action needed."
}
```

Le champ `detection_method` indique *pourquoi* l'alerte a été retenue : `extended_absence_activity_elsewhere` (corroboration inter-pièces) ou `extended_absence_holiday_mode` (Mode Vacances activé). Aucune des deux charges utiles ne comporte de champ `vacation_mode` — l'état du Mode Vacances est publié séparément, sur le sujet `aragon/assisted_living/vacation_status`, et non intégré à chaque alerte.

*(Notez que les noms de champs JSON — `severity`, `room`, `forecast_probability`, etc. — sont des identifiants techniques utilisés tels quels par le système et ne doivent jamais être traduits dans vos flux Node-RED.)*

### Flux Node-RED suggéré

```
[MQTT In]  →  [JSON]  →  [Switch: severity]
                            ├── "emergency"  →  [Notify / SMS / Email]
                            ├── "info"       →  [Log / Dashboard]
                            └── else         →  [Log / Dashboard]
```

1. Ajoutez un nœud **MQTT In** — définissez le sujet sur `aragon/assisted_living/anomaly`
2. Ajoutez un nœud **JSON** pour analyser la charge utile
3. Ajoutez un nœud **Switch** pour router selon `severity` :
   - `emergency` → notification push, SMS ou e-mail
   - `info` → journalisation vers un tableau de bord (facultatif : afficher le champ `detection_method` pour qu'un aidant comprenne *pourquoi* une alerte attendue n'a pas déclenché d'URGENCE)
   - Autre → journalisation vers un tableau de bord ou un système domotique
4. Pour distinguer une alerte retenue en raison du Mode Vacances de celle causée par l'activité d'une autre pièce, filtrez ou orientez le flux sur `detection_method` plutôt que sur un champ `vacation_mode` — celui-ci n'existe pas dans la charge utile.

---

## Référence rapide

| Situation | Que faire |
|---|---|
| Le résident part en vacances | Activez le **Mode Vacances** dans l'interface avant son départ |
| Le résident est de retour | Désactivez le **Mode Vacances** dans l'interface |
| Trop d'alertes reçues | Vérifiez le paramètre de limitation des alertes (par défaut : 60 min) |
| Aucune alerte ne parvient à Node-RED | Vérifiez le sujet MQTT : `aragon/assisted_living/anomaly` (attention au tiret bas) |
| Une alerte URGENCE était attendue mais rien n'est arrivé | Consultez l'historique des événements pour une alerte de gravité `info` sur cette pièce — une autre pièce surveillée a probablement montré une activité récente (ou le Mode Vacances était actif) ; consultez le champ `detection_method` de l'alerte pour le savoir |
| Système tout juste installé | Patientez 7 jours pour que la détection d'anomalies s'active |

---

*Le système nécessite au moins 7 jours de données de capteurs avant que la détection d'anomalies ne devienne active. (Ceci est distinct de la fenêtre d'entraînement de 14 jours du modèle de prédiction principal, et plus court qu'elle — voir la documentation principale, §4, pour cette distinction.)*

*Version 1.1 — Corrections apportées : sujet MQTT corrigé de `aragon/assistedliving/anomaly` à `aragon/assisted_living/anomaly` dans tout le document (la version précédente n'aurait silencieusement reçu aucune alerte). Exemple de charge utile corrigé — `forecast` → `forecast_probability`, `silent_minutes` → `minutes_silent`, et suppression du champ `vacation_mode` qui n'existe pas (l'état du Mode Vacances est publié séparément, sur son propre sujet, et non intégré à chaque alerte) ; ajout des champs réels (`alert_type`, `threshold_used`, `detection_method`, `timestamp`, `message`) pour correspondre à la charge utile réelle. Ajout de la corroboration d'activité inter-pièces dans l'ensemble du document — une vérification réelle effectuée par le système avant toute escalade vers une alerte URGENCE, non documentée auparavant — y compris son propre type d'alerte retenue de gravité `info`, un nouvel exemple de charge utile, un flux Node-RED mis à jour avec une troisième branche de routage, et un avertissement sur le Mode Vacances corrigé, qui ne laisse plus entendre que l'URGENCE est la seule issue possible d'un silence prolongé non surveillé. Ajout d'un renvoi vers la documentation principale pour le processus d'apprentissage sous-jacent, ainsi qu'une note explicative sur la raison pour laquelle les « 7 jours » de ce guide et les « 14 jours » de la documentation principale sont deux chiffres différents et corrects, et non une contradiction.*
