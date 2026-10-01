# KI-E2 · Die Vielfalt der Werkzeuge

*Freiwilliger Sidequest · Reihe „KI im Wiki" · Einsteiger-Track, Artikel E2 · Typ C (ausführlich) · Final · Stand 2026*

---

## Worum es geht

Wer KI nur als Chatfenster kennt, nutzt einen kleinen Teil dessen, was möglich ist. Es gibt Werkzeuge, die Quellen durchsuchen, eigene Unterlagen auswerten, Code schreiben und testen, Sprache in Text verwandeln, Bilder erzeugen oder mehrstufige Aufgaben selbstständig abarbeiten. Sie unterscheiden sich in Stärken, Risiken und Kosten, und für dieselbe Aufgabe passt nicht jedes gleich gut.

Dieser Artikel baut eine Landkarte der **Werkzeugtypen** auf, damit man zu einer Aufgabe das passende Werkzeug findet. Produktnamen und kommerzielle Angebote tauchen nur in einer datierten Beispielbox am Ende auf, weil sie sich schnell ändern. Die Typen dagegen bleiben deutlich länger stabil. Die Begriffe aus E1 (KI, maschinelles Lernen, Sprachmodell) werden vorausgesetzt.

---

## 1. Modell, Anwendung und Werkzeug: der Unterschied, der alles ordnet

Bei KI-Werkzeugen werden zwei Dinge oft in einen Topf geworfen:

- Das **Modell** ist der Motor. Es berechnet die Antworten (zum Beispiel ein Sprachmodell, ein Bildmodell oder ein Spracherkennungsmodell).
- Die **Anwendung** ist das Fahrzeug um den Motor: Bedienoberfläche, Zugriff auf Dateien, Suche, Gedächtnis, Anbindung an andere Programme.

Ein Bild aus der Gärtnerei: Derselbe Dieselmotor kann in einem Traktor, einem Radlader oder einer Pumpe stecken. Die Aufgabe, die man damit erledigt, hängt am Fahrzeug und am Anbaugerät, nicht nur am Motor. Genauso kann dasselbe Sprachmodell in einem einfachen Chat, in einer Dokumentensuche und in einem Programmierwerkzeug stecken, und je nach Anwendung kann es ganz unterschiedliche Dinge tun.

Daraus folgt eine nützliche Regel: **Man wählt zuerst die Aufgabe und den Werkzeugtyp, erst danach das Produkt.**

---

## 2. Die Landkarte: Werkzeugtypen im Überblick

Die Liste mischt Fähigkeiten (was ein Werkzeug tut) mit Arten der Bereitstellung (wo es läuft oder wie es eingebettet ist). Ein Werkzeug kann deshalb mehreren Zeilen angehören.

| Typ | Was es tut | Typische Aufgaben | Worauf man achten sollte |
| --- | --- | --- | --- |
| **Chat-Assistent** | Führt ein Gespräch, erklärt, entwirft, formuliert um | Begriffe erklären, Texte überarbeiten, Ideen sammeln | Kann halluzinieren; Eingaben können beim Anbieter landen |
| **Suche mit Quellen** | Sucht im Web und fasst die Funde mit Fundstellen zusammen | Aktuelle Informationen, Recherche mit Belegen | Quellen selbst öffnen und prüfen; auch zitierte Zusammenfassungen können Fehler enthalten |
| **Dokumenten-Assistent** | Beantwortet Fragen auf Basis eigener Dateien | Skript, Handbuch oder Vertrag befragen, Zusammenfassungen, Lernmaterial aufbereiten | Es kann scheitern, die passenden Textstellen zu finden oder richtig wiederzugeben; bei wichtigen Antworten die Fundstelle im Original prüfen; vertrauliche Dokumente beachten |
| **Code-Assistent** | Erklärt, ergänzt und prüft Programmcode | Fehlermeldungen deuten, Tests vorschlagen, Code erklären | Vorschläge verstehen und testen, nicht blind übernehmen |
| **Bildmodelle** | Verarbeiten Bilder in verschiedenen Fähigkeiten: Erzeugen, Beschreiben, Texterkennung | Illustrationen, Skizzen, Bildbeschreibungen, Text aus Fotos | Urheberrecht und Kennzeichnung; Details oft fehlerhaft |
| **Audio- und Sprachmodelle** | Wandeln Sprache in Text um (Spracherkennung) und Text in Sprache (Sprachausgabe) | Meetings transkribieren, Vorlesefunktion, Sprachsteuerung | Erkennungsfehler bei Fachbegriffen und Dialekten; bei Aufnahmen von Gesprächen Vertraulichkeit und Erlaubnis der Beteiligten klären |
| **Agenten** | Führen mehrere Schritte selbstständig aus und nutzen dabei Werkzeuge | Aufgaben mit mehreren Stationen, Programmierarbeit über viele Dateien | Brauchen klare Grenzen und Kontrolle, weil Fehler sich fortpflanzen können |
| **Lokale Modelle** | Laufen auf dem eigenen Rechner statt beim Anbieter | Datensparsames Arbeiten, Experimentieren, Offline-Nutzung | Brauchen passende Hardware und Strom; oft kleinere und weniger leistungsfähige Modelle |
| **KI in Alltagssoftware** | Steckt in Office-Programmen, Suchmaschinen, Handys, Browsern | Zusammenfassen, Übersetzen, Vorschläge, Fotosuche | Oft unauffällig zugeschaltet; Einstellungen und Datenschutz prüfen |

