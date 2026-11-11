# Aragon Maestro: Anleitung zur Benutzeroberfläche

Diese Anleitung erklärt die Webseite des Maestro: was jeder Menüpunkt und
jeder Abschnitt macht und wo Du ihn findest. Sie ist für Dich geschrieben und
für KI-Assistenten, die Fragen zu Aragon beantworten. Die **fett**
geschriebenen Namen sind die Wörter, die auf der deutschen Seite stehen; die
Seite gibt es auch auf Englisch und Französisch (Menü **SPRACHE**).

Sie beschreibt die Seite von Aragon Maestro 2.6.4 und wird aktualisiert, wenn
sich die Seite ändert.

## Die Seite öffnen

| Wo Du bist | Was Du öffnest |
| ------------- | ------------------------------ |
| Zu Hause | Gib im Browser `find.heyaragon.com` ein und trage Dein Netzwerk ein (zum Beispiel `192.168.1.`). Die Suche zeigt den Maestro; klicke ihn an. Du kannst auch direkt die IP-Adresse des Maestro eingeben. |
| Unterwegs | Wenn der **Fernzugriff** eingerichtet ist (Menü **SYSTEM**), öffne die Tailscale-Adresse des Maestro. |

Die Seite hat oben eine Menüleiste und darunter eine lange Seite.
Menüpunkte springen zu einem Abschnitt der Seite, öffnen ein Dialogfenster
oder öffnen eine eigene Seite in einem neuen Tab.

## Die Menüleiste

### SPRACHSTEUERUNG

| Menüpunkt | Was er macht |
| ------------- | ------------------------------ |
| **Sehen was verstanden wird** | Öffnet eine Seite, die zeigt, was Aragon gehört und wie es das verstanden hat. Hilfreich, wenn ein Befehl nicht ausgeführt wird. |
| **Sprachbefehle-Anleitung** | Springt zum Abschnitt **VERWENDEN**: Beispielbefehle für Licht, Rollladen, Szenen, Musik und Superszenen, mit kurzen Videos. |
| **KI-Assistent (Fragen & Antworten)** | Springt zu den Fragen, Aufgaben und Erinnerungen, die Du dem eingebauten KI-Assistenten stellen kannst. |
| **Trainieren neuer Wörter** | Bringt Aragon die Namen Deiner Räume, Geräte und Szenen bei. Starte es, nachdem Du im Gateway einen Raum oder ein Gerät hinzugefügt oder umbenannt hast. Es dauert ein bis zwei Minuten. SONOS-Favoriten können dazugenommen werden. |
| **Problembehebung** | Springt zum Abschnitt **PROBLEMBEHEBUNG**. |
| **Neustart der Sprachroutine** | Startet die Sprachanwendung neu, ohne den Maestro neu zu starten. Das Erste, was Du versuchen solltest, wenn die Sprachsteuerung nicht mehr reagiert. |
| **Schnell oder gemächlich** | Legt fest, wie lange Aragon wartet, bis Du fertig gesprochen hast (**Verstehgeschwindigkeit**). Empfohlen: erfahren 0-1, durchschnittlich 1-3, Anfänger 2-4. |
| **Erkennung feinjustieren** | Legt den **Kaldi Konfidenz-Schwellenwert** der eingebauten Spracherkennung fest (0,8-0,85 empfohlen; niedriger ist schneller, höher ist genauer). Nicht in Gebrauch, solange **Immer die Cloud-KI verwenden** eingeschaltet ist. |

### AUTOMATISIERUNG

| Menüpunkt | Was er macht |
| ------------- | ------------------------------ |
| **Superszenen** | Springt zum Abschnitt **SUPER SZENEN** (Guten Morgen, Gute Nacht, Ich bin zu Hause, Ich verlasse das Haus). |
| **Automations-Regeln** | Springt zum Abschnitt **AUTOMATIONS-REGELN**: zeitgesteuerte Regeln in normaler Sprache. |
| **Anwesenheitsüberwachung** | Öffnet die Seite der Anwesenheitsüberwachung in einem neuen Tab. |
| **Diagramme erkannter Geräusche** | Öffnet die Diagramme der Geräusche, die die Satelliten erkannt haben (Node-RED-Dashboard). |

