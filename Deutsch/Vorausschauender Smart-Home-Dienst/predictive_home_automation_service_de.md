# Vorausschauender Smart-Home-Dienst
### Technische Dokumentation

---

## 1. Zweck und Überblick

Der Vorausschauende Smart-Home-Dienst lernt, wie sich Ihr Haushalt jeden Tag durch Ihr Zuhause bewegt, und nutzt dieses Wissen, um vorherzusagen, was als Nächstes geschieht. Anstatt sich auf feste Zeitpläne oder manuelle Regeln zu verlassen, beobachtet der Dienst kontinuierlich die Belegungsmuster und erstellt ein sich weiterentwickelndes Bild Ihrer Routinen — und setzt dieses Bild dann aktiv ein.

**Der primäre Anwendungsfall dieses Dienstes ist das betreute Wohnen und die Seniorenbetreuung.** Dasselbe Belegungsmodell, das Komfort-Automatisierungen (Beleuchtung, Klimatisierung) antreibt, ermöglicht es auch, zuverlässig zwischen „dieser Raum ist gerade einfach ruhig" und „etwas könnte nicht stimmen" zu unterscheiden — eine Unterscheidung, die ein fester Bewegungs-Timeout-Alarm nicht treffen kann, ein Modell, das die tatsächlichen Rhythmen Ihres Haushalts gelernt hat, jedoch schon. §3 erläutert, wie dieses zugrunde liegende Lernen funktioniert; §6 beschreibt die Funktion für betreutes Wohnen im Detail. Der Rest dieses Dokuments gilt gleichermaßen für beide Anwendungsfälle, da dasselbe zugrunde liegende Modell beide antreibt.

Konkret bedeutet dies, dass der Dienst Fragen beantworten kann wie:

- „Bewegt sich ein älteres oder schutzbedürftiges Familienmitglied heute wie erwartet durch die Wohnung, oder war es ungewöhnlich ruhig?"
- „Ist es wahrscheinlich, dass sich in den nächsten 15 Minuten jemand im Wohnzimmer aufhält?"
- „Wird das Schlafzimmer in einer Stunde belegt sein?"

Diese Antworten werden automatisch generiert und an den Rest Ihrer Smart-Home-Plattform weitergegeben, die sie nutzen kann, um Beleuchtung, Klimatisierung, Sicherheit und Sicherheitsüberwachung mit minimalem Eingreifen Ihrerseits zu steuern.

---

## 2. Funktionsweise des Dienstes

Der Dienst arbeitet als kontinuierlicher Hintergrundprozess. Hier ist, was in jeder Phase geschieht:

**1. Sensoreingaben.** Bewegungs- und Anwesenheitssensoren in Ihrem gesamten Zuhause senden Statusaktualisierungen, sobald sie eine Änderung feststellen — eine Person, die einen Raum betritt, ihn verlässt, oder ein Raum, der leer bleibt. Jede Aktualisierung wird mit einem Zeitstempel versehen und lokal gespeichert.

**2. Erstellung einer Zeitleiste.** Der Dienst wandelt diese einzelnen Sensormesswerte in eine minutengenaue Aufzeichnung der Belegung für jeden Raum um. Meldet ein Sensor um 9:14 Uhr Anwesenheit und um 9:47 Uhr Abwesenheit, füllt der Dienst dieses 33-minütige Fenster entsprechend auf. So entsteht eine durchgehende, nachvollziehbare Historie der Nutzung jedes Raums.

**3. Erstellung von Vorhersagen aus der Historie.** Die letzten 48 Stunden der Zeitleiste jedes Raums bilden die direkte Eingabe für das Vorhersagemodell. Das Modell wurde auf wochenlangen, ähnlichen Zeitleisten aus Ihrem Zuhause trainiert, sodass seine Ausgabe implizit Muster widerspiegelt, die sich in dieser Historie wiederholt haben — zum Beispiel, ob ein Raum zu einer bestimmten Tageszeit tendenziell belegt ist. Die *Belegungsvorhersage* jedes Raums wird unabhängig erstellt — das Modell nutzt die Zeitleiste eines Raums nicht, um die Belegung eines anderen Raums vorherzusagen. (Die separate, in §6 beschriebene Sicherheitsüberwachungsfunktion betrachtet Räume tatsächlich übergreifend, jedoch zu einem anderen Zweck.)