Die Grenzen sind fließend. Viele moderne Assistenten bündeln mehrere Typen in einer Oberfläche: Sie chatten, durchsuchen das Web, lesen hochgeladene Dateien und erzeugen Bilder. Hilfreich bleibt die Frage, **welche Fähigkeit** man gerade braucht.

Beim Dokumenten-Assistenten steckt meist eine einfache Idee dahinter: Aus den hochgeladenen Unterlagen werden zur Frage passende Textstellen herausgesucht und dem Sprachmodell als Grundlage mitgegeben. Das Modell antwortet dann auf dieser Basis, statt nur aus dem Training. Die Technik dahinter behandelt der Artikel P4.

---

## 3. Von der Aufgabe zum Werkzeugtyp

Die Tabelle zeigt, wie sich Alltagsaufgaben auf Typen verteilen. Sie ist eine Orientierung, keine Regel: Mehrere Typen können passen, die Spalte „Erste Wahl" nennt den naheliegendsten.

| Aufgabe | Erste Wahl | Warum |
| --- | --- | --- |
| Ein Fachbegriff soll auf einfachem Niveau erklärt werden | Chat-Assistent | Gespräch mit Nachfragen, Erklärung in verschiedenen Tiefen |
| Man braucht aktuelle Fakten, etwa zu einem Gesetzesstand | Suche mit Quellen | Fundstellen lassen sich öffnen und gegenprüfen |
| Ein 80-seitiges Skript soll abgefragt werden | Dokumenten-Assistent | Antworten beziehen sich auf die eigenen Unterlagen |
| Eine Fehlermeldung beim Programmieren versteht man nicht | Code-Assistent oder Chat-Assistent | Code und Meldung lassen sich gemeinsam analysieren |
| Eine Besprechung soll protokolliert werden, mit Erlaubnis der Beteiligten | Audio-/Sprachmodell, danach Chat-Assistent zum Zusammenfassen | Erst Sprache zu Text, dann Text verdichten |
| Eine Skizze oder Grafik wird für eine Präsentation gebraucht | Bildmodell | Erzeugt Entwürfe aus Beschreibungen |
| Ein wiederkehrender Ablauf soll automatisiert werden | Agent oder Skript mit KI-Baustein | Mehrere Schritte laufen ohne manuelles Zutun |
| Vertrauliche Notizen sollen ausgewertet werden | Lokales Modell | Bei vollständig lokalem Betrieb können die Daten auf dem eigenen Rechner bleiben; Zusatzfunktionen und Einstellungen prüfen |

Das Muster dahinter: **Je mehr es auf aktuelle Fakten und Belege ankommt, desto wichtiger sind Suche und Quellen. Je mehr es um eigene Unterlagen geht, desto wichtiger ist die Anbindung an diese Unterlagen. Je vertraulicher die Daten, desto eher gehört das Werkzeug auf den eigenen Rechner oder in eine geprüfte Umgebung.**

