# Aragon System MCP: Deinen Maestro von einem KI-Assistenten prüfen lassen

Mit dem Aragon System MCP kann ein KI-Assistent wie Claude in Deinen Aragon
Maestro hineinschauen und herausfinden, warum etwas nicht funktioniert: ein
Satellit, der nicht reagiert, Sprachbefehle, die nichts bewirken, ein
langsames System nach einem Update. Du beschreibst das Problem in Deinen
eigenen Worten; der Assistent prüft den Maestro selbst und erklärt, was er
findet.

Er liest nur. Er ändert nie eine Einstellung, startet nichts neu und schaltet
kein Gerät. Er ist aus, bis Du ihn einschaltest, und schaltet sich nach
48 Stunden selbst wieder aus.

## Was er prüfen kann

| Bereich | Was der Assistent sehen kann |
| ----------- | ------------------------------------------------------------ |
| Zustand | Die vollständige Zustandsprüfung des Maestro: Pakete, Dienste, Ports, Sprachmodelle, der KI-Assistent, Cloud-KI, Speicherplatz und Arbeitsspeicher |
| Fehler | Warnungen und Fehler in den Protokollen jedes Aragon-Dienstes, jeweils mit dem Zeitpunkt |
| Änderungen | Sprachwechsel, Sprachtraining, gespeicherte Cloud-KI-Einstellungen, Neustarts (und was sie ausgelöst hat), Anmeldungen, das letzte Update und was es geändert hat |
| Leistung | CPU, Arbeitsspeicher, Auslagerung und Temperatur der letzten Stunden, und welches Programm eine Spitze verursacht hat |
| Sprache | Wie viele Sprachbefehle an die Cloud-KI gingen, wie schnell die Antworten kamen, Wiederholungen (nur Zahlen) |
| KNX-Gateway | Gateway-Typ und Adresse, ob die Verbindung funktioniert, die Geräte und Räume, die Aragon davon geladen hat |
| Satelliten | Raum, Aktivierungswort, Firmware und Netzwerkverbindung jedes Satelliten; Satelliten, die der Maestro nicht findet |
| Anleitungen | Aragons eigene Anleitungen und Videos, damit die Antwort Dich auf die richtige Seite verweisen kann |

## Was geteilt wird, und was nie

Der Assistent sieht Systeminformationen: Zustände, Versionen, Einstellungen
ohne ihre geheimen Werte, und Protokollzeilen. Über Claude gehen diese
Informationen an Anthropic; über einen anderen Assistenten an dessen
Anbieter; im Aragon-Chat an den Cloud-KI-Dienst.

Nie geteilt werden:

- **Passwörter und Schlüssel.** Sie werden aus allem entfernt, was der
  Connector zurückgibt, auch Benutzername und Passwort Deines KNX-Gateways.
- **Was zu Hause gesagt wurde.** Sprache wird nur als Anzahl und Zeiten
  gemeldet, nie als Text.
- **Aufnahmen.** Der Connector hat keinen Zugriff auf Audio.

## Was Du vor dem Start brauchst

- **Remote Connect eingerichtet** auf Deinem Maestro, mit einem
  Eigentümer-Passwort. Es ist dasselbe Passwort wie für Aragon Control MCP.
  Falls Du es noch nicht eingerichtet hast, lies [Deinen KI-Assistenten mit
  Aragon Control MCP verbinden](mcp-client-setup-guide.de.md).
- **Einen KI-Assistenten, der benutzerdefinierte Connectors unterstützt**,
  zum Beispiel Claude (claude.ai oder Claude Desktop) oder ChatGPT mit
  eingeschaltetem Developer mode.

## Schritt 1: Einschalten

1. Öffne die Maestro-Seite und wähle im Menü **System → Support-Zugang**
   (der Abschnitt steht unter Fernverbindung).
2. Klicke auf **Für 48 Stunden einschalten** und gib Dein Eigentümer-Passwort
   ein.
3. Der Abschnitt zeigt, bis wann der Zugang eingeschaltet ist, und Deine
   Connector-Adresse mit einer Kopieren-Schaltfläche.

Mit **Jetzt ausschalten** kannst Du ihn jederzeit ausschalten. Der Assistent
verliert dann sofort den Zugriff. Erneutes Einschalten startet ein neues
48-Stunden-Fenster und meldet jeden vorher verbundenen Assistenten ab: Er muss
sich neu anmelden. Eine Änderung des Eigentümer-Passworts im Abschnitt
Aragon Control MCP bewirkt dasselbe.

## Schritt 2: Deinen Assistenten verbinden

Deine Connector-Adresse endet auf `/aragon-system/mcp`:

```
https://dein-geraetename.dein-tailnet.ts.net/aragon-system/mcp
```

Kopiere sie aus dem Abschnitt Support-Zugang. Das fehlende `/mcp` am Ende ist
der häufigste Einrichtungsfehler.

**Claude (claude.ai und Claude Desktop)**

1. Öffne **Einstellungen → Connectors → Benutzerdefinierten Connector
   hinzufügen** und gib ihm einen Namen, zum Beispiel "Aragon System".
2. **Server-URL:** Deine Adresse, mit `/mcp` am Ende.
3. **OAuth-Client:** Wähle **"Keine Client-ID — automatisch registrieren"**.
   Wähle **nicht** "Anthropics gehostete Client-Metadaten verwenden".
