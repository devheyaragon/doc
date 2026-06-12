# Aragon AI Assistent — FAQ Sicherheit & Datenschutz

**System:** Aragon Maestro | Modell: Qwen (via llama.cpp) | Oberfläche: llama-ui + eigene MCP-Server  
**Zielgruppe:** Kunden und Endnutzer  
**Stand:** Juni 2026

---

## Überblick

Dieses Dokument beantwortet die häufigsten Fragen von Kunden zum verwendeten KI-Modell im Aragon-Assistenten, seinem Sicherheitsprofil und den konkreten Maßnahmen zum Schutz von Nutzerdaten und Systemintegrität. Es ist auf Ehrlichkeit, Transparenz und technische Genauigkeit ausgerichtet.

---

## F1 — Welches KI-Modell verwendet Aragon, und warum wird es manchmal als „unsicher" bezeichnet?

### Das Modell

Aragon verwendet **Qwen**, eine Familie großer Sprachmodelle (LLMs), die von **Alibaba Cloud** entwickelt und als Open-Weight-Modelle unter permissiven Lizenzen veröffentlicht wurden[cite:16]. Die eingesetzte Variante läuft lokal über **llama.cpp**, eine weit verbreitete Open-Source-Inferenz-Engine.

### Was „unsicher" tatsächlich bedeutet

Der Begriff „unsicher" in der KI-Sicherheitsliteratur bedeutet **nicht**, dass das System gefährlich zu bedienen ist oder dass Ihre Daten gefährdet sind. Er bezieht sich speziell auf **Schwächen bei Inhalts-Schutzmaßnahmen (Guardrails)** — das heißt, das Modell kann unter Umständen durch adversarielle Eingaben (sogenannte „Jailbreaks") dazu gebracht werden, schädliche Texte zu erzeugen, wie Schadcode, gefährliche Anleitungen oder Betrugs-Inhalte[cite:5][cite:7].

Unabhängige Evaluierungen haben Qwen-Modelle in dieser Dimension als kritisch schwach eingestuft[cite:3]. Vergleichbare Probleme bestehen bei Modellen wie DeepSeek und in geringerem Maß bei westlichen Modellen wie GPT-4 und Llama[cite:4]. Kein kommerzielles LLM gilt heute als vollständig sicher gegenüber allen adversariellen Eingaben.

### Was das in der Praxis bedeutet

Für den Aragon-Einsatz gilt:

- Alle Modelleingaben durchlaufen eine **Prompt-Sanitisierung**, bevor sie das Modell erreichen
- Alle Modellausgaben werden über **eigene MCP-Server** geleitet, die mögliche Aktionen des Modells einschränken
- Das Modell hat **keinen Internetzugang** während der Inferenz
- Der Assistent **eignet sich nicht für kritische oder hochriskante Entscheidungen** (medizinische, rechtliche oder finanzielle Beratung mit Regulierungsanforderungen)

---

## F2 — Ist Qwen ein chinesisches Modell? Besteht das Risiko, dass meine Daten nach China übertragen werden?

### Modellherkunft vs. Datenziel

Qwen wird von Alibaba Cloud, einem chinesischen Unternehmen, entwickelt und unterliegt in seiner Cloud-Form chinesischer Rechtsprechung[cite:1]. Der Aragon-Einsatz verwendet jedoch die **Open-Weight-Version** von Qwen — die Modellgewichte werden einmalig heruntergeladen und laufen vollständig auf lokaler Infrastruktur[cite:36][cite:45].

Das bedeutet:

- Keine Anfragen, Prompts oder Antworten werden an Alibaba-Cloud-Server übermittelt
- Während der Inferenz werden keine API-Aufrufe an externe Dienste getätigt
- Alibaba hat keine technische Möglichkeit, auf Gesprächsdaten zuzugreifen

### DSGVO-Implikationen

Da die gesamte Verarbeitung lokal erfolgt, erfüllt der Einsatz **DSGVO-Artikel 25** (Datenschutz durch Technikgestaltung) und vermeidet die Pflichten aus **Artikel 44** zu Drittlandübermittlungen[cite:36]. Der Betreiber handelt als **Nutzer eines Open-Weight-Modells**, nicht als Cloud-Service-Abonnent — eine rechtlich eigenständige und risikoärmere Rolle im Sinne des EU-KI-Gesetzes.

---

## F3 — Ist das System zu 100 % sicher?

### Die ehrliche Antwort

Kein KI-System ist zu 100 % sicher — und jeder Anbieter, der das behauptet, sollte kritisch hinterfragt werden. Der Aragon-Assistent bietet starke, nachweisbare Schutzmaßnahmen, trägt aber wie alle LLM-basierten Systeme inhärente Einschränkungen, die Kunden kennen sollten.

**Was garantiert wird:**