---

## 4. Vier Unterscheidungen, die Werkzeuge einordnen helfen

### 4.1 Assistent oder Agent

Ein **Assistent** antwortet auf eine Anfrage, danach ist wieder der Mensch am Zug. Ein **Agent** bekommt ein Ziel und arbeitet mehrere Schritte selbstständig ab: Er plant, nutzt Werkzeuge (etwa Suche, Dateien, Programmierumgebungen), prüft das Zwischenergebnis und macht weiter. Wie viel Eigenständigkeit ein Agent hat, hängt vom Werkzeug und den Einstellungen ab; „Agent" bedeutet nicht automatisch vollständige Autonomie. Das spart Handarbeit, bedeutet aber auch, dass sich ein früher Fehler durch alle folgenden Schritte ziehen kann. Je größer der Handlungsspielraum, desto wichtiger sind klare Grenzen, Protokolle und eine Kontrolle durch Menschen.

Ein möglicher Weg, KI-Anwendungen mit externen Werkzeugen und Datenquellen zu verbinden, ist das **Model Context Protocol (MCP)**, ein offener Standard. Er ist nicht nur für Agenten gedacht und keine Voraussetzung für sie. Anthropic hat MCP im Dezember 2025 an die Agentic AI Foundation übergeben, die unter dem Dach der Linux Foundation arbeitet. Wie MCP funktioniert, behandelt der Artikel P5.

### 4.2 Allgemein oder spezialisiert

Allgemeine Assistenten können vieles halbwegs gut. Spezialisierte Werkzeuge, etwa für Programmcode, Transkription oder Literaturrecherche, sind auf eine Aufgabenklasse zugeschnitten und bieten dort häufig passende Funktionen und Arbeitsabläufe. Ob sie bessere Ergebnisse liefern als ein allgemeiner Assistent, sollte man an der eigenen Aufgabe prüfen: Ein Spezialwerkzeug kann in einem Punkt glänzen und in einem anderen schwächer sein.

### 4.3 Cloud oder lokal

| | Cloud | Lokal |
| --- | --- | --- |
| **Wo läuft das Modell?** | Bei einem Anbieter | Auf dem eigenen Rechner |
| **Stärken** | Leistungsfähige Modelle, kein eigener Aufwand, meist vom Anbieter aktuell gehalten | Daten können für die Modellverarbeitung auf dem eigenen Rechner bleiben, Offline-Nutzung möglich, keine Gebühren pro Anfrage |
| **Schwächen** | Eingaben gehen an den Anbieter, laufende Kosten, Abhängigkeit | Hardware und Strom kosten, Einrichtung nötig, oft kleinere Modelle |

„Lokal" ist keine pauschale Datenschutzgarantie: Es gilt nur, solange Verarbeitung und genutzte Zusatzfunktionen (etwa Plug-ins oder Internetzugriff) tatsächlich auf dem eigenen Rechner bleiben.

Für den Speicherbedarf lokaler Modelle zählt der verfügbare Arbeitsspeicher bzw. GPU-Speicher. Wie viel tatsächlich gebraucht wird, hängt von der Modellgröße, der Quantisierung (Verringern der Zahlengenauigkeit, siehe P1) und der Kontextlänge ab; manche Programme können ein Modell außerdem auf Prozessor und Grafikkarte aufteilen. Eine grobe Faustregel für die Modellgewichte bei 4-Bit-Quantisierung: Parameterzahl mal 0,5 Byte. Kontext und Laufzeit brauchen zusätzlichen Speicher. Zur Orientierung: Eine quantisierte 8-Milliarden-Variante eines verbreiteten offenen Modells ist rund 5 GB groß, die 70-Milliarden-Variante rund 43 GB. Die Dateien liegen damit etwas über der Faustregel, weil sie zusätzliche Daten enthalten. Kleine Modelle laufen also auch auf Rechnern mit wenig Speicher, große brauchen deutlich mehr. Den Bedarf sollte man für das konkrete Modell und Programm prüfen.

### 4.4 Offen oder geschlossen

