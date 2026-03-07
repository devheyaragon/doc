# Aqara FP300 — Installationshandbuch

---

## 1. Hardware-Übersicht

Der FP300 ist ein batteriebetriebener Multisensor mit folgenden Komponenten:

- **60-GHz-mmWave-Radar** — Erkennung statischer Anwesenheit
- **PIR-Sensor** — schnelle Erstauslösung bei Bewegung
- **Temperatur-, Feuchtigkeits- und Helligkeitssensoren**

Die Kommunikation erfolgt über **Zigbee** (voller Funktionsumfang via Zigbee2MQTT) oder Matter-over-Thread (eingeschränkte Parameterexponierung). Für vollständige Konfigurationsmöglichkeiten immer den **Zigbee-Modus** mit Z2M verwenden.

**Technische Spezifikationen:**
- Maximale Erfassungsreichweite: **6 m**
- Erfassungswinkel: **120°**
- Batterielaufzeit: ~1 Jahr (abhängig von Abfrageintervallen und Raumaktivität)

---

## 2. Physische Installation

### 2.1 Montagemethoden

| Methode | Max. Höhe | Hinweise |
|---|---|---|
| **Klebepad** | 2 m | Im Lieferumfang enthalten; reversibel |
| **Magnethalterung** | 2 m | Ideal für Neupositionierung beim Testen |
| **Schrauben/Dübel** | Keine Begrenzung | Pflicht ab 2 m Montagehöhe |

> **Tipp:** Während der Erstinstallation und Testphase die Magnethalterung verwenden. Erst nach Validierung der Abdeckung dauerhaft befestigen.

### 2.2 Höhenempfehlungen

| Höhe | Anforderung |
|---|---|
| **1,4 – 1,8 m** | Optimal für Wand-/Eckmontage |
| **~2,0 m** | Sensor leicht nach unten in Richtung Aktivitätszone neigen |
| **> 2,0 m** | Schrauben/Dübel zwingend erforderlich; Winkel muss Höhe kompensieren |
| **Decke** | Möglich, aber reduzierte Reichweite; nicht empfohlen für Räume > 15 m² |

### 2.3 Positionierungsstrategie

**Eckmontage wird gegenüber der Wandmontage bevorzugt.** Eine Eckposition mit dem Sensor in Richtung Raummitte bietet die breiteste Abdeckung und minimiert blinde Zonen an den Raumrändern.

**Folgende Positionen vermeiden:**
- In der Nähe von Klimaanlagen oder Lüftungsöffnungen
- In der Nähe von Deckenventilátoren oder Luftreinigern
- Nahe großer Metall- oder Glasflächen (Reflexionen verursachen Fehlauslösungen)
- Positionen, bei denen der Erfassungskegel durch Wände in benachbarte Räume reicht

### 2.4 Nahbereichs-Totzone

Die Erkennung unter **1 m** vor dem Sensor ist unzuverlässig. Dies bei der Positionierung berücksichtigen — den Sensor nicht dort platzieren, wo sich Personen regelmäßig direkt davor aufhalten.

### 2.5 Empfehlung vor der endgültigen Montage

Vor der dauerhaften Befestigung den Sensor mit Malerkrepp oder der Magnethalterung für **24–48 Stunden** testen. So lassen sich blinde Flecken und Störquellen erkennen, bevor die endgültige Position festgelegt wird.

---

## 3. Kopplung mit Zigbee2MQTT

1. In der Z2M-Oberfläche auf **„Verbindung erlauben"** klicken
2. Den Sensorknopf **5 Sekunden** gedrückt halten, bis die LED blinkt — dies löst den Kopplungsmodus aus
3. Das Gerät erscheint in Z2M als **`Aqara PS-S04D`**
4. Gerät in der Z2M-Oberfläche umbenennen (z. B. `fp300_wohnzimmer`)
5. Im Tab **Exposes** prüfen, ob alle Parameter angezeigt werden: `presence`, `temperature`, `humidity`, `illuminance`, `detection_range` usw.