### EINRICHTUNG

| Menüpunkt | Was er macht |
| ------------- | ------------------------------ |
| **Gateway Konfiguration** | Öffnet den Dialog für Dein Smart-Home-Gateway: seine IP-Adresse (**Gefundene Gateways** zeigt die gefundenen), Benutzername und Passwort, wo nötig, und den **Status** der Verbindung. Nach einer Änderung kann es bis zu zwei Minuten dauern. |
| **Satelliten** | Springt zum Abschnitt **SATELLITEN**. |
| **Auswahl der Stimme** | Öffnet den Dialog, in dem Du die Stimme des Maestro wählst. **Sag es!** spielt eine Probe ab. Option: gesprochene Antworten über den SONOS-Lautsprecher im selben Raum ausgeben. |

### NODE-RED

| Menüpunkt | Was er macht |
| ------------- | ------------------------------ |
| **Öffne flow editor** | Öffnet den Node-RED-Editor in einem neuen Tab. |
| **Öffne Dashboard** | Öffnet das Node-RED-Dashboard. |
| **Node-RED library GitHub** | Öffnet Aragons Sammlung von Node-RED-Beispielen. |
| **Reset Node-RED** | Startet Node-RED neu (**Neustart**) oder installiert es neu (**Harter Reset**). Ein harter Reset kann außerdem alle Einstellungen außer dem Gateway löschen. |
| **Lege das Node-RED Password fest** | Setzt das Node-RED-Passwort auf das Werkspasswort zurück. |

### SYSTEM

| Menüpunkt | Was er macht |
| ------------- | ------------------------------ |
| **Über & Updates** | Öffnet die Seite mit den Softwareversionen und Updates (siehe unten). |
| **Fernverbindung** | Springt zum Abschnitt **FERNZUGRIFF**. |
| **Aragon Control MCP** | Springt zum Abschnitt **ARAGON CONTROL MCP**. |
| **Support-Zugang** | Springt zum Abschnitt **SUPPORT-ZUGANG**. |
| **Cloud-KI** | Springt zum Abschnitt **CLOUD-KI**. |
| **Aragon KI-Chat** | Öffnet den Aragon-Chat in einem neuen Tab. Mit eingeschalteter Cloud-KI kann der Chat auch Geräte schalten und, wenn unter **MCP Servers** eingeschaltet, den Maestro prüfen. |
| **Talk to Home** | Öffnet eine Seite, auf der Du einen Befehl eintippst oder sprichst (Spracheingabe in Chrome, Safari und Samsung Internet). Sie zeigt, wie der Befehl verstanden wurde. |
| **Vikunja To-Do App** | Öffnet die App für Aufgaben und Erinnerungen. |
| **Netdata Dashboard** | Öffnet Live-Diagramme von Prozessor, Arbeitsspeicher und Netzwerk des Maestro. |
| **Log-Dateien** | Zeigt die Log-Dateien des Maestro. |
| **Neustarten Aragon** | Startet den Maestro neu, nachdem Du bestätigt hast. Es dauert ein paar Minuten, bis Aragon wieder bereit ist. |
| **Ausschalten** | Fährt den Maestro herunter, nachdem Du bestätigt hast. Warte, bis die rote Betriebs-LED (PWR) ganz aus ist (bis zu 3 Minuten), bevor Du den Stecker ziehst. |

### HILFE und SPRACHE

| Menüpunkt | Was er macht |
| ------------- | ------------------------------ |
| **Hilfe und Dokumentation** | Öffnet Aragons Anleitungen und Videos, die auf dem Maestro gespeichert sind. |
| **HeyAragon.com** | Öffnet Aragons Website. |
| **find.HeyAragon.com** | Findet Maestros in Deinem Netzwerk. |
| **SPRACHE** | English, Français oder Deutsch: die Sprache der Seite. Weicht sie von der Sprache des Sprachassistenten ab, zeigt die Seite eine Warnung, weil einige Funktionen dann nicht richtig funktionieren. |