**4. Erstellung von Vorhersagen.** Anhand des Gelernten erstellt der Dienst Wahrscheinlichkeitsschätzungen sowohl für den *aktuellen* Moment als auch für zwei zukünftige Zeithorizonte:

   - **Jetzt gerade** — ist der Raum in diesem Moment belegt? Dies steuert unmittelbare, reaktionsschnelle Aktionen wie das Einschalten eines Lichts, sobald jemand einen Raum betritt.
   - **15 Minuten** — kurzfristige Belegung (nützlich für reaktionsschnelle Beleuchtung und Klimasteuerung)
   - **1 Stunde** — mittelfristige Belegung (nützlich für Vorheizen, Vorkühlen oder Sicherheitskontrollen)

   Jede Vorhersage ist eine Zahl zwischen 0 und 1, die angibt, wie wahrscheinlich es ist, dass ein Raum belegt ist oder sein wird. Ihre Automatisierungsregeln nutzen diese Werte, um zu entscheiden, wann gehandelt werden soll.

**5. Auslösen von Automatisierungen.** Die Plattform bewertet jede Vorhersage anhand konfigurierter Schwellenwerte. Überschreitet die Belegungswahrscheinlichkeit einen Schwellenwert, wird eine Aktion ausgelöst — zum Beispiel das Hochregeln der Heizung, das Einschalten von Lichtern oder das Deaktivieren einer Sensorzone.

---

## 3. Wie der Dienst lernt

Der Dienst nutzt ein maschinelles Lernmodell — eine Mustererkennungs-Engine —, das auf der eigenen Sensorhistorie Ihres Zuhauses trainiert wird. Es ist nicht mit Wissen über Ihre Routinen vorprogrammiert; es gewinnt dieses Wissen durch Beobachtung.

**Der Lernprozess umfasst drei Phasen:**

| Phase | Wann sie gilt | Was das für Sie bedeutet |
|---|---|---|
| **Erste Schritte** | Erster Tag | Vorhersagen basieren auf allgemeinen Vorlagen, nicht auf Ihrem Zuhause. Nur zu Testzwecken verwenden; nicht für kritische Automatisierungen verlassen. |
| **Lernphase** | Tag 1 bis Tag 7 | Das Modell beginnt, Ihre tatsächlichen Daten einzubeziehen. Vorhersagen verbessern sich merklich, können aber noch schwanken. Manuelle Eingriffe werden für wichtige Aktionen empfohlen. |
| **Ausgereift** | Nach Tag 7 | Das Modell verfügt über genügend Daten, um Ihre tatsächlichen Muster widerzuspiegeln. Vollständige Automatisierung ist angemessen. |

**Ein durchgerechnetes Beispiel.** Angenommen, Ihr Homeoffice ist an Wochentagen morgens zuverlässig von etwa 8:30 Uhr bis Mittag belegt und am Wochenende leer.

- **Erste Schritte (Tag 1):** Auf die Frage „Wird das Büro um 9 Uhr belegt sein?" hat der Dienst noch keine haushaltsspezifischen Daten und greift auf eine allgemeine Vorlage zurück — er könnte etwas wie 60 % Wahrscheinlichkeit angeben, unabhängig davon, ob Dienstag oder Samstag ist, weil er das Muster Ihres Zuhauses noch gar nicht kennengelernt hat.
- **Lernphase (Tag 1–7):** Zur Mitte dieser Phase hat der Dienst bereits mehrere reale Vormittage trainiert. Eine Abfrage für 9 Uhr an einem Wochentag tendiert nun korrekt zu einem hohen Wert (z. B. 75–85 %). Eine Abfrage für 9 Uhr am *Wochenende* kann anfangs noch ungenau sein, einfach weil das Modell noch nicht genügend Samstage gesehen hat, um sich ihrer sicher zu sein — genau deshalb werden hier weiterhin manuelle Eingriffe empfohlen.
- **Ausgereift (ab Tag 7):** Der Dienst hat inzwischen mehrere vollständige Wochen erlebt. Eine Abfrage für 9 Uhr an einem Wochentag liefert zuverlässig eine hohe Wahrscheinlichkeit; eine Abfrage für 9 Uhr am Wochenende liefert zuverlässig eine niedrige. Die Unterscheidung zwischen Wochentags- und Wochenendvormittagen — nicht nur „Vormittag" im Allgemeinen — ist konkret das, was „das Lernen Ihrer Muster" bedeutet.