- Nutzerdaten verlassen die lokale Infrastruktur nicht[cite:36][cite:45]
- Die Modellgewichte enthalten keinen ausführbaren Code oder versteckte Exfiltrations-Mechanismen — das GGUF-Format (verwendet von llama.cpp) ist nicht ausführbares Gewichts-Datenformat[cite:40]
- Der Zugriff auf Werkzeuge (Dateisystem, APIs, Shell-Befehle) wird ausschließlich über abgegrenzte MCP-Server kontrolliert; das System exponiert keine Befehle auf Betriebssystemebene[cite:31]
- Die Integrität der Modell-Checkpoints ist über SHA-256-Hashes verifizierbar, abgeglichen mit den offiziellen Qwen-Releases auf HuggingFace[cite:34]

**Was nicht garantiert werden kann:**

- Faktische Richtigkeit aller Antworten — LLMs können halluzinieren oder plausibel klingende, falsche Informationen produzieren
- Vollständige Resistenz gegenüber adversariellen Prompts — ein gezielter Angreifer kann unerwartete Ausgaben provozieren[cite:43]
- Eignung für kritische Entscheidungen — der Assistent ist für den allgemeinen Gebrauch konzipiert, nicht für regulierte oder sicherheitskritische Bereiche

---

## F4 — Wie können Kunden diese Sicherheitsaussagen überprüfen?

Technische Nachweise sind auf Anfrage für jede Aussage verfügbar:

| Aussage | Überprüfungsmethode |
|---|---|
| Keine ausgehende Datenübertragung | Netzwerkverkehrsaufzeichnung (Wireshark/tcpdump) während einer Live-Inferenzsitzung, ohne externe Verbindungen |
| Saubere Modellgewichte | SHA-256-Hash der GGUF-Dateien verglichen mit offiziellen Qwen-Checksums auf HuggingFace[cite:34] |
| Nicht ausführbare Gewichte | Quellcode-Review von llama.cpp, der bestätigt, dass GGUF nur interpretiert wird, ohne Shell-Ausführungspfad aus den Gewichten[cite:40] |
| Abgegrenzter MCP-Werkzeugzugriff | Review der MCP-Server-Konfiguration mit expliziten Berechtigungsgrenzen[cite:33] |
| Keine OS-Shell-Exposition | Bestätigung, dass llama.cpp nicht mit dem Flag `--tools all` betrieben wird, das `exec_shell_command` und Datei-I/O-Werkzeuge freigeben würde[cite:31] |
| Aktive Eingabe-Sanitisierung | Code-Review der Prompt-Vorverarbeitungs-Pipeline vor der Modell-Invokation |

---

## F5 — Welche Risiken bleiben bestehen und wie werden sie gehandhabt?

### Verbleibende Risiken

| Risiko | Wahrscheinlichkeit | Gegenmaßnahme |
|---|---|---|
| Halluzination (falsche, aber sichere Antworten) | Mittel | Menschliche Überprüfung empfohlen; der Assistent darf nicht einzige Informationsquelle sein |
| Prompt-Injection durch adversarielle Nutzereingaben | Niedrig–Mittel | Eingabe-Sanitisierungsschicht; MCP-Server schränkt Werkzeugumfang ein[cite:33][cite:43] |
| Unerwartetes agentisches Verhalten | Niedrig | Kein autonomer Internetzugang; Werkzeugaufrufe erfordern explizite MCP-Berechtigungen[cite:31] |
| Modellbias oder politisch gefilterte Ausgaben | Niedrig (lokales Modell) | Keine Live-Inhaltsfilterung durch Alibaba; Modellverhalten ist nach Deployment statisch |
| GGUF-Lieferkettenkompromittierung | Sehr niedrig | Nur offizielle Qwen-Releases werden verwendet; Checksums bei Download verifiziert[cite:34][cite:40] |

### Was in diesem Deployment kein Risiko darstellt

- **Daten nach China übertragen**: Nicht möglich — keine API-Aufrufe, vollständig offline Inferenz[cite:36][cite:45]
- **Echtzeit-Überwachung**: Das Modell hat keine persistente Erinnerung zwischen Sitzungen, sofern nicht explizit implementiert
- **Malware in den Gewichten**: Das GGUF-Format kann keinen beliebigen Code ausführen; llama.cpp ist das einzige Laufzeitsystem[cite:40]

---

## Zusammenfassung für Kunden

Der Aragon-Assistent verwendet Qwen, ein Open-Weight-Modell, das vollständig auf lokaler Infrastruktur ohne externe Datenübertragung läuft. Die Bezeichnung „unsicher" in KI-Sicherheits-Benchmarks bezieht sich auf Schwächen bei Inhalts-Guardrails, die praktisch alle aktuellen LLMs teilen — nicht auf Datensicherheits- oder Überwachungsrisiken. Die lokale Deployment-Architektur eliminiert die wesentlichen Bedenken gegenüber chinesischen Modellen. Nachprüfbare technische Belege für alle Sicherheitsaussagen sind auf Anfrage erhältlich.

> **Für kritische Entscheidungen immer menschliches Urteilsvermögen einsetzen. Der Assistent ist ein Werkzeug, keine Autorität.**

