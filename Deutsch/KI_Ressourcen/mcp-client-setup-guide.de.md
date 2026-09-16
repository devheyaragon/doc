# Ihren KI-Assistenten mit Aragon Control MCP verbinden

Diese Anleitung führt Sie durch das Verbinden eines KI-Assistenten — Claude,
ChatGPT oder eine andere kompatible App — mit Ihrem Aragon-Control-MCP-Server,
sodass Sie Ihr Zuhause per Sprache oder Text steuern können ("Schalte das
Licht im Wohnzimmer ein", "Schließe die Rollläden im Büro").

## Was Sie vor dem Start benötigen

- **Aragon Control MCP installiert und aktiv** auf Ihrem Aragon-Maestro-Gerät.
  Falls das noch nicht eingerichtet ist, siehe die Installationsanleitung,
  die Ihrer Software beilag.
- **Die Adresse (URL) Ihres Servers und Ihr Besitzer-Passwort.** Beides
  finden Sie im Bereich Aragon Control MCP Ihrer Aragon-Maestro-App — dort
  wird die Adresse Ihres Servers angezeigt, und Sie können dort das
  Passwort festlegen, mit dem Sie sich anmelden, wenn ein Assistent eine
  Verbindung anfragt. Dies ist nicht Ihr Claude-/ChatGPT-/etc.-Kontopasswort.

## Die eine Regel, die überall gilt

Wo immer ein Client nach einer **Server-URL** fragt, hängen Sie am Ende
Ihrer Adresse immer `/mcp` an:

```
https://ihr-gerätename.ihr-tailnet.ts.net/aragon-control/mcp
```

Das Weglassen von `/mcp` ist der häufigste Einrichtungsfehler — der
Assistent meldet dann einen Verbindungs- oder "nicht gefunden"-Fehler, der
so aussieht, als wäre etwas kaputt, obwohl nur das letzte Stück der Adresse
fehlt. Prüfen Sie das zuerst, falls eine Verbindung jemals fehlschlägt.

---

## Claude (claude.ai und Claude Desktop)

1. Öffnen Sie **Einstellungen → Connectors → Benutzerdefinierten Connector
   hinzufügen.**
2. **Server-URL:** Geben Sie Ihre Adresse mit `/mcp` am Ende ein, wie oben.
3. **Authentifizierung:** Lassen Sie die erkannte Option stehen ("Immer
   erforderlich" / "Jetzt anmelden") — hier muss nichts geändert werden.
4. **OAuth-Client:** Wählen Sie **"Keine Client-ID — automatisch
   registrieren"** (kann auch als "Automatisch registrieren" beschriftet
   sein). Wählen Sie **nicht** "Anthropics gehostete Client-Metadaten
   verwenden", auch wenn diese Option als "Empfohlen" markiert sein sollte
   — sie funktioniert mit diesem Server nicht.
5. Lassen Sie **Zusätzliche Anfrage-Header** leer.
6. Absenden. Sie werden zu einer **Aragon-Maestro**-Anmeldeseite
   weitergeleitet — geben Sie dort Ihr **Besitzer-Passwort** ein (das ist
   kein Claude-Konto-Login). Nach der Anmeldung werden Sie zurück zu Claude
   geleitet und sind verbunden.

Sobald die Verbindung steht, können Sie in normaler Sprache mit Ihrem
Assistenten sprechen — auf Englisch oder, falls Ihr Assistent das
unterstützt, in einer anderen Sprache, die Sie sprechen, da der Assistent
Ihre Anfrage übersetzt, bevor sie Ihr Maestro-System erreicht. Lassen Sie
sich Ihre Räume auflisten, den Status eines Lichts prüfen oder schalten Sie
etwas ein oder aus.

**Falls eine zuvor funktionierende Verbindung plötzlich "Verbindungsproblem"
oder "Verbindung nicht möglich" anzeigt:** Klicken Sie nicht einfach auf
Erneut verbinden — entfernen Sie stattdessen den Connector und fügen Sie ihn
komplett neu hinzu, falls ein paar Versuche mit "Erneut verbinden" nicht
helfen.

**Hinweis:** Die Anmeldeseite, zu der Sie weitergeleitet werden, trägt die
Marke "Aragon Maestro", während der Connector selbst in Ihrer
Connector-Liste als "Aragon Control MCP" bezeichnet ist. Das ist so
vorgesehen — die Anmeldeseite betrifft das Haussystem, dem Sie Zugriff
gewähren, nicht den Namen des Connectors.

**Falls Sie diesen Connector zuvor in Claude Desktop über eine manuell
bearbeitete Konfigurationsdatei eingerichtet haben** (eine ältere,
inoffizielle Methode): Entfernen Sie diesen alten Eintrag und starten Sie
Claude Desktop vollständig neu, bevor Sie den offiziellen Connector wie oben
beschrieben hinzufügen. Beide gleichzeitig zu betreiben kann zu Problemen
führen.

## ChatGPT

Benutzerdefinierte Connectors sind unter ChatGPTs **Entwicklermodus**
(Developer Mode) in den Connector-Einstellungen verfügbar. Verwenden Sie
dieselbe Server-URL (mit `/mcp`) und wählen Sie die automatische/dynamische
OAuth-Registrierung, wie oben beschrieben.

**Plan-Anforderung:** Bei einem persönlichen ChatGPT Plus- oder Pro-Plan
können benutzerdefinierte MCP-Connectors auf das Lesen des Status
beschränkt sein (z. B. Geräte auflisten), ohne dass Sie etwas ändern können
— ein Licht ein- oder auszuschalten funktioniert dann möglicherweise
stillschweigend nicht. Volle Steuerung (Lesen und Schreiben) erfordert
einen ChatGPT Business-, Enterprise- oder Edu-Plan. Wenn Befehle scheinbar
verbinden und Status korrekt lesen, aber nichts tatsächlich steuern,
prüfen Sie zuerst Ihren Plan, bevor Sie einen Fehler bei der Einrichtung
vermuten.

**Falls das Erstellen des Connectors beim ersten Versuch fehlschlägt** mit
einer Fehlermeldung wie "unterstützt keine Dynamic Client Registration":
Löschen Sie den Connector und fügen Sie ihn erneut hinzu. Es wurde
beobachtet, dass dies beim ersten Versuch fehlschlägt und beim zweiten
Versuch ohne weitere Änderung sofort funktioniert — dies scheint daran zu
liegen, dass ChatGPT selbst seinen Discovery-Prozess beim ersten Mal nicht
abschließt, nicht an einem Problem mit Ihrem Server oder Ihrer Adresse.

**Falls ein Befehl in Ihrer eigenen Sprache zunächst nicht funktioniert**
("Jag kunde inte tända lamporna just nu — anslutningen till hemstyrningen
misslyckades" / "Licht konnte gerade nicht eingeschaltet werden — die
Verbindung zur Haussteuerung ist fehlgeschlagen"), versuchen Sie dieselbe
Anfrage einmal auf Englisch, dann erneut in Ihrer eigenen Sprache. Es
wurde beobachtet, dass der erste nicht-englische Befehl fehlschlägt und
danach sofort funktioniert — auch für weitere Befehle in Ihrer eigenen
Sprache — sobald ein englischer Befehl einmal durchgegangen ist. Schlägt
ein Befehl danach weiterhin fehl, sollte das als echtes Problem behandelt
werden und nicht als dieser Aufwärmeffekt.

## Perplexity

Perplexity unterstützt benutzerdefinierte Remote-Connectors über dieselbe
Art von Verbindung wie oben (OAuth mit automatischer Client-Registrierung).

**Plan-Anforderung:** Das Einrichten eines *benutzerdefinierten*
MCP-Connectors in Perplexity erfordert ein **kostenpflichtiges
Perplexity-Konto** — auf der kostenlosen Stufe ist dies nicht verfügbar.

## Mistral Le Chat

Derzeit wird die Verbindung zu Mistral Le Chat **nicht unterstützt** — das
Hinzufügen des Connectors schlägt bei der Einrichtung mit einem Fehler
"Verbindung zum Server nicht möglich" fehl. Es wurde bestätigt, dass dies
ein Problem auf Seiten von Mistral ist, nicht bei Ihrem
Aragon-Control-MCP-Server (der Server funktioniert gleichzeitig einwandfrei
mit anderen Assistenten). Falls Sie Mistral Le Chat verwenden, schauen Sie
bitte erneut vorbei, sobald Mistral dies behoben hat, oder nutzen Sie in
der Zwischenzeit einen der oben unterstützten Assistenten.

## Grok

Laut xAIs eigener Dokumentation umfasst der **Free**-Plan von Grok
Connectors, und benutzerdefinierte MCP-Connectors können durch Eingabe
einer Server-URL hinzugefügt werden — dies erfordert also womöglich keinen
kostenpflichtigen SuperGrok-Plan, entgegen früherer Annahmen. **Dies wurde
noch nicht end-to-end mit Aragon Control MCP getestet**, betrachten Sie es
daher als unbestätigt, nicht als Garantie. Falls Sie es ausprobieren,
sollten dieselben Schritte gelten: Geben Sie Ihre Server-URL mit `/mcp` am
Ende ein und wählen Sie automatische/dynamische OAuth-Client-Registrierung.
Bitte teilen Sie uns mit, was dabei passiert, damit dieser Abschnitt mit
einem bestätigten Ergebnis aktualisiert werden kann.

## Andere Assistenten

Falls Ihr Assistent Verbindungen zu benutzerdefinierten Remote-MCP-Servern
mit OAuth 2.0/2.1 und automatischer (dynamischer) Client-Registrierung
unterstützt, sollten dieselben Schritte gelten: Geben Sie Ihre Server-URL
mit `/mcp` am Ende ein, wählen Sie automatische OAuth-Client-Registrierung,
und melden Sie sich mit Ihrem Aragon-Maestro-Besitzer-Passwort an, wenn Sie
dazu aufgefordert werden. Falls Sie mit einem hier nicht aufgeführten
Assistenten Probleme haben, kontaktieren Sie uns bitte und teilen Sie uns
mit, was passiert ist.