Ändert sich Ihre tatsächliche Routine später — etwa, weil Sie nun auch samstags im Büro arbeiten —, passt sich das Modell durch das unten beschriebene nächtliche erneute Training automatisch an, ohne dass Sie etwas neu konfigurieren müssen. Da das Trainingsfenster 14 Tage zurückblickt (also etwa zwei Vorkommen eines jeden Wochentags), kann es bei einer Änderung, die an einen bestimmten Tag gebunden ist, einige Wochen konsistenter Wiederholung dauern, bis sie vollständig berücksichtigt wird, anstatt bereits nach einem einzigen Vorkommen sichtbar zu sein.

Der Dienst ermittelt automatisch, in welcher Phase er sich befindet — Sie müssen diese Übergänge nicht konfigurieren oder auslösen.

**Anpassung im Zeitverlauf.** Sobald das Modell ausgereift ist, aktualisiert es sich weiterhin auf nächtlicher Basis selbst (standardmäßig zwischen 23 Uhr und 2 Uhr, wenn der Haushalt typischerweise inaktiv ist). Dadurch passt es sich auf natürliche Weise an Veränderungen Ihrer Routine an — einen neuen Homeoffice-Zeitplan, saisonale Aktivitätsschwankungen oder eine Veränderung in der Haushaltszusammensetzung — ganz ohne manuelle Neukonfiguration.

**Zwei Komponenten, zwei Lernstile.** Hinter den in §2 beschriebenen Vorhersagen stehen zwei unterschiedliche Bausteine des maschinellen Lernens, die auf unterschiedliche Weise lernen — es lohnt sich, diesen Unterschied zu verstehen, um zu wissen, was zu erwarten ist. Ein gemeinsames Basismodell wird jede einzelne Nacht von seinem ursprünglichen Ausgangspunkt aus neu trainiert, wobei jedes Mal die letzten 14 Tage Ihrer Daten verwendet werden. Seine Aufgabe ist es, ein allgemeines Verständnis der Belegungsmuster Ihres Zuhauses aufzubauen — es trifft nicht die endgültige Entscheidung, auf die Ihre Automatisierungen reagieren. Diese endgültige Entscheidung — die in §2 beschriebenen Vorhersagen **jetzt gerade**, **15 Minuten** und **1 Stunde** — wird von drei kleinen, spezialisierten Komponenten erzeugt, die auf diesem Fundament aufbauen, eine je Fall. Anders als das Basismodell beginnen diese drei nicht jede Nacht von vorn: Jede verfeinert ein wenig weiter, was sie in der Nacht zuvor bereits gelernt hat. In der Praxis bedeutet dies, dass alle drei dieser Vorhersagen mit zunehmender Laufzeit des Dienstes tendenziell immer präziser werden — weit über den „ausgereift"-Meilenstein in der obigen Tabelle hinaus —, einschließlich der „Jetzt gerade"-Vorhersage, die Ihre unmittelbarsten Automatisierungen steuert. Diese Aufteilung ist bewusst so gestaltet: Das größere Basismodell jede Nacht neu zu starten, verhindert, dass es über Monate hinweg fortlaufender Aktualisierungen allmählich abdriftet, während die wesentlich kleineren, darauf aufbauenden Komponenten sicher auf ihrem eigenen Fortschritt weiter aufbauen können.

---

## 4. Datenfenster und Historie

Der Dienst verwendet zwei unterschiedliche Datenfenster für zwei unterschiedliche Zwecke. Das Verständnis dieser Unterscheidung hilft dabei, realistische Erwartungen darüber zu entwickeln, was das Modell weiß und wie schnell es sich anpasst.

**Vorhersagefenster — 48 Stunden.** Bei der Erstellung jeder Vorhersage fragt der Dienst die letzten 48 Stunden an Sensormesswerten für den zu bewertenden Raum ab. Diese 48-Stunden-Zeitleiste ist die direkte Eingabe für das Vorhersagemodell, und ihre Länge bleibt unabhängig vom Vorhersagehorizont gleich — unabhängig davon, ob die Belegung jetzt gerade, in 15 Minuten oder in einer Stunde vorhergesagt wird.

**Trainingsfenster — 14 Tage.** Jeder nächtliche Trainingslauf stützt sich auf die letzten 14 Tage gespeicherter Sensorereignisse. Aus diesen 14 Tagen erzeugt die Pipeline eine große Menge an Trainingsbeispielen, indem ein 48-Stunden-Eingabefenster in Schritten von 15 Minuten über die gesamte Zeitleiste geschoben wird. Jedes einzelne Trainingsbeispiel enthält also weiterhin nur 48 Stunden Eingabekontext — das 14-Tage-Fenster dient lediglich dazu, genügend Historie für eine vielfältige und repräsentative Auswahl solcher Beispiele bereitzustellen, die mehrere Wochentags- und Wochenendzyklen abdeckt.

