# LF10a Teil 1 – Konzeption & Struktur

> **Zielgruppe:** Umschüler FIAE, 2. Lehrjahr (Aufbaustufe – ab hier trennen sich FIAE und FISI im Lernkontext), Dualis-Institut Hamburg
> **Prüfungsrelevanz:** Schwerpunkt AP2 im Prüfungsbereich **"Planen eines Softwareproduktes"** (schriftlich, 90 Minuten, 10% Gewichtung) – insbesondere die dort explizit geprüften Punkte "Programmspezifikationen anwendungsgerecht festlegen", "Bedienoberflächen funktionsgerecht und ergonomisch konzipieren" sowie "Maßnahmen zur Qualitätskontrolle planen und durchführen". Einzelne Grundlagen können bereits in AP1 vorkommen; projektbezogene UI-Entscheidungen können zudem im Bereich "Planen und Umsetzen eines Softwareprojektes" und im Fachgespräch relevant werden. Nicht jedes hier behandelte Detailkonzept ist automatisch ein eigenständiger Prüfungsgegenstand.
> **Lernzeit:** Ca. 2,5–3 Stunden Lesen/Durcharbeiten, +1,5–2 Stunden für Anforderungs-/Prototyp-Übungen
> **Status:** Final
> **Stand:** 2026-09-14
>
> Die 🔴/🟡/🟢-Markierungen in diesem Artikel sind eine didaktische Einschätzung zur Prüfungsvorbereitung, keine offizielle IHK-Gewichtung.
>
> **Hinweis zur Abgrenzung:** LF10a behandelt die Entwicklung von Benutzeroberflächen und ist der erste Teil einer dreiteiligen Serie (Teil 1: Konzeption & Struktur; Teil 2: Interaktion & Daten; Teil 3: Sicherheit, Test & Abnahme). Allgemeine Anforderungsanalyse-Grundlagen (Lastenheft/Pflichtenheft, Kano-Modell) wurden bereits in LF5.1 behandelt – hier geht es um die UI-spezifische Anwendung davon.

---

## IHK-Kernfragen dieses Artikels