Bei **offenen Modellen** sind die Gewichte, also die gelernten Zahlenwerte des Modells, öffentlich verfügbar. Je nach Lizenz und technischer Ausstattung lassen sie sich herunterladen, lokal betreiben und teils anpassen. **Geschlossene Modelle** werden dagegen in der Regel nur über eine Anwendung oder Schnittstelle des Anbieters genutzt.

„Offene Gewichte" ist dabei nicht dasselbe wie „Open Source". Nach der Definition der Open Source Initiative (Version 1.0, Oktober 2024) gehören zu einer Open-Source-KI neben den Parametern auch der Code und ausreichend genaue Informationen zu den Trainingsdaten. Wer ein Modell einsetzen will, sollte deshalb die Lizenzbedingungen lesen.

---

## 5. Drei Fragen vor der Wahl

Die drei Fragen greifen das Muster aus Abschnitt 3 als Checkliste auf.

1. **Was ist die Aufgabe, und was ist ein gutes Ergebnis?** Ein Entwurf, ein belegter Fakt, eine fertige Datei, ein automatisierter Ablauf?
2. **Welche Daten gehen hinein?** Öffentliche Texte, eigene Notizen, personenbezogene oder vertrauliche Daten? Je sensibler, desto sorgfältiger die Wahl von Anbieter, Einstellungen oder lokaler Lösung. Mehr dazu in E7.
3. **Wie wichtig ist Richtigkeit?** Bei einem Ideenentwurf reicht ein grober Treffer, bei Zahlen, Gesetzen oder Code im Einsatz braucht es eine Prüfung der Ergebnisse. Mehr dazu in E6 und A8.

---

## 6. Häufige Fehlgriffe

- **Ein Werkzeug für alles.** Wer jede Aufgabe im Chatfenster löst, verschenkt Möglichkeiten und riskiert falsche Fakten, wo eine Suche mit Quellen besser wäre.
- **Dateien hochladen, ohne nachzudenken.** Vertrauliche Unterlagen gehören nicht in jedes Werkzeug. Das ist eine Frage der Umgebung, nicht nur des Misstrauens.
- **Agenten ohne Aufsicht.** Je mehr Spielraum, desto genauer ist zu prüfen, was das System getan hat.
- **Produktname statt Aufgabe.** Marken wechseln, Modelle werden ersetzt. Wer in Aufgaben und Typen denkt, kommt mit jedem Neuling schnell zurecht.
- **Bunte Oberfläche gleich Qualität.** Ob ein Werkzeug sauber arbeitet, zeigt sich an geprüften Ergebnissen, nicht am Auftritt.

---

## Beispielbox: Produkte und Projekte zu den Typen

> **Stand: 01.10.2026.** Die Auswahl stützt sich auf Quellen aus dem Frühjahr und Sommer 2026. Sie ist eine Orientierung ohne Anspruch auf Vollständigkeit und ohne Bewertung. Angebote, Namen und Funktionen ändern sich schnell; der Haupttext oben bleibt davon unberührt.

| Typ | Beispiele |
| --- | --- |
| Chat-Assistenten | ChatGPT (OpenAI), Claude (Anthropic), Gemini (Google), Copilot (Microsoft), Angebote von Mistral, DeepSeek und xAI (Grok) |
| Suche mit Quellen | Perplexity; außerdem Suchfunktionen innerhalb der Chat-Assistenten |
| Dokumenten-Assistent | Gemini Notebook (Google, bis Juli 2026 unter dem Namen NotebookLM): beantwortet Fragen auf Basis hochgeladener Quellen |
| Code-Assistenten in der Entwicklungsumgebung | Cursor, GitHub Copilot |
| Code-Agenten im Terminal | Claude Code, OpenCode, Aider |
| Code-Agenten in der Cloud | Devin, Codex Cloud (OpenAI) |
| Bildmodelle | Midjourney, Stable Diffusion, Adobe Firefly |
| Sprachausgabe | ElevenLabs (Cloud), Piper (lokal) |
| Spracherkennung | Whisper (OpenAI), offenes Modell, lokal betreibbar |
| KI in Alltagssoftware | Copilot in Windows und Office (Microsoft), Gemini in Google-Diensten, Firefly in Adobe-Programmen |
| Lokale Modelle (Programme) | Ollama, llama.cpp, LM Studio |
| Offener Standard zur Werkzeuganbindung | Model Context Protocol (MCP) |