Diese beiden Fenster dienen unterschiedlichen Zwecken. Das **48-Stunden-Vorhersagefenster** gibt dem Modell zum Zeitpunkt der Inferenz aktuellen Kontext — was in Ihrem Zuhause in den vergangenen zwei Tagen tatsächlich geschehen ist. Das **14-Tage-Trainingsfenster** stellt sicher, dass das Modell genügend von den wöchentlichen Rhythmen Ihres Zuhauses gesehen hat, um zu verstehen, was typisch ist, ohne an länger zurückliegendes Verhalten gebunden zu sein, das Ihre aktuelle Routine möglicherweise nicht mehr widerspiegelt.

Die Datenbank speichert Sensorereignisdaten 30 Tage lang und verwirft sie danach. Über diese Aufbewahrungsfrist hinaus werden keine Rohdaten von Ereignissen aufbewahrt. (Diese 30-tägige Aufbewahrungsfrist ist getrennt vom 14-tägigen Trainingsfenster und länger als dieses — sie existiert, damit das Trainingsfenster sicher zurückblicken kann, ohne jemals an die Grenze dessen zu stoßen, was tatsächlich gespeichert ist.)

---

## 5. Datenwiederverwendung und Kontinuität

**Daten werden angesammelt, nicht zwischen Trainingszyklen verworfen.** Innerhalb des 14-tägigen Trainingsfensters können dieselben Sensorereignisse über mehrere nächtliche Trainingsläufe hinweg verwendet werden. Vor jedem Lauf macht der Dienst die vollständigen zwei Wochen verfügbarer Ereignisse erneut zugänglich — das bedeutet, dass jeder Trainingslauf von der vollständigen jüngsten Historie profitiert, nicht nur von den Ereignissen, die seit dem vorherigen Lauf eingetroffen sind.

**Wann ein erneutes Training stattfindet.** Das erneute Training wird nach einem nächtlichen Zeitplan ausgelöst, während des ruhigen Zeitfensters zwischen 23 Uhr und 2 Uhr. Es kann auch durch bedeutsame Ereignisse ausgelöst werden, etwa wenn das System von der Phase „Lernphase" in die Phase „Ausgereift" übergeht. Der Vorgang ist vollständig automatisch und erfordert keinerlei Handeln Ihrerseits.

**Urlaubsmodus — Unterdrückung von Abwesenheitsalarmen (manuell).** Wenn Sie wissen, dass Sie abwesend sein werden, können Sie den Urlaubsmodus über die Schaltfläche in der Monitor-Oberfläche aktivieren. Dies ist speziell ein **Schalter zur Unterdrückung von Abwesenheitsalarmen**. Ist er aktiviert, teilt er dem Anomalie-Detektor mit, dass eine längere Stille in Ihrem Zuhause zu erwarten ist, wodurch falsche Sicherheitsbenachrichtigungen während einer geplanten Abwesenheit verhindert werden.

Der Urlaubsmodus pausiert **nicht** das Training des Modells, unterbricht **nicht** die Datenerfassung und beeinträchtigt die Vorhersagen in keiner Weise. Der Dienst zeichnet weiterhin Sensorereignisse auf und führt sein planmäßiges nächtliches Training während Ihrer Abwesenheit ganz normal fort. Der Urlaubsmodus sollte vor Ihrer Abreise aktiviert und bei Ihrer Rückkehr wieder deaktiviert werden.

**Schutz der Trainingsqualität an ungewöhnlichen Tagen (automatisch).** Unabhängig vom Urlaubsmodus enthält die Trainings-Pipeline einen automatischen Mechanismus, der erkennt, wenn die aktuellen Daten dem Modell falsche Muster beibringen würden — zum Beispiel während eines nicht angekündigten Feiertags, einer Krankheitsphase oder eines beliebigen Zeitraums, in dem die Aktivität im Vergleich zu Ihrer normalen Routine für diesen Wochentag ungewöhnlich gering ist.

