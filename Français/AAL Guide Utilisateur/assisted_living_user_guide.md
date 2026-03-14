# Moniteur de Vie Assistée — Guide Utilisateur

**Version 1.0 · Mars 2026**

---

## Ce qu'il fait

Le Moniteur de Vie Assistée détecte une inactivité inattendue dans le domicile. Il apprend les habitudes quotidiennes normales du résident pendant 7 jours, puis déclenche une alerte lorsqu'aucun mouvement n'est détecté pendant une période où le système prédit qu'une personne devrait être présente.

Vous interagissez avec le système de deux manières :
- **L'interface web Monitor** — pour voir l'état, ajuster les paramètres et gérer le Mode Vacances
- **Node-RED** — pour recevoir et réagir aux alertes MQTT

---

## L'Interface Web

### Paramètres de Détection

Le panneau Paramètres affiche les paramètres de détection actifs :

| Paramètre | Défaut | Signification |
|---|---|---|
| Seuil de prévision | 0.70 | Confiance minimale de l'IA qu'une présence est attendue |
| Seuil de silence | 30 min | Durée d'inactivité avant déclenchement d'une anomalie |
| Seuil d'urgence | 2 h | Absence prolongée qui déclenche une alerte URGENCE |
| Limitation des alertes | 60 min | Intervalle minimum entre alertes répétées par pièce |
| Topic MQTT | `aragon/assistedliving/anomaly` | Topic où les alertes sont publiées |

### Historique des Événements

Le panneau Événements affiche un journal horodaté de toutes les alertes d'anomalie, incluant la pièce, la gravité et la durée de silence au moment de l'alerte.

### Mode Vacances

Activez le Mode Vacances chaque fois que le résident s'absente du domicile pour éviter les fausses alertes.

**Pour activer :**
1. Ouvrez la section Vie Assistée dans l'interface web
2. Activez **Mode Vacances → ON**
3. Entrez facultativement une raison (ex. *« Vacances – retour le 20 mars »*)

**Pour désactiver :** Basculez **Mode Vacances → OFF** lorsque le résident rentre à domicile.

> ⚠️ Si le Mode Vacances n'est **pas** activé et que le domicile est vide, des alertes URGENCE se déclencheront après 2 heures de silence.

---

## Fonctionnement de la Détection

Toutes les quelques minutes, le système vérifie deux conditions pour chaque pièce surveillée (Chambre et Salon) :

- **Prévision ≥ 0.70** — le modèle d'IA s'attend à une présence en ce moment
- **Silence ≥ 30 min** — aucun mouvement détecté pendant au moins 30 minutes

Lorsque **les deux** conditions sont remplies simultanément, une alerte est publiée sur MQTT.

Si le silence dépasse **2 heures**, la gravité passe à **URGENCE** quelle que soit la prévision. Les alertes répétées par pièce sont limitées à une fois toutes les 60 minutes maximum.

---

## Alertes MQTT dans Node-RED

### Topic

```
aragon/assistedliving/anomaly
```

### Exemple de Payload

```json
{
  "severity": "EMERGENCY",
  "room": "Bedroom",
  "forecast": 1.000,
  "silent_minutes": 121,
  "vacation_mode": false
}
```

### Flux Node-RED Suggéré

1. Nœud **MQTT In** — définir le topic sur : `aragon/assistedliving/anomaly`
2. Nœud **JSON** pour analyser le payload
3. Nœud **Switch** pour router par gravité :
   - `"EMERGENCY"` → notification push, SMS ou email
   - Autre → journalisation vers tableau de bord ou domotique
4. Optionnel : filtrer sur `vacation_mode == false` pour supprimer les alertes pendant les vacances

---

## Référence Rapide

| Situation | Action |
|---|---|
| Résident part en vacances | Activer le Mode Vacances dans l'interface **avant** son départ |
| Résident rentre à domicile | Désactiver le Mode Vacances dans l'interface |
| Trop d'alertes répétées | Les alertes sont limitées — max une fois/heure par pièce |
| Aucune alerte dans Node-RED | Vérifier le topic : `aragon/assistedliving/anomaly` |
| Système nouvellement installé | Attendre 7 jours pour la période d'apprentissage |

---

*Le système nécessite au moins 7 jours de données de capteurs avant que la détection d'anomalies ne devienne active.*
