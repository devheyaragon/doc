# Monitor für betreutes Wohnen — Benutzerhandbuch

**Version 1.1 · August 2026**

---

## Funktionsweise

Der Monitor für betreutes Wohnen überwacht unerwartete Inaktivität im Zuhause. Er lernt die normalen täglichen Muster des Bewohners und löst dann einen Alarm aus, wenn zu einem Zeitpunkt, an dem das Modell die Anwesenheit einer Person erwartet, keine Bewegung erkannt wird. Eine ausführlichere Erläuterung, wie das zugrunde liegende Vorhersagemodell die Muster Ihres Zuhauses lernt, finden Sie in der Hauptdokumentation des *Vorausschauenden Smart-Home-Diensts*, §3.

Die Anomalieerkennung selbst benötigt mindestens **7 Tage** gesammelter Sensordaten, bevor sie aktiv wird — eine separate, kürzere Anforderung, die speziell für diese Funktion gilt und sich vom 14-tägigen Trainingsfenster des in diesem Dokument beschriebenen Hauptvorhersagemodells unterscheidet. Lassen Sie sich nicht davon irritieren, dass diese beiden Zahlen unterschiedlich sind — sie beantworten unterschiedliche Fragen.

Sie interagieren auf zwei Arten mit dem System:

- **Die Monitor-Weboberfläche** — zur Statusanzeige, zum Anpassen von Einstellungen und zur Verwaltung des Urlaubsmodus
- **Node-RED** — zum Empfangen von und Reagieren auf MQTT-Alarme

---

## Die Weboberfläche

### Erkennungseinstellungen

Diese schreibgeschützten Werte sind im Bereich **Einstellungen** sichtbar:

| Einstellung | Standardwert | Bedeutung |
|---|---|---|
| Vorhersageschwelle | 0,70 | Mindestvertrauen, dass jemand anwesend sein sollte |
| Stilleschwelle | 30 Min. | Inaktivitätsdauer, bevor eine Anomalie gemeldet wird |
| Sensorzustand | 2 Std. | Verlängerte Abwesenheit, die einen NOTFALL-Alarm auslöst |
| Alarmdrosselung | 60 Min. | Mindestabstand zwischen wiederholten Alarmen für denselben Raum |
| Raumübergreifendes Aktivitätsfenster | 30 Min. | Wie kürzlich *ein anderer* überwachter Raum Aktivität gezeigt haben muss, um einen NOTFALL zurückzuhalten (siehe unten) |
| MQTT-Thema | `aragon/assisted_living/anomaly` | Wo Alarme veröffentlicht werden |

### Ereignisverlauf

Der Bereich **Ereignisse** zeigt ein zeitgestempeltes Protokoll aller Anomalie-Alarme an, einschließlich Raum, Schweregrad und Dauer der Stille.

### Urlaubsmodus

Aktivieren Sie den Urlaubsmodus immer dann, wenn der Bewohner nicht zu Hause ist, um Fehlalarme zu vermeiden.

**Zum Aktivieren:**

