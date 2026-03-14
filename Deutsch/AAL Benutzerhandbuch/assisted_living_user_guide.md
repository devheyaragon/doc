# Assistenzwohnen-Monitor — Benutzerhandbuch

**Version 1.0 · März 2026**

---

## Was es tut

Der Assistenzwohnen-Monitor erkennt unerwartete Inaktivität im Haushalt. Er lernt die normalen Tagesmuster des Bewohners über 7 Tage und löst dann einen Alarm aus, wenn keine Bewegung erkannt wird, während das System eine Anwesenheit vorhersagt.

Sie interagieren mit dem System auf zwei Arten:
- **Die Monitor-Weboberfläche** — zur Ansicht des Status, Anpassung der Einstellungen und Verwaltung des Urlaubsmodus
- **Node-RED** — zum Empfang und zur Reaktion auf MQTT-Alarme

---

## Die Weboberfläche

### Erkennungseinstellungen

Das Einstellungsfenster zeigt die aktiven Erkennungsparameter:

| Einstellung | Standard | Bedeutung |
|---|---|---|
| Vorhersageschwelle | 0.70 | Minimale KI-Sicherheit, dass Anwesenheit erwartet wird |
| Stilleschwelle | 30 min | Inaktivitätsdauer vor Auslösung einer Anomalie |
| Notfallschwelle | 2 h | Längere Abwesenheit, die NOTFALL-Alarm auslöst |
| Alarmdrosselung | 60 min | Mindestabstand zwischen wiederholten Alarmen pro Raum |
| MQTT-Topic | `aragon/assistedliving/anomaly` | Topic, wo Alarme veröffentlicht werden |

### Ereignisverlauf

Das Ereignisfenster zeigt ein protokolliertes Journal aller Anomalie-Alarme mit Raum, Schweregrad und Stille-Dauer zum Zeitpunkt des Alarms.

### Urlaubsmodus

Aktivieren Sie den Urlaubsmodus, wenn der Bewohner nicht zu Hause ist, um Fehlalarme zu vermeiden.

**Zum Aktivieren:**
1. Öffnen Sie den Bereich Assistenzwohnen in der Weboberfläche
2. Schalten Sie **Urlaubsmodus → EIN**
3. Geben Sie optional einen Grund ein (z.B. *„Urlaub – Rückkehr am 20. März"*)

**Zum Deaktivieren:** Schalten Sie **Urlaubsmodus → AUS**, wenn der Bewohner zurückkehrt.

> ⚠️ Wenn der Urlaubsmodus **nicht** aktiviert ist und das Haus leer ist, werden NOTFALL-Alarme nach 2 Stunden Stille ausgelöst.

---

## Funktionsweise der Erkennung

Alle paar Minuten prüft das System zwei Bedingungen für jeden überwachten Raum (Schlafzimmer und Wohnzimmer):

- **Vorhersage ≥ 0.70** — das KI-Modell erwartet gerade eine Anwesenheit
- **Stille ≥ 30 min** — keine Bewegung seit mindestens 30 Minuten erkannt

Wenn **beide** Bedingungen gleichzeitig erfüllt sind, wird ein Alarm über MQTT veröffentlicht.

Wenn die Stille **2 Stunden** überschreitet, eskaliert der Schweregrad zu **NOTFALL**, unabhängig von der Vorhersage. Wiederholte Alarme pro Raum werden gedrosselt — maximal einmal alle 60 Minuten.

---

## MQTT-Alarme in Node-RED

### Topic

```
aragon/assistedliving/anomaly
```

### Beispiel-Payload

```json
{
  "severity": "EMERGENCY",
  "room": "Bedroom",
  "forecast": 1.000,
  "silent_minutes": 121,
  "vacation_mode": false
}
```

### Vorgeschlagener Node-RED-Flow

1. **MQTT In**-Node — Topic setzen auf: `aragon/assistedliving/anomaly`
2. **JSON**-Node zum Parsen des Payloads
3. **Switch**-Node zur Weiterleitung nach Schweregrad:
   - `"EMERGENCY"` → Push-Benachrichtigung, SMS oder E-Mail
   - Andere → Protokollierung zum Dashboard oder Hausautomation
4. Optional: Filtern auf `vacation_mode == false`, um Urlaubsalarme zu unterdrücken

---

## Schnellreferenz

| Situation | Aktion |
|---|---|
| Bewohner fährt in Urlaub | Urlaubsmodus in der Oberfläche **vor** Abreise aktivieren |
| Bewohner kehrt zurück | Urlaubsmodus in der Oberfläche deaktivieren |
| Zu viele wiederholte Alarme | Alarme sind gedrosselt — max. einmal/Stunde pro Raum |
| Keine Alarme in Node-RED | Topic überprüfen: `aragon/assistedliving/anomaly` |
| System gerade installiert | 7 Tage für die Lernphase warten |

---

*Das System benötigt mindestens 7 Tage Sensordaten, bevor die Anomalieerkennung aktiv wird.*