---

## 4. Konfiguration — Richtige Reihenfolge

> ⚠️ **Alle Einstellungen immer vor dem Starten des Spatial Learning vornehmen.** Der Sensor kalibriert seine Leerbasis anhand der aktiven Konfiguration. Werden Einstellungen nach dem Learning geändert, verschlechtert sich die Genauigkeit und eine neue Lernsitzung ist erforderlich.

---

### Wichtig: Verhalten der Z2M-Oberfläche verstehen

**Der FP300 ist ein batteriebetriebenes Gerät im Schlafmodus.** Wenn Sie einen Parameterwert im Exposes-Tab von Z2M ändern, wird die Änderung **sofort gesendet**, aber der Sensor verarbeitet sie erst beim nächsten Aufwachen — das kann je nach Raumaktivität **mehrere Minuten** dauern.

**So erzwingen Sie sofortiges Feedback nach Einstellungsänderungen:**
1. Zum Sensor gehen
2. **Knopf einmal drücken** (kurzer Druck)
3. Das Gerät wacht sofort auf und verarbeitet alle ausstehenden Befehle innerhalb weniger Sekunden

Die Z2M-Oberfläche aktualisiert sich automatisch, sobald der Sensor zurückmeldet. Ein „Anwenden"-Button ist nicht erforderlich — jeder Schalter/jede Auswahlliste sendet sofort ihren eigenen Befehl.

---

### Schritt 1 — Erkennungsmodus festlegen

| Z2M-Parameter | Empfohlener Wert | Hinweise |
|---|---|---|
| `presence_detection_options` | `mmwave` | Eliminiert PIR-bedingte Fehlauslösungen |
| `pir_detection_interval` | `60` Sek. | Nur relevant bei Modus `both`; begrenzt PIR-Wiederauslöserate |

`both` nur verwenden, wenn eine sehr schnelle Lichtreaktion (< 1 Sek.) zwingend erforderlich ist. In allen anderen Fällen liefert `mmwave` zuverlässigere Anwesenheitsdaten.

---

### Schritt 2 — Empfindlichkeit konfigurieren

| Z2M-Parameter | Wert | Hinweise |
|---|---|---|
| `motion_sensitivity` | Siehe Raumprofile unten | **Steuert mmWave-Radar-Empfindlichkeit** (nicht PIR) — grundlegende Erkennungseinstellung |
| `ai_sensitivity_adaptive` | `ON` | Ermöglicht automatische Selbstoptimierung über Zeit |

> **Hinweis zur Parameterbenennung:** Trotz des Namens `motion_sensitivity` steuert dieser Parameter die **mmWave-Radar-Empfindlichkeit** für die Erkennung statischer Anwesenheit. Er bleibt der primäre Einstellparameter, auch wenn `presence_detection_options` auf `mmwave` gesetzt ist.

---

### Schritt 3 — Abwesenheitsverzögerung einstellen

| Z2M-Parameter | Bereich | Hinweise |
|---|---|---|
| `absence_delay_timer` | 10 – 300 Sek. | Siehe Raumprofile unten |

Dieser Timer legt fest, wie lange nach der letzten erkannten Bewegung der Sensor wartet, bevor er den Raum als leer meldet. Ein zu kurzer Wert führt zu Fehlmeldungen bei ruhig sitzenden Personen.

---

### Schritt 4 — Erfassungsreichweite einschränken

Alle 24 `detection_range_X`-Bänder (je **0,25 m**) sind standardmäßig auf `true` gesetzt, d. h. der Sensor erfasst den vollen Bereich von 0–6 m — einschließlich durch Wände. Die Reichweite auf die tatsächliche Raumtiefe beschränken.

**Berechnung:**
- Maximale Raumtiefe (in Metern) durch 0,25 dividieren → Anzahl der zu aktivierenden Bänder
- `detection_range_0` bis `detection_range_3` immer deaktivieren (0–1 m Totzone nahe der Montagewand)
- Alle Bänder jenseits der Raumgrenze deaktivieren