Vor jedem Trainingslauf vergleicht das System das aggregierte Aktivitätsniveau des Tages mit einem statistischen Referenzwert, der aus demselben Wochentag der vorangegangenen Wochen gebildet wird. War das Aktivitätsniveau an drei oder mehr aufeinanderfolgenden Tagen deutlich unter dem Normalwert, schließt das System, dass der Zeitraum ungewöhnlich ist, und überspringt entweder den Trainingslauf vollständig (sobald das Modell ausgereift ist) oder mischt zum Ausgleich synthetische Referenzdaten bei (während der Lernphase). Dies schützt das Modell davor zu lernen, dass ein leeres Haus die Norm sei.

Dieser Mechanismus ist vollständig getrennt vom Urlaubsmodus. Er läuft innerhalb der Trainings-Pipeline ab und liest keinerlei Informationen aus der Urlaubsmodus-Einstellung. Das Aktivieren des Urlaubsmodus löst ihn nicht aus, und er hat keine Auswirkung auf Anomalie-Alarme. Die beiden Systeme lösen unterschiedliche Probleme:

- Der **Urlaubsmodus** ist eine manuelle Steuerung, die Sie aktivieren, um Sicherheitsalarme stummzuschalten, wenn Sie wissen, dass Sie verreisen.
- Die **automatische Erkennung ungewöhnlicher Tage** ist eine Schutzfunktion im Hintergrund, die die langfristige Genauigkeit des Modells bewahrt, wenn eine ungewöhnliche Abwesenheit erkannt wird — ganz ohne manuellen Eingriff.

**Modellkonsistenz.** Das Vorhersagemodell und die zugehörigen Vorhersagekomponenten (eine je Zeithorizont) werden stets synchron gehalten. Führt ein Trainingslauf zu einem Modell, das die Qualitätsprüfungen nicht besteht, bleibt das vorherige Modell im Einsatz. Wird ein erfolgreiches Update später aus irgendeinem Grund zurückgesetzt, werden die Vorhersagekomponenten mit zurückgesetzt.

---

## 6. Datenschutz und Datenumfang

**Was erfasst wird.** Der Dienst erfasst ausschließlich Bewegungs- und Anwesenheitsereignisse von den von Ihnen konfigurierten Sensoren. Jedes Ereignis enthält eine Raumkennung, einen Zeitstempel und einen Anwesenheitsstatus (belegt oder unbelegt). Es werden keine Audio-, Video- oder personenbezogenen Daten erfasst.

**Wo Daten gespeichert werden.** Alle Sensordaten, erlernten Modelle und Konfigurationen werden lokal auf Ihrem Aragon-Maestro-Gerät gespeichert. Es wird nichts an einen Cloud-Server oder einen externen Dienst gesendet. Die Belegungshistorie Ihres Zuhauses verlässt niemals Ihr Heimnetzwerk.

**Was nicht erfasst wird.**

- Der Dienst zeichnet nicht auf, welche bestimmte Person in einem Raum anwesend ist — nur, dass der Raum belegt ist.
- Er überwacht weder den Inhalt von Gesprächen noch Aktivitäten oder Verhalten über die reine binäre Anwesenheitserkennung hinaus.
- Er stellt keine Verbindung zu Datenquellen Dritter her.

**Überwachung für betreutes Wohnen (falls aktiviert) — der primäre Anwendungsfall des Dienstes (siehe §1).** In Haushalten, in denen eine Sicherheitsfunktion für betreutes Wohnen aktiv ist, überwacht der Dienst kontinuierlich ungewöhnlich lange Phasen der Inaktivität über alle konfigurierten Räume hinweg. Was als „ungewöhnlich" gilt, ist kein fester Zeitwert — es basiert auf demselben erlernten Belegungsmodell, das in diesem gesamten Dokument beschrieben wird, weshalb das System eine wirklich besorgniserregende Stille von einem gewöhnlich ruhigen Nachmittag unterscheiden kann.

Wird eine längere Stille festgestellt — das heißt, es wurde länger als erwartet keine Sensoraktivität aufgezeichnet —, wird ein Alarm ausgelöst. Bevor ein Alarm ausgelöst wird, prüft der Dienst, ob ein *anderer* überwachter Raum echte, aktuelle Aktivität gezeigt hat; ist dies der Fall, wird der Alarm zurückgehalten, da eine an anderer Stelle im Zuhause aktive Person selbst einen Beleg für ihre Sicherheit darstellt, auch wenn ein bestimmter Raum ruhig geblieben ist. Dies reduziert Fehlalarme — zum Beispiel löst ein Raum, dessen einziger Sensor ein Lichtschalter ist, der tagsüber schlicht nicht benötigt wurde, allein keinen falschen Sicherheitsalarm aus.