## Die Seite von oben nach unten

| Abschnitt | Was Du dort findest |
| ------------- | ------------------------------ |
| Linke Spalte | **Über** (Privatsphäre), **Technologie** (ein Maestro und 1-10 Satelliten, WL oder PoE, alle im selben Netzwerk). |
| **START** | Hinweis: Gib Deinen Räumen und Geräten im Gateway passende Namen, damit sie per Sprache gesteuert werden können. |
| **MAESTRO** | Wie Du den Maestro installierst, dann **Konfiguration des Gateways**: die Schaltfläche **Gateway Konfiguration** und der Verbindungsstatus, **Auswahl der Stimme** und **Trainieren neuer Wörter** für neue Geräte- oder Raumnamen. |
| **SATELLITEN** | Satelliten einrichten (siehe unten). |
| **VERWENDEN** | Wie Du mit Aragon sprichst (Wakeword, kurze Pause, dann der Befehl) und die Liste der Sprachbefehle mit Videos; darunter der KI-Assistent für Fragen, Aufgaben und Erinnerungen. |
| **PROBLEMBEHEBUNG** | **Neustart der Sprachroutine**, **Sehen was verstanden wird**, **Schnell oder gemächlich**, **Erkennung feinjustieren** und **Runterladen** für die Log-Dateien. |
| **AUTOMATIONS-REGELN** | **Regeln verwalten** und **Standort-Einstellungen** (siehe unten). |
| **SUPER SZENEN** | Die vier Superszenen und **Superszenen zurücksetzen** (siehe unten). |
| **FERNZUGRIFF** | **Fernzugriff aktivieren** / **Fernzugriff deaktivieren** (Tailscale; lege vorher ein kostenloses Tailscale-Konto an) und weitere Subnetze, die von unterwegs erreichbar sein sollen (CIDR, zum Beispiel `192.168.9.0/24`), mit **Speichern**. |
| **ARAGON CONTROL MCP** | Lässt einen KI-Assistenten wie Claude oder ChatGPT Dein Zuhause steuern. Die Verbindungsadresse mit **Kopieren**, der Dienst mit **Dienst neu starten**, **Dienst starten** oder **Dienst stoppen**, die **Einrichtungsanleitung**, das **Besitzer-Passwort** (das Du auf der Anmeldeseite des Maestro eingibst, nicht Dein Claude- oder ChatGPT-Passwort) und **Adresse neu veröffentlichen**. Braucht den **Fernzugriff**. |
| **SUPPORT-ZUGANG** | Lässt einen KI-Assistenten Deinen Maestro auf Probleme prüfen; er kann nur lesen. **Für 48 Stunden einschalten** (mit dem Besitzer-Passwort), **Jetzt ausschalten**, die Connector-Adresse mit **Kopieren**, der letzte Zugriff, die Zeile für den Aragon-Chat und das **Benutzerhandbuch**. Solange er eingeschaltet ist, zeigt ein Hinweis oben auf jeder Seite das an. |
| **CLOUD-KI** | Ein KI-Dienst im Internet (Scaleway, Paris), der Sprachbefehle versteht und beantwortet: **Cloud-KI verwenden**, **Immer die Cloud-KI verwenden**, die **Einrichtungsanleitung**, der **API-Schlüssel** Deines Scaleway-Kontos und **Sprache und Stimme**: **Zu Hause gesprochene Sprache**, **Zu Hause werden auch andere Sprachen gesprochen**, **In der zu Hause gesprochenen Sprache sprechen** und die **Stimme der zu Hause gesprochenen Sprache** (**Installieren**, **Verwenden**, **Entfernen**, **Stimme testen**). |
| **ONLINE-STIMME** | Eine natürliche Stimme von KugelAudio, nach Verbrauch bezahlt: **Online-Stimme verwenden (KugelAudio)**, der **KugelAudio-API-Schlüssel**, **Stimme pro Sprache** (**Verwenden**, **Standard**) und **Testen**. Alles, was der Maestro sagt, geht dann als Text an KugelAudio. |
| **WEBSUCHE** | Suchen im Internet über Mojeek, nach Verbrauch bezahlt mit Deinem eigenen Schlüssel: **Websuche verwenden (Mojeek)** und der **Mojeek-API-Schlüssel**. |
| Fußzeile | Open-Source-Lizenzen und Kontakt. |