1. Öffnen Sie den Bereich **Betreutes Wohnen** in der Weboberfläche
2. Schalten Sie **Urlaubsmodus → EIN**
3. Geben Sie optional einen Grund ein (z. B. *„Urlaub – Rückkehr am 20. März"*)

**Zum Deaktivieren:**

- Schalten Sie **Urlaubsmodus → AUS**, sobald der Bewohner wieder zu Hause ist

> ⚠️ Ist der Urlaubsmodus **nicht** aktiviert, bleibt ein überwachter Raum länger als **2 Stunden** still, **und hat auch kein anderer überwachter Raum kürzlich Aktivität gezeigt**, löst das System **tatsächlich** einen NOTFALL-Alarm aus. Hat ein anderer Raum kürzlich Aktivität gezeigt, wird der Alarm stattdessen zurückgehalten — siehe „Funktionsweise der Erkennung" unten, warum das so ist und wie Sie es trotzdem erkennen können.

---

## Funktionsweise der Erkennung

Das System prüft alle paar Minuten zwei Bedingungen für jeden überwachten Raum:

1. **Vorhersage ≥ 0,70** — das KI-Modell erwartet, dass gerade jemand anwesend ist
2. **Stille ≥ 30 Min.** — seit mindestens 30 Minuten wurde keine Bewegung erkannt

Sind **beide** Bedingungen gleichzeitig erfüllt, wird ein Alarm an MQTT veröffentlicht.

Hält die Stille länger als **2 Stunden** an, prüft das System vor der Eskalation noch einen weiteren Punkt: Hat **irgendein anderer überwachter Raum** innerhalb der letzten 30 Minuten echte Aktivität gezeigt? Falls ja, wird die anhaltende Stille in diesem einen Raum als erwartungsgemäß behandelt — dass der Bewohner an anderer Stelle im Zuhause aktiv ist, stellt selbst einen Beleg für seine Sicherheit dar — und es wird stattdessen ein Alarm mit niedrigem Schweregrad `info` veröffentlicht (siehe unten), kein NOTFALL. Nur wenn nirgendwo ein überwachter Raum kürzliche Aktivität gezeigt hat, eskaliert der Schweregrad auf **NOTFALL**, unabhängig vom Vorhersagewert.

Wiederholte Alarme für denselben Raum — unabhängig vom Schweregrad — werden auf **einmal pro Stunde** gedrosselt, um eine Überflutung zu vermeiden. NOTFALL- und `info`-Alarme werden unabhängig voneinander gedrosselt, sodass eine Reihe zurückgehaltener `info`-Alarme einen echten NOTFALL niemals verzögern kann.

---

## MQTT-Alarme in Node-RED

### Thema

```
aragon/assisted_living/anomaly
```

> Beachten Sie den Unterstrich in `assisted_living` — ein häufiger Tippfehler andernorts ist `assistedliving`, wodurch stillschweigend keinerlei Alarme empfangen werden.

### Beispiel-Payload — NOTFALL

> **Hinweis:** Das Feld `message` wird vom System stets auf Englisch erzeugt, unabhängig von der Sprache dieser Dokumentation — es wird nicht übersetzt. Das folgende Beispiel entspricht daher genau dem, was Sie tatsächlich in Node-RED empfangen.

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

### Beispiel-Payload — zurückgehaltener Alarm (`info`)

Wird anstelle eines NOTFALL-Alarms veröffentlicht, wenn der Urlaubsmodus oder die raumübergreifende Aktivitätsprüfung die Stille erklärt:

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

Das Feld `detection_method` gibt an, *warum* der Alarm zurückgehalten wurde: `extended_absence_activity_elsewhere` (raumübergreifende Aktivitätsprüfung) oder `extended_absence_holiday_mode` (Urlaubsmodus war aktiv). Keine der beiden Payloads enthält ein Feld `vacation_mode` — der Status des Urlaubsmodus wird separat veröffentlicht, unter dem Thema `aragon/assisted_living/vacation_status`, nicht eingebettet in jeden einzelnen Alarm.

*(Beachten Sie, dass die JSON-Feldnamen — `severity`, `room`, `forecast_probability` usw. — technische Bezeichner sind, die vom System unverändert verwendet werden, und in Ihren Node-RED-Flows niemals übersetzt werden dürfen.)*

### Vorgeschlagener Node-RED-Flow

```
[MQTT In]  →  [JSON]  →  [Switch: severity]
                            ├── "emergency"  →  [Notify / SMS / Email]
                            ├── "info"       →  [Log / Dashboard]
                            └── else         →  [Log / Dashboard]
```

1. Fügen Sie einen **MQTT In**-Knoten hinzu — setzen Sie das Thema auf `aragon/assisted_living/anomaly`
2. Fügen Sie einen **JSON**-Knoten hinzu, um die Payload zu parsen
3. Fügen Sie einen **Switch**-Knoten hinzu, um nach `severity` weiterzuleiten:
   - `emergency` → Push-Benachrichtigung, SMS oder E-Mail
   - `info` → Protokollierung in einem Dashboard (optional: Feld `detection_method` anzeigen, damit eine Betreuungsperson erkennt, *warum* ein erwarteter Alarm nicht als NOTFALL ausgelöst wurde)
   - Sonstige → Protokollierung in einem Dashboard oder Smart-Home-System
4. Um einen durch den Urlaubsmodus zurückgehaltenen Alarm von einem durch die Aktivität eines anderen Raums verursachten zu unterscheiden, filtern oder verzweigen Sie anhand von `detection_method` statt eines Felds `vacation_mode` — Letzteres ist nicht Teil der Payload.

---

## Kurzübersicht

| Situation | Was zu tun ist |
|---|---|
| Bewohner fährt in den Urlaub | **Urlaubsmodus** in der Oberfläche vor der Abreise aktivieren |
| Bewohner ist zurück | **Urlaubsmodus** in der Oberfläche deaktivieren |
| Zu viele Alarme erhalten | Einstellung zur Alarmdrosselung prüfen (Standard: 60 Min.) |
| Keine Alarme in Node-RED | MQTT-Thema prüfen: `aragon/assisted_living/anomaly` (auf den Unterstrich achten) |
| Ein NOTFALL wurde erwartet, aber nichts kam an | Ereignisverlauf auf einen Alarm mit Schweregrad `info` für diesen Raum prüfen — wahrscheinlich hat ein anderer überwachter Raum kürzlich Aktivität gezeigt (oder der Urlaubsmodus war aktiv); das Feld `detection_method` des Alarms gibt Aufschluss darüber |
| System gerade erst installiert | 7 Tage warten, bis die Anomalieerkennung aktiv wird |

---

*Das System benötigt mindestens 7 Tage Sensordaten, bevor die Anomalieerkennung aktiv wird. (Dies ist getrennt vom 14-tägigen Trainingsfenster des Hauptvorhersagemodells zu sehen und kürzer als dieses — siehe Hauptdokumentation, §4, für diese Unterscheidung.)*

*Version 1.1 — Angewendete Korrekturen: MQTT-Thema durchgängig von `aragon/assistedliving/anomaly` auf `aragon/assisted_living/anomaly` korrigiert (die vorherige Version hätte stillschweigend keinerlei Alarme empfangen). Beispiel-Payload korrigiert — `forecast` → `forecast_probability`, `silent_minutes` → `minutes_silent`, und das nicht existierende Feld `vacation_mode` entfernt (der Status des Urlaubsmodus wird separat, auf einem eigenen Thema, veröffentlicht und nicht in jeden Alarm eingebettet); echte Felder (`alert_type`, `threshold_used`, `detection_method`, `timestamp`, `message`) ergänzt, um der tatsächlichen Payload zu entsprechen. Raumübergreifende Aktivitätsprüfung im gesamten Dokument ergänzt — eine reale, zuvor nicht dokumentierte Prüfung, die das System vor jeder Eskalation zu NOTFALL durchführt —, einschließlich eines eigenen zurückgehaltenen Alarmtyps mit Schweregrad `info`, einer neuen Beispiel-Payload, eines aktualisierten Node-RED-Flows mit einer dritten Routing-Verzweigung sowie eines korrigierten Warnhinweises zum Urlaubsmodus, der nicht mehr suggeriert, NOTFALL sei das einzig mögliche Ergebnis einer unbeaufsichtigten, verlängerten Stille. Ein Querverweis auf die Hauptdokumentation für den zugrunde liegenden Lernprozess wurde ergänzt, ebenso ein klärender Hinweis darauf, warum die „7 Tage" dieses Leitfadens und die „14 Tage" der Hauptdokumentation zwei unterschiedliche, jeweils korrekte Werte sind und keinen Widerspruch darstellen.*