**Beispiel — 4 m Raumtiefe, Sensor in der Ecke:**

| Bänder | Bereich | Wert |
|---|---|---|
| `detection_range_0` → `_3` | 0 – 1 m | `false` (Totzone) |
| `detection_range_4` → `_15` | 1 – 4 m | `true` |
| `detection_range_16` → `_23` | 4 – 6 m | `false` (außerhalb Raum) |

---

### Schritt 5 — KI-Störquellenerkennung aktivieren

| Z2M-Parameter | Wert | Hinweise |
|---|---|---|
| `ai_interference_source_selfidentification` | `ON` | Lernt Ventilatoren, Klimaanlage, Konvektion zu ignorieren |

---

### Schritt 6 — Spatial Learning durchführen

Dies ist immer der **letzte Schritt**, nach dem Speichern aller anderen Einstellungen.

1. **Raum vollständig leeren**
2. In der Z2M-Oberfläche → Tab Exposes → `spatial_learning` → **„Start Learning"** auswählen
3. Den Raum für mindestens **60 Sekunden** verlassen
4. In der Oberfläche oder im Log erscheint keine Bestätigung — dies ist erwartetes Verhalten
5. Der Sensor setzt autonomes Hintergrund-Learning nach der initialen Auslösung dauerhaft fort

> Wurde der Raum während früherer Lernsitzungen nicht vollständig geleert, sind diese Baselines kontaminiert. Jede neue Sitzung überschreibt die vorherige vollständig.

---

## 5. Raumprofile

### 🛋️ Wohnzimmer

Szenario: Lange Aufenthaltszeiten, oft sitzend und ruhig; Fernseher, Ventilator oder Klimaanlage möglich.

| Parameter | Wert |
|---|---|
| `presence_detection_options` | `mmwave` |
| `motion_sensitivity` | `medium` |
| `ai_sensitivity_adaptive` | `ON` |
| `ai_interference_source_selfidentification` | `ON` |
| `absence_delay_timer` | `90` Sek. |
| Erkennungsreichweite | Auf Sitzbereich beschränken (typisch 3–5 m) |
| Montageposition | Ecke, 1,4–1,8 m, auf Sofa/Sitzbereich ausgerichtet |

---

### 🛏️ Schlafzimmer

Szenario: Schlafende Person mit minimaler Bewegung; Fehlmeldungen „abwesend" während des Schlafs unbedingt vermeiden.

| Parameter | Wert |
|---|---|
| `presence_detection_options` | `mmwave` |
| `motion_sensitivity` | `high` |
| `ai_sensitivity_adaptive` | `ON` |
| `ai_interference_source_selfidentification` | `ON` |
| `absence_delay_timer` | `180` Sek. |
| Erkennungsreichweite | Auf Bettdistanz beschränken (typisch 2–4 m) |
| Montageposition | Ecke, 1,4–1,7 m, direkt auf das Bett ausgerichtet |

> **Hinweis:** Der FP300 unterstützt **keine** Schlafüberwachung (Atem-/Mikrobewegungserkennung). Diese Funktion ist ausschließlich dem Aqara FP2 vorbehalten. Hohe Empfindlichkeit ermöglicht die Erkennung sehr ruhiger Personen, ist aber kein Ersatz für Schlafüberwachung.

---

### 🚿 Bad / WC

Szenario: Kurze Aufenthalte; schnelles Ein-/Ausschalten erforderlich; Feuchtigkeitssensor für Lüfterautomatisierung nutzbar.

| Parameter | Wert |
|---|---|
| `presence_detection_options` | `both` |
| `motion_sensitivity` | `low` |
| `ai_sensitivity_adaptive` | `ON` |
| `ai_interference_source_selfidentification` | `ON` |
| `absence_delay_timer` | `30` Sek. |
| Erkennungsreichweite | Eng — auf Raumtiefe begrenzen (typisch 1–2,5 m) |
| Montageposition | Hohe Ecke über Tür, in Rauminneres ausgerichtet |