## Dialoge und Seiten im Einzelnen

### Satelliten

Im Abschnitt **SATELLITEN**:

1. Schließe den Satelliten an (PoE-Kabel, oder USB-Strom und Ethernet beim
   WL-Modell). Nach etwa einer Minute spielt er einen Ton.
2. Klicke auf **Satellitensuche**. Die Liste darunter zeigt die gefundenen
   Satelliten mit ihrer IP-Adresse.
3. Wähle einen Satelliten aus und klicke auf **...mehr dazu**. Der Dialog
   **Satellit Einstellungen** öffnet sich.
4. **Geräuschidentifikation**: Wähle die Räume, in denen Geräusche erkannt
   werden. Dafür braucht es mindestens einen Satelliten, dessen **Wakeword
   System** auf zentralisiert steht.

In **Satellit Einstellungen**:

| Einstellung oder Schaltfläche | Was sie macht |
| ------------- | ------------------------------ |
| **Raum** | Der Raum, in dem der Satellit steht. |
| **Lautstärke** | Lautstärke des Satelliten. |
| **Wakeword** | Das Wort, das den Satelliten aufweckt. Wähle eines, dessen Sprache zur Sprache des Satelliten passt. |
| **Wakeword System**, **Wakeword Aussprache**, **Empfindlichkeit:** | Wie das Wakeword erkannt wird. |
| **Einzelgeräte-Antwort** | Nur der erste Satellit, der Dich gehört hat, antwortet. |
| **Ton bei Aktivierung abspielen** | Ein Piepton, wenn der Satellit aufwacht: Warte darauf, bevor Du sprichst. |
| **Keine visuelle Bestätigung** | Kein blaues Licht. |
| **Verwenden der DeepFilterNet-Rauschunterdrückung** | Besseres Verstehen in lauten Räumen. |
| **Wi-Fi** | Verbindet den Satelliten mit dem WLAN (nur 2,4 GHz). |
| **Öffne die App**, **Starte die App neu**, **Starte neu**, **Ausschalten** | Öffnet die eigene Seite des Satelliten, startet seine App neu, startet ihn neu oder schaltet ihn aus (zum Wiedereinschalten Kabel ab- und wieder anstecken). |
| **Aktualisierung** | Aktualisiert die Software des Satelliten (einige Minuten, nicht unterbrechen). |
| **Wakewords erhalten** | Lädt die verfügbaren Wakewords. |
| **Audio-Test**, **Testsignal ausgeben** | Nimmt 5 Sekunden auf und spielt sie ab, oder spielt einen Testton. |

### Superszenen

Die Schaltflächen **Guten Morgen**, **Gute Nacht**, **Ich bin zu Hause** und
**Ich verlasse das Haus** öffnen je einen Dialog. Trage dort bis zu drei
Sprachbefehle ein, die ausgeführt werden, wenn Du die Superszene sagst, und
bei Bedarf eine Bedingung (**Wann**, zum Beispiel Tag oder Nacht).
**Standardwerte wiederherstellen** holt die ursprünglichen Befehle einer
Szene zurück; **Superszenen zurücksetzen** setzt alle vier zurück.

### Automations-Regeln