| # | Frage | Abschnitt |
| --- | --- | --- |
| 1 | Wie werden aus einem Ist-Prozess und Nutzergruppen prüfbare, priorisierte UI-Anforderungen? | [→ 1. Anforderungen und Zielgruppen](#1-anforderungen-und-zielgruppen) |
| 2 | Wie unterscheiden sich Drei-Schichten-Architektur, Client-Server und MVC? | [→ 2. Informationsarchitektur und Prototyp](#2-informationsarchitektur-und-prototyp) |
| 3 | Welches UX-/Usability-Testverfahren passt zu welcher Fragestellung? | [→ 2. Informationsarchitektur und Prototyp](#2-informationsarchitektur-und-prototyp) |
| 4 | Warum beschreibt HTML zuerst Bedeutung, und wann ist ARIA gerechtfertigt? | [→ 3. Semantisches HTML und Barrierefreiheit](#3-semantisches-html-und-barrierefreiheit) |
| 5 | Wie werden Breakpoints begründet, und welche WCAG-2.2-AA-Kriterien gelten für responsive Oberflächen? | [→ 4. Responsive Design und Designsystem](#4-responsive-design-und-designsystem) |

---

## 1. Anforderungen und Zielgruppen

> **Grundprinzip:** Ein Kunde, der "ein Portal" bestellt, hat schon eine Lösung im Kopf – aber noch kein beschriebenes Problem. Ein Architekt, der sofort zu bauen beginnt, ohne zu fragen, wer im Haus wohnt und wofür jeder Raum gebraucht wird, baut am Bedarf vorbei. Erst der verstandene Ablauf rechtfertigt den Entwurf.

### 1.1 Problemraum vor Lösungsraum

Der **Problemraum** umfasst den heutigen Ablauf, beteiligte Personen, benötigte Informationen, beobachtbare Schwierigkeiten und offene Fragen. Der **Lösungsraum** beginnt erst mit konkreten Oberflächen, Funktionen oder Technologien. Jede Aussage wird einer von drei Klassen zugeordnet:

| Klasse | Bedeutung |
| --- | --- |
| **Belegt** | Durch Auftrag, Beobachtung, Prozessdokument oder bestätigte Aussage gestützt |
| **Annahme** | Plausibel, aber noch nicht bestätigt |
| **Offene Frage** | Für eine Entscheidung fehlt Information |

**Stakeholder** sind Personen oder Gruppen, die eine Lösung nutzen, beauftragen, betreiben, prüfen oder von ihr betroffen sind – Nutzer:innen sind eine wichtige, aber nicht die einzige Teilgruppe.

> **Wichtig für die Prüfung:** Plausibilität ist kein Beleg. Wird eine Annahme nicht sichtbar gekennzeichnet, entstehen Anforderungen und Gestaltung auf einer möglicherweise falschen Grundlage.

### 1.2 Ist-Prozess, Medienbruch und Personas

Zu jedem Prozessschritt gehören: ausführende Rolle, benötigte/erzeugte Informationen, verwendetes Medium, mögliche Rückfrage/Ausnahme, sichtbares Zwischenergebnis. Ein **Medienbruch** liegt beim Wechsel zwischen nicht durchgängig verbundenen Medien/Systemen vor. Für die Anforderungsanalyse relevant wird er insbesondere dann, wenn daraus eine beobachtbare negative Folge entsteht – etwa Doppelarbeit, Verzögerung oder Übertragungsfehler.

Eine **Persona** ist ein verdichtetes Arbeitsmodell für eine relevante Nutzergruppe – keine erfundene Biografie. Sie beruht auf Erkenntnissen aus Beobachtungen, Interviews oder anderen Nutzungsdaten; liegen solche Belege noch nicht vor, handelt es sich um eine ausdrücklich als Annahme gekennzeichnete Proto-Persona (vgl. die Belegt/Annahme/offene-Frage-Klassifikation aus 1.1). Relevant sind Ziel/zentrale Aufgaben, Häufigkeit/Zeitdruck, Arbeitsumgebung/Endgerät, benötigte Informationen, Erfahrung, Einschränkungen. Der **Nutzungskontext** erklärt, unter welchen Bedingungen die Aufgabe erledigt wird (z. B. Handschuhe an einem Tablet vs. Büroarbeitsplatz).

> **Wichtig für die Prüfung:** Alter, Name oder Hobbys sind nur relevant, wenn sie nachweisbar eine UI-Entscheidung beeinflussen (Navigation, Inhalt, Bedienung, Darstellung) – sonst gehören sie nicht in die Persona.

### 1.3 User Flow, Anforderungen und Akzeptanzkriterien

Ein **User Flow** verbindet Nutzerziel, Aktionen, Entscheidungen und Rückmeldungen von einem fachlichen Startpunkt zu einem sichtbaren Ergebnis – inklusive Fehler, Abbruch und Rückweg. Der **Happy Path** ist nur der erwartete Erfolgsweg ohne Ausnahme, also ein Teil des Flows, kein Ersatz für Negativ-/Abbruch-/Korrekturwege.

**Funktionale Anforderungen** beschreiben, was ein System tun bzw. ermöglichen soll (z. B. "Nutzer können eine Anfrage erfassen"); **nichtfunktionale Anforderungen** beschreiben Qualitätsmerkmale und Randbedingungen, unter denen dies geschieht (z. B. "Das Formular muss per Tastatur bedienbar sein", "Die Antwort soll innerhalb von 2 Sekunden erfolgen"). Rechtliche/qualitative Rahmenbedingungen wie Datenschutz können dabei sowohl nichtfunktionale Anforderungen erzeugen als auch konkrete funktionale Anforderungen nach sich ziehen (z. B. "Nutzer können ihre Einwilligung widerrufen"). Priorisierung nach **Muss/Soll/Kann**:

| Stufe | Bedeutung |
| --- | --- |
| **Muss** | Ohne Erfüllung ist der vereinbarte Kernnutzen oder eine zwingende Grenze verfehlt |
| **Soll** | Hoher Nutzen, aber bei begründeter Abwägung verschiebbar |
| **Kann** | Zusätzlicher Nutzen, sofern Zeit und Risiko es erlauben |

Ein **Akzeptanzkriterium** macht eine Anforderung beobachtbar. Eine häufige Form ist Gegeben–Wenn–Dann (Given/When/Then): *"Gegeben sei ein definierter Ausgangszustand. Wenn eine Person eine Handlung ausführt, dann tritt ein sichtbares, prüfbares Ergebnis ein."* Entscheidend ist nicht die genaue Formulierung, sondern dass das Kriterium eindeutig beobachtbar und prüfbar ist. Wörter wie "intuitiv", "modern" oder "schnell" sind ohne mess-/beobachtbares Kriterium zu unbestimmt.

> **IHK-Typfrage:** *"Ein Kunde fordert: 'Die Anfrageerfassung soll benutzerfreundlich sein.' Formulieren Sie eine prüfbare Anforderung mit Akzeptanzkriterium."*
> **Musterantwort:** "Benutzerfreundlich" benennt weder beobachtbares Verhalten noch einen Grenzwert und ist damit kein Akzeptanzkriterium. Zunächst wird der konkrete Nutzungskontext geklärt (z. B. Erfassung unter Zeitdruck in der Werkhalle). Daraus könnte eine prüfbare Anforderung werden: "Für die vollständige Erfassung einer Standardanfrage sind höchstens fünf Pflichtfelder erforderlich." Das braucht zwei Akzeptanzkriterien, da ein einzelnes die Anforderung nicht vollständig abdeckt: (1) "Gegeben ist das leere Formular. Wenn die als Pflichtfeld gekennzeichneten Eingaben gezählt werden, dann sind höchstens fünf vorhanden." (2) "Gegeben ist das leere Formular. Wenn alle Pflichtfelder gültig ausgefüllt und abgesendet werden, dann erscheint eine Eingangsbestätigung mit sichtbarer Vorgangsnummer." Die Prüfung umfasst eine Inspektion der Pflichtfelder sowie einen positiven und negativen Akzeptanztestfall.

### 1.4 Datenbedarf und Datensparsamkeit

Für jedes Datenfeld wird geprüft: Zweck und fachliche Entscheidung, benötigte Nutzergruppe, Pflicht/optional, Sichtbarkeit, Schutzbedarf, Aufbewahrungs-/Löschfrage. **Datensparsamkeit** bedeutet, nur für den festgelegten Zweck erforderliche personenbezogene Daten zu verarbeiten – Freitext ist besonders kritisch, da dort unvorhersehbare Informationen landen können.

> **Typische Prüfungsfalle:** "Ausgeblendete oder deaktivierte UI-Funktionen sind bereits eine Berechtigungsprüfung" – das stimmt nicht. Ausblenden kann Orientierung verbessern, verhindert aber keinen manipulierten Aufruf. Echte Autorisierung muss serverseitig erfolgen (vertieft in LF10a Teil 3).

---

## 2. Informationsarchitektur und Prototyp

> **Grundprinzip:** Eine Navigation, die einfach die internen Ordnernamen einer Firma übernimmt ("Vorgang", "Dispo", "Zentrale Ablage"), beantwortet einer neuen Person weder "Wo beginne ich?" noch "Wo finde ich meine Anfrage?". Erst wenn Inhalte nach Nutzeraufgaben statt nach interner Ablagelogik geordnet werden, entsteht eine brauchbare Struktur.

### 2.1 Arten von Benutzerschnittstellen

Eine **Benutzerschnittstelle** ist Teil eines Mensch-Maschine-Systems (MMS). Aufgabe und Nutzungskontext entscheiden über die passende Interaktionsform:

| Kürzel | Art | Kennzeichen |
| --- | --- | --- |
| CLI | Command Line Interface | Textbasierte Befehlseingabe, ressourcensparend, gut automatisierbar |
| GUI | Graphical User Interface | Grafische Elemente (Fenster, Schaltflächen, Eingabefelder) |
| NUI | Natural User Interface | Berührung/Geste |
| VUI | Voice User Interface | Sprachbasierte Ein-/Ausgabe |

🟢 *Randnotiz:* Weitere, seltenere Kategorien sind **OUI** (Organic User Interface, verformbare/räumliche Oberflächen) und **BCI** (Brain-Computer Interface, Steuerung über neuronale Signale, oft als assistive Schnittstelle) – für den FIAE-Schwerpunkt dieses Artikels nachrangig.

> **Wichtig für die Prüfung:** Die Klassifikation beschreibt die Interaktionsform, nicht automatisch die Qualität – eine CLI kann für eine wiederholbare Administrationsaufgabe angemessener sein als eine GUI.

### 2.2 Informationsarchitektur und Architekturmuster

Eine robuste Reihenfolge: (1) Nutzeraufgaben/benötigte Informationen sammeln → (2) zusammengehörige Inhalte gruppieren → (3) Gruppen verständlich benennen → (4) Priorität/Reihenfolge festlegen → (5) erst danach Ansichten/Navigation ableiten. Das **Mental Model** ist die Vorstellung, die Nutzer:innen über einen Ablauf besitzen – Beschriftungen orientieren sich daran, nicht an internen Datenbanknamen.

Drei häufig verwechselte Architekturmuster beantworten unterschiedliche Fragen:

| Muster | Beantwortet | Kernidee |
| --- | --- | --- |
| **Drei-Schichten-Architektur** | Welche fachlichen Zuständigkeiten gibt es? | Trennt Präsentation, Anwendungslogik, Datenhaltung |
| **Client-Server-Architektur** | Wie sind Dienste auf Systeme verteilt? | Client fordert Leistung an, Server stellt sie über Schnittstelle bereit |
| **Model View Controller (MVC)** | Wie sind Darstellung, Eingabe und Modell strukturiert? | Model = Daten/Zustand, View = Darstellung, Controller = Interaktionsverarbeitung |

> **Wichtig für die Prüfung:** Die drei Begriffe sind **keine Synonyme** – wer sie austauschbar verwendet, dokumentiert eine Systemgrenze falsch. Sie schließen sich auch nicht gegenseitig aus: MVC beschreibt häufig die interne Struktur *innerhalb* der Präsentationsschicht einer Drei-Schichten-Architektur, während Client-Server zusätzlich beschreibt, wie diese Schichten auf kommunizierende Systeme verteilt sind. Rollenbezogene Navigation (unterschiedliche Menüs je Rolle) ist zudem eine Darstellungsentscheidung, **keine Sicherheitsgrenze** – die lässt sich im Browser manipulieren; echte Autorisierung bleibt Backend-Aufgabe.

### 2.3 Design Thinking, Prototypenstufen und UI-Zustände

**Design Thinking** ist ein nutzerzentrierter, iterativer Prozessrahmen (verstehen/beobachten → Problem schärfen → Ideen entwickeln → prototypisieren → testen) – keine einmalig linear abzuarbeitende Phasenfolge.

| Stufe | Detailgrad |
| --- | --- |
| **Sketch** | Schnelle, bewusst grobe Ideenskizze |
| **Wireframe** | Grobe Struktur, Reihenfolge, Bedienelemente |
| **Mockup** | Visuell ausgearbeitet, meist noch statisch |
| **Prototyp** | Prüfbare Simulation eines Ablaufs |

Bereits im Prototyp gehören mindestens fünf UI-Zustände dazu (je nach Anwendung ergänzt um weitere wie "deaktiviert", "Berechtigung fehlt" oder "offline"): **Normal** (erwartete Daten/Aktionen), **Leer** (keine Daten, aber Ursache + nächster Schritt), **Laden** (laufender Vorgang, angemessene Rückmeldung), **Fehler** (Problem, Auswirkung, Fortsetzungsmöglichkeit), **Erfolg** (Ergebnis, Folgemöglichkeit). Jeder Zustand beantwortet: Was ist passiert? Was bedeutet das für mich? Was kann ich jetzt tun?

> **IHK-Typfrage:** *"Ein Prototyp zeigt nur den Normalzustand einer Anfrageliste. Welches Risiko birgt das, und welche Zustände fehlen mindestens?"*
> **Musterantwort:** Werden Leer-, Lade-, Fehler- und Erfolgszustand erst beim Programmieren bedacht, passt der bereits getestete Erfolgsentwurf oft nicht mehr zu ihnen, und wichtige Korrekturwege fehlen im späteren Test. Mindestens fehlen: der Leerzustand (z. B. "Noch keine Anfragen erfasst" mit einem Hinweis, wie man eine neue Anfrage anlegt), der Ladezustand (Rückmeldung, während Daten geladen werden), der Fehlerzustand (z. B. Ladefehler mit Wiederholungsoption) und der Erfolgszustand nach einer abgeschlossenen Aktion.

### 2.4 Usability, UX und die sieben Interaktionsprinzipien

**Usability**: wie wirksam, effizient und zufriedenstellend bestimmte Nutzer:innen ihre Ziele in einem festgelegten Kontext erreichen. **User Experience (UX)**: weiter gefasst, umfasst auch Wahrnehmungen/Reaktionen vor, während und nach der Nutzung.

Die sieben Interaktionsprinzipien nach **DIN EN ISO 9241-110:2020**:

| Prinzip | Kernaussage |
| --- | --- |
| Aufgabenangemessenheit | Unterstützt die Aufgabe ohne unnötige Schritte |
| Selbstbeschreibungsfähigkeit | Zustand und nächste Schritte sind verständlich |
| Erwartungskonformität | Begriffe/Reaktionen entsprechen dem Nutzungskontext |
| Erlernbarkeit | Bedienung mit vertretbarem Aufwand erschließbar |
| Steuerbarkeit | Ablauf, Richtung, Tempo beeinflussbar (inkl. Unterbrechbarkeit, Flexibilität, Individualisierbarkeit) |
| Robustheit gegen Benutzungsfehler | Fehler werden vermieden, erkannt, korrigierbar |
| Benutzerbindung | Fördert Vertrauen, ohne manipulative Gestaltung |

> **Wichtig für die Prüfung:** Die Prinzipien sind Beurteilungsperspektiven, keine unabhängig voneinander abhakbaren technischen Regeln – für einen konkreten Entwurf müssen sie in beobachtbare Kriterien übersetzt werden.

### 2.5 UX-Testverfahren passend auswählen

Bei einem **Usability-Test** wird untersucht, wie gut repräsentative Nutzer:innen einen Entwurf, Prototyp oder ein Produkt für konkrete Aufgaben verwenden können – nicht die Person selbst wird "getestet". Die Aufgabe nennt das Ziel, nicht den Bedienweg. Schlecht: "Klicke oben rechts auf Filter." Gut: "Finde einen Raum für 18 Personen am Dienstagvormittag." Ein Befund besteht aus: Aufgabe, Beobachtung, Problem, Auswirkung, Priorität, Entscheidung – Beobachtung und Interpretation werden getrennt.

| Verfahren | Liefert |
| --- | --- |
| Kontextanalyse | Beobachtung/Befragung im tatsächlichen Nutzungskontext |
| Fokusgruppe | Moderierte Gruppendiskussion mit Zielgruppe |
| Online-Befragung | Rückmeldung vieler Personen, ohne Verhaltensbeobachtung |
| Expertenbasierte Überprüfung (z. B. heuristische Evaluation) | Fachkundige Prüfung anhand Heuristiken |
| Labortest | Kontrollierte Aufgabenbearbeitung, gezielte Beobachtung |
| Eye-Tracking | Blickverlauf als ergänzender Hinweis (erklärt Ursache nicht allein) |

> **Wichtig für die Prüfung:** Ein **Usability-Test** untersucht, wie gut repräsentative Nutzer:innen mit einem Entwurf, Prototyp oder fertigen Produkt konkrete Aufgaben bewältigen – er setzt also keine bereits fertig implementierte Software voraus. Ein **Akzeptanztest** prüft dagegen, ob vorher vereinbarte Anforderungen/Akzeptanzkriterien erfüllt sind – derselbe Ablauf kann in beiden vorkommen, aber Fragestellung und Auswertung unterscheiden sich.

---

## 3. Semantisches HTML und Barrierefreiheit

> **Grundprinzip:** Eine Schaltfläche und ein rechteckiger `<div>`-Bereich können optisch identisch aussehen – für Tastatur und assistive Technik sind sie es nicht. Die native Schaltfläche bringt Rolle, Fokussierbarkeit und erwartetes Tastenverhalten automatisch mit; der `<div>`-Bereich nicht. HTML beschreibt zuerst Bedeutung, nicht Aussehen.

### 3.1 Elemente, Struktur und Dokumentreihenfolge

Wichtige Strukturelemente: `<header>` (Kopfbereich), `<nav>` (Navigationsgruppen), `<main>` (eindeutiger Hauptinhalt), `<section>` (thematischer Abschnitt mit Überschrift), `<aside>` (Ergänzung), `<footer>` (Abschluss). Eine `<div>` ist ein **neutraler Container** ohne automatische Semantik – für Aktionen wird grundsätzlich `<button>` verwendet, für Navigation `<a href="…">`.

Die **Dokumentreihenfolge** (Reihenfolge im HTML) beeinflusst Lesefluss, Tastaturbedienung und Darstellung ohne CSS. Überschriften bilden eine **logische Hierarchie**, keine Schriftgrößen. Linktexte sollten auch außerhalb des Satzes verständlich sein ("Details zu Anfrage 4711" statt "mehr").

### 3.2 Zugängliche Formulare

```html
<div class="field">
  <label for="room-count">Personenzahl</label>
  <input id="room-count" name="roomCount" type="number"
         min="1" max="60" required
         aria-describedby="room-count-hint">
  <p id="room-count-hint">Zahl zwischen 1 und 60.</p>
</div>
```

Ein Formularfeld benötigt einen zugänglichen Namen, meist über ein sichtbares `<label>`. **Platzhalter sind kein Label** – sie verschwinden bei Eingabe und sind oft kontrastarm. Zusammengehörige Auswahlmöglichkeiten werden mit `<fieldset>` und `<legend>` gruppiert.

### 3.3 WCAG, ARIA und die Prüfperspektive Name/Rolle/Zustand

Die **WCAG (Web Content Accessibility Guidelines)** beschreiben technologieunabhängige Erfolgskriterien, gegliedert nach vier Grundprinzipien – den **POUR-Prinzipien**: **P**erceivable (wahrnehmbar), **O**perable (bedienbar), **U**nderstandable (verständlich), **R**obust (robust). Jedes Erfolgskriterium gehört zu einer von drei Konformitätsstufen: **A** (Mindestanforderung), **AA** (üblicher Zielstandard, auch in diesem Artikel verwendet), **AAA** (höchste Stufe, oft nicht für alle Inhalte erreichbar). **ARIA** ergänzt Semantik/Zustände nur dann, wenn HTML allein eine echte Lücke lässt – die erste Regel lautet: passendes natives HTML-Element nutzen, wenn es existiert. ARIA verändert **nicht automatisch** Tastaturverhalten oder Funktion.

Prüfperspektive **Name, Rolle, Wert/Zustand**: Wie heißt das Element für assistive Technologien? Welche Rolle hat es (Link, Schaltfläche, Eingabe …)? Welcher Wert/Zustand ist relevant (ausgewählt, aufgeklappt …)?

Ein **Skip-Link** überspringt wiederkehrende Navigation und führt direkt zum Hauptinhalt. Eine **Live-Region** informiert über dynamische Statusänderungen ohne Fokusverschiebung – `role="status"` eignet sich für unkritische Meldungen; `role="alert"` besitzt eine dringliche, assertive Semantik und kann laufende Ausgaben unterbrechen (das genaue Verhalten hängt von Browser/assistiver Technik ab) und sollte deshalb nur für wirklich dringliche Meldungen verwendet werden.

> **IHK-Typfrage:** *"Ein Entwickler macht ein `<div>` mit `role="button"` klickbar, statt `<button>` zu verwenden. Bewerten Sie diese Entscheidung."*
> **Musterantwort:** `role="button"` ändert nur einen Teil der zugänglichen Beschreibung – die Rolle wird korrekt angekündigt. Fokussierbarkeit, Aktivierung mit Enter- und Leertaste, deaktivierter Zustand und das erwartete Tastenverhalten müssen dagegen zusätzlich manuell nachgebaut werden und werden dabei leicht vergessen oder fehlerhaft umgesetzt. Ein natives `<button>` liefert all das bereits automatisch und ist deshalb robuster – die Entscheidung sollte revidiert werden, sofern kein triftiger Grund gegen ein natives Element spricht.

### 3.4 Tastatur- und Strukturprüfung

Prüfschritte ohne Maus: (1) mit `Tab`/`Shift+Tab` alle interaktiven Elemente erreichen, (2) Links/Schaltflächen mit erwarteten Tasten auslösen, (3) sichtbaren Fokus erkennen, (4) in überlagerten Bereichen nicht feststecken, (5) nach Aktionen nachvollziehbaren Fokusort behalten. Anschließend CSS ausschalten: Bleiben Inhalte, Überschriften, Reihenfolge verständlich?

> **Typische Prüfungsfalle:** Positive `tabindex`-Werte "reparieren" die Fokusreihenfolge nicht – sie legen eine zweite, schwer wartbare Reihenfolge über das Dokument, die bei jeder Änderung erneut von der visuellen/DOM-Reihenfolge abweicht. Besser: sinnvolle HTML-Reihenfolge ohne positive Werte. Ebenso: Ein automatisierter Accessibility-Scan ist ein **Teilnachweis**, kein Beweis vollständiger Barrierefreiheit – Verständlichkeit und vollständige Tastaturbedienung müssen manuell geprüft werden.

---

## 4. Responsive Design und Designsystem

> **Grundprinzip:** Ein Layout, das nur bei drei zufällig gewählten Bildschirmbreiten getestet wurde, ist so wenig "responsiv" wie eine Brücke, die nur bei drei zufälligen Belastungen geprüft wurde, "sicher" ist. Ein Breakpoint ist erst begründet, wenn dort tatsächlich ein Inhalts- oder Bedienproblem auftritt – nicht bei einem bekannten Gerätenamen.

### 4.1 Kaskade, Boxmodell und Einheiten

Die **Kaskade** entscheidet bei widersprüchlichen Regeln anhand Herkunft, Wichtigkeit, Spezifität und Reihenfolge, welche gewinnt. ID-Selektoren haben höhere Spezifität als Klassen/Typen und sollten nicht als Standardmittel dienen – ständig steigende Spezifität löst Konflikte kurzfristig, erschwert aber spätere Änderungen. Im **Boxmodell** besteht ein Element aus Inhalt, Innenabstand, Rahmen, Außenabstand; `box-sizing: border-box` vereinfacht oft die Größenplanung.

| Einheit | Zweck |
| --- | --- |
| `rem` | Skalierbare Abstände/Schriftgrößen, relativ zur Basisschriftgröße des Dokuments (im Unterschied zu `em`, das relativ zum jeweiligen Elternelement skaliert und sich dadurch bei Verschachtelung aufsummieren kann) |
| `%`, `fr`, `minmax()`, `auto` | Flexible Verteilung |
| `ch` | Lesbare Textzeilen |
| `px` | Präzise Größenangaben für feine Details (z. B. Rahmenstärke); für responsive Hauptdimensionen nicht unnötig starr einsetzen |

Starre Pixel-Breiten für Hauptbereiche brechen bei Zoom, Übersetzung oder längeren Inhalten leicht.

### 4.2 Designsystem und Design-Tokens

Ein **Designsystem** verbindet Regeln, wiederverwendbare Komponenten und dokumentierte Entscheidungen. Ein **Design-Token** ist ein benannter Gestaltungswert:

```css
:root {
  --color-text: #1b1f24;
  --color-accent: #1558a6;
  --space-1: 0.5rem;
  --focus-ring: 0 0 0 0.2rem #ffbf47;
}
```

Der Name beschreibt den **Zweck**, nicht den konkreten Wert (`--color-danger` bleibt verständlicher als `--red-500`). Farbe allein darf nie einen Zustand erklären – Text, Symbol oder Struktur müssen ergänzen.

### 4.3 Layoutmodelle und Breakpoints

| Modell | Einsatz |
| --- | --- |
| Normalfluss | Inhalte, die natürlich untereinanderstehen |
| Flexbox | Eindimensionale Anordnung (Zeile/Spalte) |
| Grid | Zweidimensionale Zeilen-/Spaltenbeziehungen |

**Mobile-first** bedeutet, eine einfache Grunddarstellung zu definieren und bei verfügbarer Fläche zu erweitern – nicht, dass nur Smartphones wichtig sind. Ein **Breakpoint** liegt dort, wo Inhalt kollidiert, unlesbar wird oder eine Bedienhandlung unnötig schwer wird.

> **Typische Prüfungsfalle:** CSS-`order` verändert nur die **visuelle** Anordnung, nicht DOM-, Lese- oder Fokusreihenfolge – Tastaturnutzende springen dann scheinbar rückwärts durch die Seite. Die logische HTML-Reihenfolge muss die verständliche Grundlage bleiben.

### 4.4 Ausgewählte WCAG-2.2-AA-Prüfwerte

Diese Tabelle enthält ausgewählte, modulübergreifend wiederkehrende Prüfwerte als Referenz sowohl für die Umsetzung (hier) als auch für die spätere Testphase (LF10a Teil 3) – sie ist **keine vollständige Kriterienliste**:

| Prüfgegenstand | Erfolgskriterium | Prüfwert |
| --- | --- | --- |
| Textkontrast | 1.4.3 | mind. 4,5:1 (mind. 3:1 bei großem Text) |
| Textvergrößerung | 1.4.4 | bis 200% ohne Inhalts-/Funktionsverlust |
| Reflow | 1.4.10 | bei 320 CSS-Pixel Breite grundsätzlich ohne Verlust und ohne zweidimensionales Scrollen; ausgenommen sind nur Inhaltsteile, deren Nutzung oder Bedeutung ein zweidimensionales Layout tatsächlich erfordert (z. B. bestimmte komplexe Tabellen/Karten) – "breit" allein begründet keine Ausnahme |
| Nichttextkontrast | 1.4.11 | mind. 3:1 für wesentliche grafische Objekte/Bedienelemente, soweit für Verständnis/Bedienung erforderlich |
| Sichtbarer Fokus | 2.4.7 | jedes tastaturbedienbare Element hat sichtbaren Fokusindikator |
| **Fokus nicht verdeckt** (neu in WCAG 2.2) | 2.4.11 | Ein fokussiertes Element darf durch vom Autor erstellten Inhalt (z. B. Sticky Header, Cookie-Banner) nicht vollständig verdeckt sein |
| Zielgröße | 2.5.8 | mind. 24×24 CSS-Pixel; kleinere Ziele nur über die im Kriterium definierten Ausnahmen (u. a. messbare Abstandsausnahme, Inline-Ziele) |
| Statusmeldungen | 4.1.3 | programmatisch ermittelbar, ohne dafür Fokus zu erhalten |

> **Wichtig für die Prüfung:** Diese Tabelle ist **kein vollständiger WCAG-Konformitätsnachweis** – eine AA-Konformitätsaussage setzt weit mehr voraus (u. a. Tastaturbedienbarkeit insgesamt, Fokusreihenfolge, Beschriftungen, Fehlermeldungen, Spracheangaben, konsistente Navigation). Ein Messwerkzeug liefert Zahlenwerte, aber die richtige Auswahl von Vorder-/Hintergrundfarbe und die Bewertung von Zuständen bleibt eine fachliche Aufgabe.

---

## 5. Zusammenspiel der Konzepte in LF10a Teil 1

Die vier Module bauen direkt aufeinander auf: **Anforderungen und Zielgruppen** (Abschnitt 1) liefern den belegten Ausgangspunkt – ohne verstandenen Ist-Prozess und geprüfte Datenbedarfe entsteht jede weitere Entscheidung auf unsicherem Grund. **Informationsarchitektur und Prototyp** (Abschnitt 2) übersetzen diese Anforderungen in Struktur, Navigation und testbare Entwürfe, bevor Code entsteht. **Semantisches HTML** (Abschnitt 3) macht diese Struktur für Menschen und assistive Technik tatsächlich zugänglich. **Responsive Design** (Abschnitt 4) sorgt dafür, dass diese zugängliche Struktur unter wechselnden Bedingungen (Bildschirmgröße, Zoom, Eingabemethode) verständlich bleibt. Die in Abschnitt 2.3 entworfenen UI-Zustände (Normal/Leer/Laden/Fehler/Erfolg) und die in Abschnitt 4.4 verankerte WCAG-Tabelle ziehen sich als roter Faden durch die gesamte LF10a-Serie bis zur Testphase in Teil 3.

---

## 6. Typische Prüfungsfallen

| # | Falle | Richtigstellung |
| --- | --- | --- |
| 1 | Ein genannter Lösungsvorschlag ("ein Portal") beschreibt bereits das Problem | Problem- und Lösungsraum sind getrennt zu betrachten – erst der belegte Ist-Prozess rechtfertigt eine konkrete Lösung |
| 2 | Eine Persona braucht Alter, Namen und Hobbys | Relevant sind nur Merkmale, die eine nachvollziehbare UI-Entscheidung beeinflussen (Ziel, Zeitdruck, Umgebung, Endgerät, Einschränkungen) |
| 3 | Der Happy Path ist der vollständige User Flow | Der Happy Path ist nur der Erfolgsweg ohne Ausnahme – ein vollständiger Flow enthält auch Fehler, Abbruch und Korrektur |
| 4 | Drei-Schichten-Architektur, Client-Server und MVC meinen dasselbe | Sie beantworten unterschiedliche Fragen (fachliche Zuständigkeiten / Verteilung auf Systeme / Struktur innerhalb einer Anwendung) und sind keine Synonyme |
| 5 | Rollenbezogene Navigation ist bereits eine Sicherheitsgrenze | Sie ist eine Darstellungsentscheidung, im Browser manipulierbar – echte Autorisierung ist Backend-Aufgabe |
| 6 | Mehr ARIA bedeutet mehr Barrierefreiheit | ARIA kann native Rollen überschreiben oder widersprüchliche Zustände erzeugen – es schließt gezielt Lücken, ersetzt aber kein passendes HTML |
| 7 | Positive `tabindex`-Werte reparieren die Fokusreihenfolge | Sie erzeugen eine zweite, schwer wartbare Reihenfolge – besser ist eine sinnvolle HTML-Reihenfolge ohne positive Werte |
| 8 | Ein bekannter Gerätewert ist automatisch ein guter Breakpoint | Ein Breakpoint ist erst begründet, wenn an dieser Stelle ein konkretes Inhalts-/Bedienproblem auftritt |
| 9 | CSS-`order` sorgt für die richtige Reihenfolge | `order` verändert nur die visuelle Anordnung, nicht DOM-/Lese-/Fokusreihenfolge |
| 10 | Ein grüner Accessibility-Scan beweist Barrierefreiheit | Automatisierte Werkzeuge sind ein Teilnachweis – Verständlichkeit und vollständige Tastaturbedienung müssen manuell geprüft werden |

---

## 7. Vertiefung und Ausblick (freiwillig – für den ersten Durchgang nicht erforderlich)

- **Nutzwerttabellen im Detail**: Wie Gewichtung und Bewertung von Gestaltungsvarianten nachvollziehbar dokumentiert werden, ohne eine objektive Wahrheit vorzutäuschen.
- **Weitere ARIA-Live-Region-Feinheiten**: Unterschiede zwischen `aria-live="polite"` und `aria-live="assertive"` über die Basisfälle `status`/`alert` hinaus.
- **CSS-Spezifitätsrechnung im Detail**: Wie sich die Spezifität von Selektorkombinationen exakt berechnen lässt.
- **Grid-Template-Areas**: Benannte Layoutbereiche als Alternative zu reinen Spalten-/Zeilendefinitionen.
- **Eye-Tracking-Methodik**: Wie Fixationen und Sakkaden ausgewertet werden und welche Grenzen die Methode hat.

---

## 8. Selbsttest

| # | Frage | Kurzantwort |
| --- | --- | --- |
| 1 | Was unterscheidet Problemraum und Lösungsraum? | Problemraum: heutiger Ablauf, Personen, Informationen, Schwierigkeiten; Lösungsraum: konkrete Oberflächen/Funktionen/Technologien |
| 2 | Was macht einen Medienbruch prüfungsrelevant? | Erst eine beobachtbare Folge (Doppelarbeit, Übertragungsfehler, Verzögerung), nicht der Wechsel allein |
| 3 | Was unterscheidet Muss/Soll/Kann? | Muss: Kernnutzen/zwingende Grenze verfehlt ohne Erfüllung; Soll: hoher Nutzen, verschiebbar; Kann: Zusatznutzen je nach Zeit/Risiko |
| 4 | Nenne mindestens fünf UI-Zustände, die früh im Prototyp entworfen werden. | Normal, Leer, Laden, Fehler, Erfolg (je nach Anwendung weitere wie deaktiviert, Berechtigung fehlt, offline) |
| 5 | Was unterscheidet Usability von User Experience? | Usability: Wirksamkeit/Effizienz/Zufriedenheit beim Zielerreichen; UX: umfasst zusätzlich Wahrnehmungen/Reaktionen vor, während und nach der Nutzung |
| 6 | Warum ist `<div role="button">` schlechter als `<button>`? | Fokus, Tastaturaktivierung und Zustand müssen manuell nachgebaut werden – `<button>` liefert das automatisch |
| 7 | Was ist der Unterschied zwischen `role="status"` und `role="alert"`? | `status` für unkritische Meldungen ohne Unterbrechung; `alert` hat dringliche, assertive Semantik und kann laufende Ausgaben unterbrechen – nur für wirklich dringliche Meldungen verwenden |
| 8 | Wann ist ein Breakpoint begründet? | Wenn an dieser Stelle ein konkretes Inhalts- oder Bedienproblem auftritt, nicht bei einem bestimmten Gerätenamen |
| 9 | Welchen WCAG-2.2-Prüfwert hat die Zielgröße (2.5.8)? | Mindestens 24×24 CSS-Pixel, oder Erfüllung einer definierten Ausnahme (u. a. messbare Abstandsausnahme, Inline-Ziele) |

---

## 9. IHK-Cheatsheet

| Begriff | Kurzdefinition |
| --- | --- |
| **Stakeholder** | Nutzt, beauftragt, betreibt, prüft oder ist betroffen – weiter als "Nutzer" |
| **Medienbruch** | Wechsel zwischen unverbundenen Medien, relevant erst durch beobachtbare Folge |
| **Akzeptanzkriterium** | Macht eine Anforderung eindeutig beobachtbar/prüfbar; häufig als Gegeben-Wenn-Dann formuliert |
| **Muss/Soll/Kann** | Priorisierung, nicht "wichtig/unwichtig" |
| **Drei-Schichten / Client-Server / MVC** | Fachliche Zuständigkeiten / Systemverteilung / App-interne Struktur – keine Synonyme |
| **Wireframe vs. Mockup vs. Prototyp** | Grobe Struktur / visuell ausgearbeitet, statisch / prüfbare Simulation |
| **Die 7 Interaktionsprinzipien (ISO 9241-110)** | Aufgabenangemessenheit, Selbstbeschreibungsfähigkeit, Erwartungskonformität, Erlernbarkeit, Steuerbarkeit, Robustheit, Benutzerbindung |
| **UI-Zustände** | Mindestens Normal, Leer, Laden, Fehler, Erfolg – je nach Anwendung mehr |
| **Semantisches HTML** | Element nach fachlicher Aufgabe wählen, nicht nach Aussehen |
| **ARIA-Grundregel** | Natives HTML nutzen, wenn es existiert – ARIA nur für echte Lücken |
| **Name, Rolle, Wert/Zustand** | Prüfperspektive für zugängliche Elemente |
| **Design-Token** | Benannter Gestaltungswert mit Zweckbezug im Namen |
| **Reflow (1.4.10)** | Bei 320px Breite kein Inhalts-/Funktionsverlust, kein 2D-Scrollen – außer bei Inhalten, die zwingend 2D-Layout erfordern |

---

## 10. Prüfungstaktik

| Aufgabentyp | Typische Formulierung | Was die Prüfung hören will |
| --- | --- | --- |
| Anforderung prüfbar formulieren | "Formulieren Sie eine prüfbare Anforderung für X" | Konkretes Gegeben-Wenn-Dann-Kriterium, keine vagen Adjektive |
| Architekturbegriffe abgrenzen | "Erläutern Sie den Unterschied zwischen X und Y" | Jeweils die beantwortete Frage benennen, nicht nur ein Beispiel |
| ARIA/HTML-Entscheidung bewerten | "Bewerten Sie diesen `<div>`-Ansatz" | Konkrete fehlende Eigenschaften (Fokus, Tastatur, Zustand) benennen, natives Element vorschlagen |
| Breakpoint begründen | "Warum liegt der Breakpoint bei X?" | Konkretes Inhalts-/Bedienproblem nennen, nicht Gerätename |
| WCAG-Kriterium anwenden | "Erfüllt dieses Layout Kriterium X?" | Konkreten Prüfwert nennen und mit dem Fall abgleichen |

---

## 11. Merksätze für das Fachgespräch

> Erst den belegten Arbeitsablauf und die betroffenen Menschen verstehen, dann eine Oberfläche entwerfen.

> Struktur, Detailgrad und Testverfahren folgen der Nutzeraufgabe und der offenen Frage – nicht dem Lieblingswerkzeug.

> Erst native Bedeutung und logische Reihenfolge herstellen, dann nur gezielt mit ARIA ergänzen.

> Ein Layout ist responsiv, wenn Inhalt und Bedienung unter veränderten Bedingungen verständlich bleiben.

> Ausgeblendet ist nicht geschützt – Autorisierung ist immer Backend-Aufgabe.

---

```yaml
dokument: LF10a-Teil1-wiki-artikel
lernfeld: "LF10a Teil 1 (Module 10a.1-10a.4)"
titel: "Konzeption & Struktur"
typ: "Typ A – Kompakter Prüfungs-Wiki (FIAE-fokussiert)"
status: final
stand: 2026-09-14
quellen_intern:
  - "LF10a.1-4 Lernskripte (Anforderungen/Zielgruppen, Informationsarchitektur/Prototyp, Semantisches HTML/Barrierefreiheit, Responsive Design/Designsystem)"
  - "LF10a Szenarien (ServiceHub, drei Wahlszenarien) als Projektrahmen zur Kenntnis genommen, nicht inhaltlich in den Artikel übernommen (szenariospezifisch)"
  - "Bewusster Querverweis zu LF5.1 (allgemeine Anforderungsanalyse) statt Wiederholung"
  - "Ausgewählte WCAG-2.2-AA-Prüfwerte ursprünglich aus dem Rohmaterial übernommen, in Runde 2 web-verifiziert und präzisiert (2.4.11 ergänzt, Reflow-/Zielgröße-Ausnahmen geschärft, Titel als Auswahl statt vollständiger Referenz gekennzeichnet) - Tabelle dient als gemeinsame Pflegegrundlage für LF10a Teil 1 und Teil 3"
quellen_fachlich:
  - titel: "WCAG 2.2 (W3C), DIN EN ISO 9241-110:2020, WHATWG HTML Living Standard"
    herausgeber: "W3C, DIN/ISO, WHATWG"
    status: "Rohmaterial verlinkt Primärquellen (W3C WCAG 2.2, WHATWG HTML); Erfolgskriterien-Nummern (1.4.3/1.4.4/1.4.10/1.4.11/2.4.7/2.4.11/2.5.8/4.1.3) in Runde 2 gezielt gegen W3C-Understanding-Dokumente web-verifiziert (nicht nur plausibilisiert), 2.4.11 als neues Kriterium bestätigt, 2.4.7-Einstufung als AA (nicht A) bestätigt"
  - titel: "Architekturmuster (Drei-Schichten/Client-Server/MVC), UX-Testverfahren, Designsystem-Grundlagen"
    herausgeber: "Rohmaterial + etabliertes Software-/UX-Engineering-Standardwissen"
    status: "stabile Konzepte, keine Web-Verifikation nötig"
review_historie:
  - runde: 1
    datum: 2026-09-14
    ergebnis: "Erstdraft erstellt aus den vier LF10a.1-4-Lernskripten. Als ein zusammenhängender Artikel (Teil 1 von 3) gemäß mit Auftraggeber abgestimmter Dreiteilung verfasst. FIAE-fokussiert, mit explizitem Hinweis auf die Aufbaustufe (Trennung FIAE/FISI). WCAG-2.2-Tabelle unverändert aus Rohmaterial übernommen, da bereits korrekt und vom Auftraggeber als artikelübergreifende Referenz gekennzeichnet. Bewusster Querverweis zu LF5.1 statt Wiederholung allgemeiner Anforderungsanalyse."
  - runde: 2
    datum: 2026-09-14
    ergebnis: "3 Reviews eingearbeitet. Wichtigster Fund (2 Reviews, eigenständig web-verifiziert): WCAG-2.2-Tabelle fehlte das neue AA-Kriterium 2.4.11 'Focus Not Obscured' - ergänzt; Behauptung einer Review, 2.4.7 sei nur Level A statt AA, wurde gegengeprüft und als falsch verworfen (2.4.7 ist tatsächlich AA), nicht übernommen. Tabellentitel als 'Ausgewählte WCAG-2.2-AA-Prüfwerte' präzisiert (keine vollständige Kriterienliste), Reflow- und Zielgröße-Ausnahmen fachlich schärfer gefasst. Weitere Korrekturen: POUR-Prinzipien und Konformitätsstufen A/AA/AAA explizit benannt; role='alert' technisch präziser (assertive Semantik statt absolute 'erzeugt Unterbrechungen'); Usability-Test-Definition auf Entwurf/Prototyp/Produkt erweitert (nicht nur 'tatsächliche Nutzung'); Given-When-Then als häufige Form statt einzige Definition gekennzeichnet; IHK-Kernfrage 2 in zwei Fragen aufgeteilt; heuristische Evaluation als Fachbegriff ergänzt; UI-Arten-Tabelle auf FIAE-relevante Formen fokussiert (OUI/BCI in Randnotiz); UI-Zustände als Mindestmenge statt abschließende Liste gekennzeichnet; funktionale/nichtfunktionale Anforderungen allgemeiner gefasst; em/rem-Unterschied ergänzt; px-Beschreibung entschärft; Medienbruch-Definition und Relevanzbedingung begrifflich getrennt; MVC-zu-Drei-Schichten-Verhältnis ergänzt; Prüfungsrelevanz-Kopf präzisiert. Eine Review behauptete umfangreiche '[...]'-Platzhalter und abgeschnittene Textfragmente in mehreren Kapiteln - beim gezielten Abgleich (grep) nicht bestätigt, Abschnittsnummerierung 3.1-3.4 vollständig und lückenlos."
  - runde: 3
    datum: 2026-09-14
    ergebnis: "3 Reviews eingearbeitet. Wichtigster Fund (1 Review, web-verifiziert): Prüfungsbereich-Bezeichnung im Kopf war erfunden ('Konzeption und Umsetzung von Softwareprodukten' existiert nicht offiziell) - korrigiert zu 'Planen eines Softwareproduktes' (echter FIAE-AP2-Bereich, 90 Min., 10%), mit konkretem Bezug zu §13 Abs.1 Nr.2-4 FIAusbV (Programmspezifikationen, Bedienoberflächen, Qualitätskontrolle). YAML aktualisiert: 2.4.11 in Kriterienliste ergänzt, 'unverändert übernommen' korrigiert, Prüfquelle als 'gezielt web-verifiziert' statt 'eigenes Wissen' präzisiert. Akzeptanzkriterium-Beispiel logisch nachgebessert (ein Kriterium prüfte die 'höchstens fünf'-Anforderung nicht, jetzt zwei getrennte Kriterien). Persona-Grundlage ergänzt (Beleg vs. Proto-Persona, analog zur 1.1-Klassifikation). Kleinere Konsistenzkorrekturen: Cheatsheet-Akzeptanzkriterium und Reflow-Eintrag an präzisierte Haupttext-Formulierungen angeglichen, Selbsttest-Zielgröße-Antwort aktualisiert, doppeltes 'allein' behoben, 'selbst erzeugter Inhalt' zu normativem 'vom Autor erstellter Inhalt' präzisiert. Zum dritten Mal behauptete eine Review umfangreiche Platzhalter/abgeschnittene Fragmente - beim gezielten Abgleich erneut nicht bestätigt."
  - runde: 4
    datum: 2026-09-14
    ergebnis: "Eigene Abschlussprüfung: Usability-Test-Definition in Abschnitt 2.5 trug noch die alte, engere Formulierung ('Entwurf getestet'), während der Wichtig-Block direkt darunter bereits die erweiterte Fassung (Entwurf/Prototyp/Produkt) hatte - genau die Spannung, die eine Review in Runde 3 explizit anmerkte. Nachgezogen. Selbsttest 4 (UI-Zustände) und 7 (role=alert) trugen ebenfalls noch alte, in Runde 2/3 bereits präzisierte Formulierungen - angeglichen. Keine weiteren Fachfehler gefunden."
freigabe: "Final gesetzt nach 4 Runden (3 externe Prüfrunden + 1 eigene Abschlussprüfung, 2026-09-14) – Freigabe durch Autor:in bestätigt"
```