# Einrichtung des Cloud-KI-Servers

Mit dem Cloud-KI-Server versteht Ihr Aragon Maestro auch Sprachbefehle, die
er selbst nicht versteht. Sie können dann frei sprechen, auch in einer
anderen Sprache als der des Maestro, und erhalten eine kurze gesprochene
Antwort. Die Befehle werden von einem KI-Dienst im Internet verstanden,
Scaleway Generative APIs, der in Paris (Frankreich) läuft. Scaleway rechnet
ihn über Ihr eigenes Konto ab.

Ohne Internet, oder solange der Dienst nicht antwortet, versteht der
Maestro die Befehle wie bisher selbst.

Die Schaltflächen der Scaleway-Konsole sind hier so genannt, wie sie in der
englischen Konsole heißen.

## Was Sie vorher brauchen

- **Ein Scaleway-Konto.** Auch Privatpersonen können eines eröffnen;
  bezahlt wird per Karte oder SEPA-Lastschrift. Registrierung unter
  [console.scaleway.com](https://console.scaleway.com).
- **Aragon Control MCP muss laufen** auf Ihrem Maestro. Der
  Cloud-KI-Server führt damit die Befehle im Haus aus. Sie finden es im
  Abschnitt **Aragon Control MCP** der Maestro-Seite. Der Fernzugriff für
  KI-Assistenten wird dafür nicht benötigt.
- **Aktuelle Maestro-Software.**

## Schritt 1 – Eigener Zugang für den Maestro (empfohlen)

Scaleway kann einer *Application* einen eigenen Zugang geben, der nur die
KI-Modelle nutzen darf. Der Schlüssel, den Sie im Maestro eingeben, kann
dann sonst nichts in Ihrem Scaleway-Konto tun.

1. Klicken Sie in der Scaleway-Konsole oben rechts im Aufklappmenü auf
   **IAM & API keys**.
2. Öffnen Sie den Reiter **Applications** und klicken Sie auf
   **Create application**.
3. Geben Sie einen Namen ein, zum Beispiel *Aragon Maestro*, und klicken
   Sie auf **Create application**.
4. Öffnen Sie den Reiter **Policies** und klicken Sie auf
   **Create policy**.
5. Geben Sie einen Namen ein, zum Beispiel *Aragon Maestro AI*, und wählen
   Sie die eben erstellte Application als **principal**.
6. Fügen Sie eine Regel hinzu: Wählen Sie das Permission Set
   **GenerativeApisModelAccess** (unter Generative APIs – Serverless),
   klicken Sie auf **Next**, wählen Sie Ihr Project als Bereich (Scope)
   und schließen Sie die Policy ab.

Sie können diesen Schritt auslassen und den Schlüssel für Ihren eigenen
Benutzer erstellen (Schritt 2, „Bearer“). Dieser Schlüssel hat dann aber
alle Rechte Ihres Benutzers.

## Schritt 2 – API-Schlüssel erstellen

1. Öffnen Sie in **IAM & API keys** den Reiter **API keys** und klicken
   Sie auf **+ Generate API key**.
2. **Bearer:** Wählen Sie die Application aus Schritt 1 (oder sich selbst).
3. **Description:** zum Beispiel *Aragon Maestro*.
4. **Expiration:** Wählen Sie, wann der Schlüssel abläuft. Ein Schlüssel
   ohne Ablaufdatum muss nie erneuert werden; ein Ablaufdatum begrenzt das
   Risiko, falls der Schlüssel einmal in falsche Hände gerät. Wenn Sie ein
   Ablaufdatum wählen, tragen Sie es in Ihren Kalender ein. Ihre
   Scaleway-Organisation kann eine Höchstdauer vorgeben.
5. **Object Storage:** Wählen Sie **No, skip for now**.
6. Klicken Sie auf **Generate API key**.
7. Kopieren Sie den **Secret Key**. Er wird nur dieses eine Mal angezeigt.
   (Der Access Key, der kürzere, der mit *SCW* beginnt, wird nicht
   benötigt.)

## Schritt 3 – Im Maestro einschalten

Öffnen Sie die Maestro-Seite und gehen Sie zum Abschnitt
**Cloud-KI-Server**.

1. **API-Schlüssel:** Fügen Sie den Secret Key ein und klicken Sie auf
   **Speichern**. Die Seite zeigt dann „Ein Schlüssel ist gesetzt.“ Der
   Schlüssel wird auf dem Maestro gespeichert und nicht wieder angezeigt.
2. **Zu Hause gesprochene Sprache:** Wählen Sie die Sprache, in der die
   meisten mit dem Maestro sprechen, und klicken Sie auf **Speichern**.
   Kurze Befehle werden dann viel besser verstanden; andere Sprachen
   funktionieren weiterhin.
3. Schalten Sie **Cloud-KI-Server verwenden** ein. Nach einem Moment zeigt
   der Status **Bereit**.
4. Optional: **Immer den Cloud-KI-Server verwenden.** Schalten Sie dies
   ein, wenn im Haushalt eine andere Sprache als die des Maestro gesprochen
   wird. Jeder Sprachbefehl geht dann an den KI-Dienst, auch solche, die der
   Maestro selbst verstanden hätte; sonst kann der Maestro fremde Wörter für
   einen Befehl halten. Die Sprachanwendung startet bei einer Änderung neu.

Probieren Sie es aus: Sagen Sie einen Befehl in eigenen Worten, zum
Beispiel „Mach es hier etwas heller“ oder „Schließe die Rollläden im
Wohnzimmer“.

## Was es kostet

Scaleway rechnet nach Nutzung ab. Preise im Oktober 2026, ohne Steuern:

- Spracherkennung (*whisper-large-v3*): 0,003 € pro Audiominute.
- Verstehen des Befehls (*qwen3-235b-a22b-instruct-2507*): 0,75 € pro
  Million Eingabe-Token und 2,25 € pro Million Ausgabe-Token.
- Kostenloses Kontingent: die ersten 1.000.000 Token und 60 Audiominuten.

Ein Sprachbefehl braucht etwa 5.000 Token und einige Sekunden Audio, das
sind rund 0,004 €. Nur die Befehle, die der Maestro nicht selbst versteht,
gehen an den Dienst; bei 50 Sprachbefehlen am Tag sind das etwa 1 € im
Monat (unsere Schätzung). Mit **Immer den Cloud-KI-Server verwenden**
zählt jeder Befehl.

Scaleway kann den Dienst nicht bei einem Höchstbetrag stoppen, aber Sie
können in der Scaleway-Konsole **Billing alerts** einrichten und werden
dann per E-Mail gewarnt. Aktuelle Preise:
[scaleway.com/en/pricing/model-as-a-service](https://www.scaleway.com/en/pricing/model-as-a-service/).

## Was an Scaleway gesendet wird

Für jeden Befehl, der an den KI-Dienst geht: die Aufnahme dessen, was Sie
gesagt haben, ihr Text und die Namen Ihrer Räume, Geräte und Szenen, damit
der KI-Dienst das richtige wählen kann. Bei Fragen zum Wetter, zu einer
Rechnung oder Ähnlichem wird auch das Ergebnis dieser Abfrage gesendet.
Der Dienst läuft in Paris (Frankreich).

## Wenn der Schlüssel nicht mehr funktioniert

Ist der Schlüssel abgelaufen oder gelöscht, zeigt der Status auf der
Maestro-Seite: „Ein, aber der KI-Dienst lehnt den API-Schlüssel ab. Er ist
möglicherweise abgelaufen.“ Der Maestro verarbeitet die Befehle dann selbst.

1. Erstellen Sie einen neuen Schlüssel wie in Schritt 2.
2. Fügen Sie ihn im Feld **API-Schlüssel** ein und klicken Sie auf
   **Speichern**. Der Schlüssel wird sofort geprüft.
3. Löschen Sie in der Scaleway-Konsole den alten Schlüssel (Reiter
   **API keys**, Menü neben dem Schlüssel, **Delete**).

## Statusmeldungen

| Status | Was zu tun ist |
|---|---|
| Bereit | Nichts. |
| Aus | **Cloud-KI-Server verwenden** einschalten. |
| Ein, aber kein API-Schlüssel gesetzt | Schlüssel eingeben (Schritt 3). |
| Ein, aber der KI-Dienst lehnt den API-Schlüssel ab | Neuen Schlüssel erstellen (siehe oben). |
| Ein, aber Aragon Control MCP läuft nicht | Im Abschnitt **Aragon Control MCP** starten. |
| Ein, aber der KI-Dienst war gerade nicht erreichbar | Internetverbindung prüfen. Der Maestro arbeitet inzwischen selbst und versucht es später erneut. |
| Antwortet nicht | Maestro neu starten. Bleibt es so, wenden Sie sich an Ihren Installateur. |