**Regeln verwalten** öffnet die Liste der Regeln. **Neue Regel hinzufügen**,
die Regel in normaler Sprache eintippen (zum Beispiel „Schließe alle
Rollläden um 22 Uhr am Wochenende"), auf **Prüfen** klicken, dann auf
**Speichern**. Alles, was Du per Sprache steuern kannst, kann eine Regel
sein; Regeln wiederholen sich, es sind keine einmaligen Aktionen. Für Regeln
bei Sonnenaufgang oder -untergang trage unter **Standort-Einstellungen** →
**Ändern** die nächste größere Stadt ein. Schreibe Regeln in der Sprache des
Sprachassistenten.

### Über & Updates

Die Seite, die sich über **SYSTEM** → **Über & Updates** öffnet:

| Teil | Was er macht |
| ------------- | ------------------------------ |
| **1. Aragon ist auf dem neuesten Stand** / **1. Es steht ein Update zur Verfügung** | Das Aragon-Update, mit **Update**. Starte den Maestro während eines Updates nicht neu und schalte ihn nicht aus. |
| **2. Sprachanwendung (Aktion & Logik)** | Installiert die Sprachanwendung (**Installieren**), zum Beispiel wenn eine neue Version angezeigt wird. |
| **3. Sprachassistent (Verständnis der natürlichen Sprache)** | Installiert den Sprachassistenten (**Installieren**), zum Beispiel um seine Sprache zu ändern, wenn die Seite vor unterschiedlichen Sprachen warnt. |
| **4. Gateway ändern (Fortgeschritten)** | Wechselt zu einem anderen Gateway-Typ. |
| **5. Gateway vergessen (Fortgeschritten)** | Löscht alle Gateway-Einstellungen (IP, Benutzernamen, Passwörter). |
| **Zeige/Verberge Systemaktualisierung** | **6. System-Update**: aktualisiert alle Systemdateien. Nur bei Problemen. |
| **Piper/TTS-Update anzeigen/verbergen** | Aktualisiert die Dateien der Sprachausgabe (großer Download). |
| **Zeige/Verberge Systemzeit** | Zeitzone, Netzwerkzeitsynchronisation, Datum und Uhrzeit. |
| **ZRAM Einstellungen** | Komprimierter Arbeitsspeicher als Auslagerungsspeicher. |
| **Zeige/Verberge die Lizenzen** | Die in Aragon verwendete Open-Source-Software. |
| **ZURÜCK** | Zurück zur Hauptseite. |

## Wie mache ich ...?

| Aufgabe | Wo |
| ------------- | ------------------------------ |
| Den Maestro neu starten | **SYSTEM** → **Neustarten Aragon**, dann bestätigen. Hängt nur die Sprachsteuerung, versuche zuerst **SPRACHSTEUERUNG** → **Neustart der Sprachroutine**. |
| Den Maestro ausschalten | **SYSTEM** → **Ausschalten**; den Stecker erst ziehen, wenn die rote PWR-LED aus ist. |
| Das Wakeword eines Satelliten ändern | **SATELLITEN**: **Satellitensuche**, den Satelliten auswählen, **...mehr dazu**, **Wakeword**. |
| Einen neuen Raum oder ein neues Gerät per Sprache steuern | Im Gateway benennen, dann **SPRACHSTEUERUNG** → **Trainieren neuer Wörter**. |
| Sehen, warum ein Befehl nicht verstanden wurde | **SPRACHSTEUERUNG** → **Sehen was verstanden wird**. |
| Die Stimme des Maestro ändern | **EINRICHTUNG** → **Auswahl der Stimme**. |
| Aragon aktualisieren | **SYSTEM** → **Über & Updates**. |
| Die Log-Dateien holen | **PROBLEMBEHEBUNG** → **Runterladen**, oder **SYSTEM** → **Log-Dateien**. |
| Den Maestro von unterwegs erreichen | **SYSTEM** → **Fernverbindung**. |
| Das Zuhause aus Claude oder ChatGPT steuern | **SYSTEM** → **Aragon Control MCP** und dort die **Einrichtungsanleitung**. |
| Den Maestro vom Support oder einem KI-Assistenten prüfen lassen | **SYSTEM** → **Support-Zugang** → **Für 48 Stunden einschalten**. |
| Einen KI-Dienst für Sprachbefehle nutzen | **SYSTEM** → **Cloud-KI** und dort die **Einrichtungsanleitung**. |
| Mit Aragon chatten | **SYSTEM** → **Aragon KI-Chat**. |
| Die Sprache der Seite ändern | **SPRACHE**. |
| Die Anleitungen finden | **HILFE** → **Hilfe und Dokumentation**, oder doc.heyaragon.com. |