Diese Funktion kann jederzeit über die Plattformeinstellungen aktiviert, deaktiviert oder pausiert werden und arbeitet — wie der Rest dieses Dokuments — ausschließlich mit lokal gespeicherten Daten, wobei jeder Alarm direkt auf dem Gerät erzeugt wird.

---

*Dokumentversion 1.1 — Angewendete Korrekturen: Aussage zu raumübergreifenden Mustern entfernt; Vorhersage- und Trainings-Datenfenster geklärt und getrennt dargestellt; Umfang des Urlaubsmodus auf reine Alarmunterdrückung korrigiert; automatische Erkennung ungewöhnlicher Tage vom manuellen Urlaubsmodus getrennt und mit explizitem Vergleich versehen.*

*Dokumentversion 1.2 — Angewendete Korrekturen: Aufbewahrungsfrist für Rohdaten von 7 auf 30 Tage korrigiert; Wiederverwendungsfenster für das Training von 7 auf 14 Tage korrigiert (und klar von der nun unterschiedlichen Aufbewahrungsfrist abgegrenzt, da beide zuvor denselben Wert hatten und dies nun nicht mehr der Fall ist); Vorhersagehorizont von 2 Stunden entfernt, da vom System nicht mehr erzeugt (die Horizonte von 15 Minuten und 1 Stunde bleiben bestehen).*

*Dokumentversion 1.3 — Angewendete Korrekturen: Die Aussage in §2 zu „keiner Beziehung zwischen Räumen" wurde auf Belegungsvorhersagen eingegrenzt, da die Sicherheitsfunktion für betreutes Wohnen nun tatsächlich raumübergreifend prüft (siehe unten); §6 wurde aktualisiert, um diese raumübergreifende Prüfung und ihren Zweck zu beschreiben (Reduzierung falscher Sicherheitsalarme). Ein konkretes durchgerechnetes Beispiel wurde §3 hinzugefügt, das veranschaulicht, was sich in jeder Lernphase ändert, und eine frühere Aussage zu einer Anpassungsgeschwindigkeit von „1–2 Wochen" wurde abgeschwächt, um keine tatsächlich nicht verifizierte Genauigkeit zu behaupten.*

*Dokumentversion 1.4 — Angewendete Korrekturen: Betreutes Wohnen / Seniorenbetreuung wurde ausdrücklich zum primären Anwendungsfall erhoben (Einleitung in §1, eine führende Beispielfrage sowie eine erweiterte Behandlung in §6 mit Rückverweis auf §1), anstatt nur knapp im Datenschutzabschnitt erwähnt zu werden. §3 wurde um eine verifizierte Erläuterung einer echten architektonischen Unterscheidung ergänzt: Ein gemeinsames Basismodell wird jede Nacht von Grund auf neu trainiert (jedes Mal mit einem 14-Tage-Fenster), während drei spezialisierte Vorhersagekomponenten das Gelernte nächtlich schrittweise akkumulieren — für die Vorhersagen „Jetzt gerade", „15 Minuten" und „1 Stunde". (Ein früherer Entwurf dieses Absatzes legte fälschlich nahe, dass „Jetzt gerade" direkt das von Grund auf neu trainierte Basismodell verwende; nach Abgleich mit dem Quellcode korrigiert, wonach diese Vorhersage von einer der drei akkumulierenden Komponenten erzeugt wird, ebenso wie die 15-Minuten- und 1-Stunden-Vorhersagen.) §2 wurde entsprechend aktualisiert, um „Jetzt gerade" ausdrücklich neben den beiden Vorhersagehorizonten aufzuführen, anstatt ausschließlich zukunftsgerichtete Vorhersagen zu beschreiben. „Home-Hub" wurde durch „Aragon-Maestro-Gerät" ersetzt, um die korrekte Produktbezeichnung zu verwenden.*

*Dokumentversion 1.5 — Ergänzung: Ein Satz wurde §3 hinzugefügt, der kurz und allgemein begründet, warum diese Aufteilung zwischen Neustart und akkumulierendem Lernen besteht, damit die Unterscheidung nicht willkürlich wirkt. Bewusst kurz gehalten und frei von Implementierungsdetails, entsprechend dem Umfang dieses Dokuments — die vollständige technische Begründung findet sich in der technischen Dokumentation.*