4. Lass **Zusätzliche Anfrage-Header** leer und sende ab.
5. Gib auf der Anmeldeseite **Aragon Maestro** Dein **Eigentümer-Passwort**
   ein (nicht Dein Claude-Passwort). Du wirst zu Claude zurückgeleitet und
   bist verbunden.

**ChatGPT:** Schalte in **Settings → Security and login** den **Developer
mode** ein und füge dann einen Connector mit derselben Adresse und
automatischer OAuth-Registrierung hinzu.

Aragon Control MCP und der System MCP sind zwei getrennte Connectors mit zwei
Adressen. Du kannst beide haben: Der eine steuert Dein Zuhause, der andere
prüft es.

## Schritt 3: Deine Frage stellen

Starte einen neuen Chat, achte darauf, dass der Connector in diesem Chat
eingeschaltet ist, und beschreibe das Problem so, wie Du es einem Menschen
erklären würdest. Zum Beispiel:

- "Mein Maestro reagiert nicht mehr auf Sprachbefehle. Finde heraus, warum,
  und sag mir, was Du geprüft hast."
- "Der Satellit im Wohnzimmer reagiert nicht. Was ist los?"
- "Sprachbefehle schalten kein Licht. Ist mein KNX-Gateway verbunden?"
- "Seit dem letzten Update ist etwas kaputt. Was hat sich geändert?"
- "War gestern zwischen 18:00 und 22:00 Uhr etwas Ungewöhnliches auf meinem
  Maestro?"
- "Wie ändere ich das Aktivierungswort eines Satelliten?"

Der Assistent wählt selbst, welche Prüfungen er ausführt. Gute Antworten
sagen, was geprüft wurde, was normal ist und was nicht, und verweisen auf die
passende Aragon-Anleitung.

**Jede Prüfung vor dem Ausführen sehen:** Weil die Prüfungen nur lesen, darf
Claude sie ohne Nachfrage ausführen. Wenn Du jede einzeln freigeben möchtest,
öffne die Einstellungen des Connectors in Claude und stelle die gewünschten
Werkzeuge, zum Beispiel **Recent errors** und **Support report**, auf
**Genehmigung erforderlich** (in der englischen Oberfläche **Needs approval**).

## Im Aragon-Chat (mit Cloud-KI)

Dieselben Prüfungen funktionieren auch auf der Chat-Seite des Maestro, ohne
Claude, wenn die Cloud-KI eingeschaltet ist. Das hängt nicht vom Schalter
Support-Zugang ab und braucht kein Remote Connect.

1. Öffne den Chat über das Maestro-Menü und dann **MCP-Server**.
2. Schalte **Aragon System** ein und starte ein **Neues Gespräch**.
3. Stelle Deine Frage wie in Schritt 3.

Solange es eingeschaltet ist, kostet jede Nachricht an die Cloud-KI etwas
mehr, weil die Beschreibungen der Prüfungen mit jeder Nachricht mitgehen. Zum
Beenden schalte es aus und starte ein neues Gespräch; wenn Du den Chat über
das Maestro-Menü öffnest, ist es wieder aus. Solange die Prüfungen angeboten
werden, kann der Chat keine Webseiten öffnen.

## Einen Bericht an den Aragon-Support senden

Bitte den Assistenten: *"Erstelle einen Support-Bericht und gib ihn mir als
Text zum Senden an den Aragon-Support."* Der Bericht enthält dieselben Fakten,
die der Assistent gesehen hat, mit Datum und Uhrzeit, sodass der Support genau
sieht, was geprüft wurde. Erstelle nach einer Korrektur einen zweiten Bericht,
um zu zeigen, was sich geändert hat.

## Wenn die Verbindung nicht klappt

- **"Verbindung nicht möglich" oder "Dienst nicht verfügbar":** Der
  Support-Zugang ist aus, oder seine 48 Stunden sind abgelaufen. Schalte ihn
  auf der Maestro-Seite wieder ein und verbinde neu.
- **Die Verbindung hat funktioniert und verlangt jetzt eine neue Anmeldung:**
  Der Support-Zugang wurde erneut eingeschaltet oder das Eigentümer-Passwort
  geändert, was frühere Verbindungen abmeldet. Melde Dich mit Deinem
  Eigentümer-Passwort an.
- **Neu verbinden hilft nicht:** Entferne den Connector in Claude und füge
  ihn erneut hinzu.
- **Der Abschnitt meldet "Remote Connect ist nicht verbunden":** Der Maestro
  ist gerade nicht mit Tailscale verbunden. Der Support-Zugang funktioniert
  von selbst wieder, sobald Remote Connect verbunden ist; Aus- und
  Einschalten ist nicht nötig.
- **Die Adresse antwortet überhaupt nicht:** Remote Connect muss eingerichtet
  und erreichbar sein. Prüfe den Status unter Fernverbindung auf der
  Maestro-Seite.

## Grenzen

- Der Assistent erklärt; er repariert nicht. Die Änderung machst Du oder der
  Aragon-Support.
- Er kann keinen Maestro prüfen, der ausgeschaltet ist oder keine
  Internetverbindung hat.
- Er sieht das KNX-Gateway, die Satelliten und den Node-RED-Flow von der
  Seite des Maestro: ob sie antworten und was sie melden, nicht ihre eigenen
  internen Einstellungen.