---

## Fazit

KI ist ein Werkzeugkasten, kein einzelnes Werkzeug. Die wichtigste Unterscheidung ist die zwischen Modell (Motor) und Anwendung (Fahrzeug): Was ein Werkzeug kann, hängt an beidem. Werkzeugtypen wie Chat-Assistent, Suche mit Quellen, Dokumenten-Assistent, Code-Assistent, Bildmodelle, Audio- und Sprachmodelle, Agenten, lokale Modelle und KI in Alltagssoftware decken sehr verschiedene Aufgaben ab. Wer erst die Aufgabe klärt, dann die Daten und die Anforderung an die Richtigkeit, findet meist schnell den passenden Typ. Im nächsten Artikel E3 geht es darum, mit einem Chat-Assistenten ein Gespräch zu führen statt nur Einzelfragen zu stellen.

```yaml
dokument: ki-e2-die-vielfalt-der-werkzeuge
typ: C
ausfuehrung: ausfuehrlich
reihe: ki-im-wiki
status: final
stand: 2026-10-01
quellen_fachlich:
  - "MCP-Blog, MCP joins the Agentic AI Foundation (09.12.2025)"
  - "Google, NotebookLM wird Gemini Notebook (16.07.2026)"
  - "Continue-Repository (nicht mehr gepflegt, daher nicht in der Box)"
  - "OpenAI Hilfezentrum, Using Codex Cloud"
  - "Open Source Initiative, Open Source AI Definition 1.0 (28.10.2024)"
  - "Ollama-Bibliothek Llama 3.1 (Dateigrößen 8B ca. 4,9 GB, 70B ca. 43 GB, jeweils q4_K_M)"
  - "IT-Daily, KI-Assistenten im Vergleich 2026 (01.05.2026); Firecrawl, Best AI Coding Agents (2026); daily.dev, Running LLMs Locally (2026); innfactory.ai, Whisper; itgrundlagen.com, KI-Liste 2026; kopfundstift.de, KI-Sprachgeneratoren (Stand Juli 2026) – nur für Produktbox, Sekundärquellen"
verifikation_offen:
  - "Produktzeilen ohne Primärbeleg (Perplexity, Cursor, GitHub Copilot, Claude Code, OpenCode, Aider, Devin, Bildmodelle, ElevenLabs, Piper, Alltagssoftware, Mistral/DeepSeek/xAI) über Herstellerseiten prüfen"
  - "Faustregel 0,5 Byte pro Parameter und CPU/GPU-Aufteilung gegen Ollama- bzw. llama.cpp-Dokumentation abgleichen (Artikel P1)"
  - "MCP-Angaben gegen Mitteilung der Linux Foundation prüfen"
review_historie:
  - runde: 0
    datum: 2026-10-01
    ergebnis: "Erster Draft nach Recherche."
  - runde: 1
    datum: 2026-10-01
    ergebnis: "Drei externe Reviews eingearbeitet und gegen Primärquellen geprüft: NotebookLM zu Gemini Notebook (16.07.2026), Continue entfernt, Open Weights von Open Source AI getrennt, VRAM-Tabelle durch qualitative Aussage und Faustregel ersetzt, Lokal-Datenschutz und Kosten präzisiert, MCP von Agenten gelöst, Agent nicht automatisch autonom, Spezialwerkzeuge neutral, Box um Bildmodelle, Sprachausgabe und Alltagssoftware ergänzt, Anbieter bei Grok ergänzt, Hoflader durch Radlader ersetzt. Nicht übernommen: weitere Zahlenstaffel zu VRAM, Produktvergleiche."
  - runde: 2
    datum: 2026-10-01
    ergebnis: "Letzter Selbstcheck: Typ-C-Regeln und alte Formulierungen geprüft, keine Befunde. Kleine Glättungen (Faustregel mit Dateigröße abgeglichen, zwei Absolutformulierungen entschärft). Final nach ausdrücklichem OK von David. Produktzeilen bleiben Sekundärquellen-Stand, siehe verifikation_offen."
```