> **Bonus:** Den integrierten Feuchtigkeitssensor nutzen, um einen Abluftventilator bei `humidity > 70 %` zu aktivieren — unabhängig von der Anwesenheitserkennung.

---

### 🚶 Flur / Korridor

Szenario: Nur kurzer Durchgang; hohes Risiko von Durchwand-Erkennung in Nachbarräume.

| Parameter | Wert |
|---|---|
| `presence_detection_options` | `both` |
| `motion_sensitivity` | `low` |
| `ai_sensitivity_adaptive` | `ON` |
| `ai_interference_source_selfidentification` | `OFF` |
| `absence_delay_timer` | `15` Sek. |
| Erkennungsreichweite | Exakt auf Korridorlänge begrenzen; kein Puffer |
| Montageposition | Stirnwand entlang der Korridorlänge oder hohe Ecke |

---

### 💼 Homeoffice / Arbeitszimmer

Szenario: Einzelperson am Schreibtisch über längere Zeit; Lichtautomatisierung üblich.

| Parameter | Wert |
|---|---|
| `presence_detection_options` | `mmwave` |
| `motion_sensitivity` | `medium` |
| `ai_sensitivity_adaptive` | `ON` |
| `ai_interference_source_selfidentification` | `ON` |
| `absence_delay_timer` | `60` Sek. |
| Erkennungsreichweite | Auf Schreibtischabstand beschränken (typisch 1,5–3 m) |
| Montageposition | Ecke neben oder über dem Schreibtisch, auf Arbeitsplatz ausgerichtet |

---

## 6. Empfindlichkeits-Schnellreferenz

| Raumtiefe | Empfohlene Empfindlichkeit |
|---|---|
| < 4 m | `low` |
| 4 – 7 m | `medium` |
| > 7 m | `high` |

---

## 7. Der Knopf

| Aktion | Funktion |
|---|---|
| **Einmal kurz drücken** | Erzwingt sofortiges Aufwachen; verarbeitet ausstehende Z2M-Befehle sofort |
| **5+ Sekunden halten** | Werksreset / Neu-Kopplung (auch zum Wechsel Zigbee ↔ Thread) |
| **10x kurz drücken** | Erzwingt Netzwerk-Wiederverbindung |

> **Tipp:** Nach Änderungen in Z2M den Knopf einmal drücken, um sofortiges Feedback zu erzwingen, anstatt mehrere Minuten zu warten, bis das Gerät von selbst aufwacht.

---

## 8. Fehlerbehebung

| Symptom | Wahrscheinliche Ursache | Lösung |
|---|---|---|
| Geistererkennungen nachts | PIR reagiert auf Temperaturveränderungen oder Zugluft | Auf `mmwave` wechseln; `ai_interference_source_selfidentification` aktivieren |
| Fehlmeldungen „abwesend" beim ruhigen Sitzen | Empfindlichkeit zu niedrig oder Timer zu kurz | `motion_sensitivity` erhöhen; `absence_delay_timer` auf ≥ 60 Sek. setzen |
| Durchwand-Erkennungen | Erkennungsreichweite nicht eingeschränkt | `detection_range`-Bänder jenseits der Raumgrenze deaktivieren |
| Dauernde Fehlerkennungen nach vollständiger Einrichtung | Spatial Learning mit besetztem Raum durchgeführt | Spatial Learning mit vollständig leerem Raum wiederholen |
| Keine Erkennung nahe am Sensor | Nahbereichs-Totzone (< 1 m) | Sensor neu positionieren; `detection_range_0` – `_3` deaktiviert lassen |
| Sensor meldet sofort „abwesend" nach dem Verlassen | `absence_delay_timer` zu kurz | Auf raumtypgerechten Wert erhöhen |
| Z2M-Einstellungen aktualisieren sich nicht in der Oberfläche | Gerät schläft und hat Befehl noch nicht verarbeitet | Knopf einmal drücken, um sofortiges Aufwachen und Verarbeitung zu erzwingen |
