# LF10a Teil 3 – Sicherheit, Test & Abnahme

> **Zielgruppe:** Umschüler FIAE, 2. Lehrjahr (Aufbaustufe), Dualis-Institut Hamburg
> **Prüfungsrelevanz:** Schwerpunkt AP2 im Prüfungsbereich "Planen eines Softwareproduktes" (schriftlich, 90 Minuten, 10% Gewichtung) – insbesondere "Maßnahmen zur Qualitätskontrolle planen und durchführen"; Datenschutzbezug auch für WiSo relevant. Projektbezogene Inhalte (Abnahme, Dokumentation) können außerdem im Fachgespräch relevant werden, sofern sie Teil der eigenen betrieblichen Projektarbeit sind.
> **Lernzeit:** Ca. 2,5–3 Stunden Lesen/Durcharbeiten, +1,5–2 Stunden für Teststrategie-/Abnahme-Übungen
> **Status:** Final
> **Stand:** 2026-09-14
>
>Die 🔴/🟡/🟢-Markierungen in diesem Artikel sind eine didaktische Einschätzung zur Prüfungsvorbereitung, keine offizielle IHK-Gewichtung.
>
> **Hinweis zur Abgrenzung:** Letzter Teil der dreiteiligen LF10a-Serie (Teil 1: Konzeption & Struktur; Teil 2: Interaktion & Daten; Teil 3: Sicherheit, Test & Abnahme). Die WCAG-2.2-AA-Prüfwerttabelle in Abschnitt 2.3 ist inhaltsgleich mit der in Teil 1 (Abschnitt 4.4) eingeführten Tabelle – **Teil 1 ist die Pflegeautorität** für diese Werte, hier wird sie nur für die Testperspektive angewendet.

---

## IHK-Kernfragen dieses Artikels

| # | Frage | Abschnitt |
| --- | --- | --- |
| 1 | Wie unterscheiden sich Identity, Authentication, Authorization und Rolle, und warum ist Ausblenden keine Authorization? | [→ 1. Rollen, Datenschutz und sichere UI](#1-rollen-datenschutz-und-sichere-ui) |
| 2 | Wie wird eine risikoorientierte Teststrategie aufgebaut, und was leistet eine manuelle Accessibility-Prüfung gegenüber einem automatisierten Scan? | [→ 2. Funktions-, UX- und Accessibility-Tests](#2-funktions-ux--und-accessibility-tests) |
| 3 | Was macht einen Auslieferungsstand reproduzierbar, und welche Abnahmeentscheidungen gibt es? | [→ 3. Gesamtauftrag, Dokumentation und Abnahme](#3-gesamtauftrag-dokumentation-und-abnahme) |

---

## 1. Rollen, Datenschutz und sichere UI

> **Grundprinzip:** Ein Museumswärter, der am Eingang den Ausweis kontrolliert (Authentication), muss trotzdem an jeder Vitrine erneut prüfen, ob genau diese Person genau diesen Raum betreten darf (Authorization) – der Ausweis am Eingang beantwortet nur, wer da ist, nicht was diese Person darf.

### 1.1 Identity, Authentication, Authorization, Rolle

Für diesen Abschnitt werden bewusst die englischen Fachbegriffe verwendet – nicht aus Stilgründen, sondern weil das Deutsche hier zwei Wörter (Authentisierung/Authentifizierung) für denselben Vorgang kennt, deren Zuordnung in der Fachliteratur uneinheitlich gehandhabt wird. Im Englischen gibt es nur **ein** Wort, "authentication", das den gesamten Vorgang (Nachweis **und** Prüfung) abdeckt – und genau dieser englische Begriff ist auch der, mit dem in der Praxis tatsächlich gearbeitet wird: in Bibliotheken (`authenticate()`-Middleware), Protokollen (OAuth, OpenID Connect), HTTP-Status (401 Unauthorized) und Dokumentation. 🔴

| Begriff | Bedeutung | IHK-Relevanz |
| --- | --- | --- |
| **Identity** | Wer ein System nutzt | 🔴 |
| **Authentication** | Eine Identität wird behauptet und durch einen Nachweis (Passwort, Zertifikat, Token) belegt und geprüft – Nachweis und Prüfung werden hier bewusst nicht künstlich in zwei deutsche Einzelbegriffe zerlegt | 🔴 |
| **Authorization** | Entscheidet, welche Aktion diese Identität ausführen darf | 🔴 |
| **Rolle** | Bündelt fachliche Verantwortungen und Berechtigungen | 🟡 |

> **Hinweis:** Wird in einer Prüfungsaufgabe ausdrücklich nach "Authentisierung" oder "Authentifizierung" gefragt, bezieht sich das im Kern auf denselben Vorgang wie hier "Authentication" – die feinere deutsche Zweiteilung (Nachweis durch die Person vs. Prüfung durch das System) kann je nach Quelle unterschiedlich zugeordnet sein und muss deshalb nicht auswendig in einer bestimmten Richtung memoriert werden. Entscheidend ist das Verständnis des Ablaufs, nicht die exakte deutsche Wortwahl.

In einem echten System prüft die Anmeldung zunächst die behauptete Identität; danach hält eine zeitlich begrenzte Sitzung oder ein **Zugangstoken** den Authentication-Zustand. Bei jeder geschützten Anfrage muss der Server die Authorization trotzdem verbindlich durchsetzen und darf sich nicht darauf verlassen, dass die UI eine Funktion ausgeblendet hat oder dass die Berechtigung bereits einmalig bei der Anmeldung geprüft wurde – die konkrete technische Umsetzung (z. B. über Sitzungsdaten, Token-Claims oder ein Policy-System) kann dabei unterschiedlich aussehen. 🔴 Ein Zugangstoken ist ein zeitlich und im Verwendungsbereich begrenztes Zugriffsmittel, das der Server validiert; es gehört weder in URL-Parameter noch in sichtbare Fehlermeldungen, Logausgaben oder ein Repository. 🔴

### 1.2 Rollen-/Aktions-/Datenmatrix

Je Rolle wird dokumentiert: fachlich erlaubte Aktion, benötigte Daten, ausdrücklich **nicht** benötigte Daten, erwartete UI-Darstellung, serverseitig zu erzwingende Regel.

> **Wichtig für die Prüfung:** Ausblenden oder Deaktivieren verbessert die Orientierung, ist aber **keine Authorization** (direkter Anknüpfungspunkt an Teil 1 und Teil 2, wo dieselbe Warnung bereits für Navigation bzw. Clientvalidierung galt). Ein direkter Aufruf oder eine manipulierte Anfrage darf geschützte Aktionen nicht ermöglichen – das kann nur ein Backend verbindlich prüfen. Unterschiedliche Menüs allein sind **kein vollständiges Rollenmodell**, da offenbleibt, welche Zugriffe wirklich erlaubt sind, solange Daten und serverseitige Regel nicht mitdokumentiert sind.

### 1.3 Datenschutz systematisch prüfen

Für jedes personenbezogene Datenfeld: Zweck, Erforderlichkeit, Rechtsgrundlage, Transparenz, Sichtbarkeit, Aufbewahrungsfrage. **Datenminimierung** bedeutet nach Art. 5 Abs. 1 lit. c DSGVO, personenbezogene Daten auf das für den Zweck notwendige Maß zu beschränken. Ob eine konkrete Verarbeitung insgesamt rechtmäßig ist, hängt von weiteren Voraussetzungen ab und wird im Lernprojekt **nicht pauschal entschieden** – unklare Rechtsgrundlagen, Aufbewahrungsfristen oder Informationspflichten bleiben als offene Entscheidung sichtbar, statt vorschnell "gelöst" zu werden.

> **Wichtig für die Prüfung:** "Einwilligung" ist keine automatisch passende Rechtsgrundlage für jedes Datenfeld – die Entscheidung hängt von Zweck, Erforderlichkeit und einer tatsächlich passenden Rechtsgrundlage ab. Eine pauschale Berufung auf Einwilligung überspringt diese Prüfung und kann verschleiern, dass ein Feld für den Zweck gar nicht benötigt wird. 🟡

### 1.4 Sichere Ein- und Ausgabegrenzen

Nutzereingaben sind **Daten, kein ausführbarer Code**. Für dynamische Inhalte ist `textContent` die sichere Standardwahl; `innerHTML` interpretiert Markup und darf nicht mit ungeprüften Eingaben verwendet werden.

```js
output.textContent = userInput; // sicher: wird als Text dargestellt, nicht als Markup interpretiert
```

Weitere Prüfpunkte: keine Geheimnisse in URL, Konsole oder UI-Fehlerdetails; serverseitige Validierung bleibt erforderlich, Ausgabe-Kodierung ist zusätzlich kontextabhängig dort nötig, wo Werte tatsächlich eingefügt werden (Backend wie Frontend); verständliche, aber nicht informationsreiche Zurückweisung; keine realen Personen- oder Zugangsdaten in Tests.

> **IHK-Typfrage:** *"Eine Testeingabe `<strong>Admin</strong>` wird in einem Statusfeld über `output.innerHTML = userInput` ausgegeben. Bewerten Sie das Risiko und schlagen Sie eine sichere Alternative vor."*
> **Musterantwort:** `innerHTML` interpretiert den Eingabewert als HTML-Markup statt als reinen Text – die Zeichenkette `<strong>Admin</strong>` würde also tatsächlich fett dargestellt, statt wörtlich angezeigt zu werden. Bei ungeprüften Nutzereingaben entsteht dadurch eine HTML-Injection- und potenziell eine **Cross-Site-Scripting (XSS)**-Gefahr – nicht zwangsläufig, weil ein einfach eingefügtes `<script>`-Element automatisch ausgeführt würde, sondern weil andere aktive Konstrukte wie Event-Handler-Attribute (z. B. `onerror`) auf diesem Weg in die Seite gelangen können. Die sichere Alternative ist `output.textContent = userInput`: `textContent` interpretiert den Wert grundsätzlich als reinen Text, unabhängig vom Inhalt der Zeichenkette. Serverseitige Validierung bleibt zusätzlich erforderlich; Ausgaben müssen außerdem dort sicher behandelt werden, wo sie tatsächlich eingefügt werden (als HTML, Attribut, URL oder JavaScript-Wert) – Ausgabesicherheit ist kontextabhängig, nicht allein eine Backend-Aufgabe. 🔴

### 1.5 Missbrauchstests und die Grenze der Simulation

Ein Missbrauchstest untersucht bewusst abweichende Nutzung: Rollenwechsel, direkter Ansichtsaufruf, manipulierte Eingabe, abgelaufener Zustand, unnötige Datensichtbarkeit. Im UI-Projekt wird die **sichtbare Reaktion** geprüft und dokumentiert, was erst mit einem echten Backend bewertbar ist. Testfälle verwenden ausschließlich die eigene Anwendung und synthetische Daten – es werden **keine fremden Systeme** gescannt oder angegriffen.

> **Wichtig für die Prüfung:** Ein lokaler Rollenschalter simuliert nur einen UI-Zustand – er prüft keine Identität und schützt keine Daten. Wird die Simulation als echte Anmeldung bezeichnet oder dargestellt, entsteht ein **falsches Sicherheitsversprechen**. Das Merksatz-Prinzip: Die UI zeigt erlaubte Wege verständlich an; vertrauenswürdige Authorization und Datenbegrenzung müssen im Gesamtsystem durchgesetzt werden.

---

## 2. Funktions-, UX- und Accessibility-Tests

> **Grundprinzip:** Ein Pilot, der vor jedem Flug dieselben zehn Standardpunkte prüft, obwohl das eigentliche Risiko in einem neu eingebauten Bauteil liegt, verschwendet Prüfzeit an der falschen Stelle. Eine Teststrategie beginnt deshalb mit der Frage, welches Risiko wie zuverlässig untersucht werden muss – nicht mit einer Werkzeugliste.

### 2.1 Teststrategie, Rückverfolgbarkeit und Testarten

Eine **Teststrategie** erklärt, was mit welcher Testart, Priorität und Begründung geprüft wird – sie beginnt bei Anforderungen und Risiken, nicht bei einem Werkzeug. **Rückverfolgbarkeit** verbindet jede Anforderung mit ihrem Risiko, den zugehörigen Testfällen und Ergebnissen; wird ein Befund korrigiert, gehören Korrekturentscheidung und Nachtest ebenfalls in diese Kette.

| Testart | Prüft | IHK-Relevanz |
| --- | --- | --- |
| **Funktionstest** | Beobachtbares Sollverhalten | 🔴 |
| **Zustandstest** | Normal-, Leer-, Lade-, Fehler-, Erfolgszustände (vgl. Teil 1, Abschnitt 2.3) | 🔴 |
| **Tastaturtest** | Erreichbarkeit, Reihenfolge, Fokus | 🟡 |
| **Accessibility-Prüfung** | Ausgewählte WCAG-bezogene Kriterien, manuell und werkzeuggestützt | 🟡 |
| **UX-Test** | Wie Personen realistische Aufgaben bearbeiten | 🟡 |
| **Browser-Engine-Vergleich** | Technische Abweichungen zwischen Rendering-/JavaScript-Engines | 🟢 |

> **Wichtig für die Prüfung:** Automatisierung eignet sich für wiederholbare, eindeutig prüfbare Regeln – sie kann **nicht zuverlässig** entscheiden, ob eine Beschriftung fachlich verständlich, eine Reihenfolge sinnvoll oder eine Fehlermeldung hilfreich ist. 🔴 Bei GUI-Tests werden **Darstellung** (Anordnung, Kontrast, Verhalten bei unterschiedlichen Breiten) und **Funktionalität** (Navigation, Ereignisse, Zustandswechsel) als zwei verbundene, aber unterschiedliche Bereiche geprüft: Ein Element kann korrekt aussehen und trotzdem fachlich falsch reagieren, oder technisch korrekt arbeiten, während seine Rückmeldung nicht wahrnehmbar oder das Element nicht erreichbar ist. 🟡

### 2.2 Gute Funktionsfälle

Ein vollständiger Testfall enthält: stabile Test-ID und Anforderungsbezug, Vorbedingung/Testdaten, reproduzierbare Schritte, eindeutiges Soll, tatsächliches Ist und Beleg, Status. Abgedeckt werden positive, negative, Grenz-, Abbruch-, Rollen- und Zustandsfälle – "funktioniert" allein ist **kein** ausreichendes Soll.

### 2.3 Manuelle Accessibility-Prüfung

Für die Nachweise gelten dieselben WCAG-2.2-AA-Werte wie in Teil 1 (Abschnitt 4.4 dort ist die Pflegeautorität):

| Prüfgegenstand | Erfolgskriterium | Prüfwert |
| --- | --- | --- |
| Textkontrast | 1.4.3 | mind. 4,5:1 (mind. 3:1 bei großem Text) |
| Textvergrößerung | 1.4.4 | bis 200% ohne Inhalts-/Funktionsverlust |
| Reflow | 1.4.10 | bei vertikal scrollendem Inhalt bis 320 CSS-Pixel Breite grundsätzlich ohne Verlust und ohne zweidimensionales Scrollen (bei horizontaler Leserichtung entsprechend 256 CSS-Pixel Höhe); ausgenommen sind Inhaltsteile, deren Nutzung oder Bedeutung ein zweidimensionales Layout tatsächlich erfordert – "breit" allein ist keine Ausnahme |
| Nichttextkontrast | 1.4.11 | mind. 3:1 für wesentliche grafische Objekte/Bedienelemente, soweit für Verständnis/Bedienung erforderlich |
| Sichtbarer Fokus | 2.4.7 | jedes tastaturbedienbare Element hat sichtbaren Fokusindikator |
| Fokus nicht verdeckt (neu in WCAG 2.2) | 2.4.11 | Ein fokussiertes Element darf durch vom Autor erstellten Inhalt nicht vollständig verdeckt sein |
| Zielgröße | 2.5.8 | mind. 24×24 CSS-Pixel; kleinere Ziele nur über definierte Ausnahmen (u. a. messbare Abstandsausnahme, gleichwertige Alternative, Inline-Ziele, User-Agent-Steuerung, wesentliche Darstellung) |
| Statusmeldungen | 4.1.3 | programmatisch ermittelbar, ohne dafür Fokus zu erhalten |

Ergänzend geprüft werden: logische Reihenfolge, zugängliche Namen/sichtbare Beschriftungen, vollständiger Tastaturdurchlauf. Ein automatisches Werkzeug **ergänzt** diese Prüfung, ersetzt sie aber nicht – Version und Grenzen des Werkzeugs werden dokumentiert. Diese Tabelle ist wie in Teil 1 **kein vollständiger WCAG-Konformitätsnachweis**, sondern eine ausgewählte, modulübergreifend wiederkehrende Referenz.

### 2.4 Moderierter UX-Test

UX-Tests verwenden realistische Zielaufgaben ohne Bedienhinweise. Drei Schritte werden getrennt gehalten: **Beobachtung** ("Person öffnet dreimal die Navigation"), **Interpretation** ("Der Einstieg wird möglicherweise nicht erkannt"), **Vorschlag** ("Primäraktion eindeutiger benennen und erneut testen").

> **Wichtig für die Prüfung:** Ein UX-Test untersucht die tatsächliche Nutzung und deckt auch unerwartete Verständlichkeits-/Bedienprobleme auf. Ein **Akzeptanztest** (vgl. Teil 1, Abschnitt 2.5) prüft dagegen, ob vorab vereinbarte Anforderungen/Akzeptanzkriterien erfüllt sind. Ein UX-Befund kann eine neue Anforderung oder Verbesserung auslösen – er ist **nicht automatisch** ein fehlgeschlagenes Akzeptanzkriterium, da er oft über das ursprünglich Vereinbarte hinausgeht.

Die Verfahrenswahl richtet sich nach der Untersuchungsfrage (Kontextanalyse, Fokusgruppe, Online-Befragung, expertenbasierte Überprüfung/heuristische Evaluation, Labortest, Eye-Tracking – vgl. Teil 1, Abschnitt 2.5): Fokusgruppe und Befragung zeigen **geäußerte** Einschätzungen, Kontext- und Labortest zeigen **beobachtbares** Verhalten, Eye-Tracking zeigt, wohin eine Person blickt, aber nicht automatisch warum.

### 2.5 Browser-Engine-Vergleich, Befunde und Regression

> **Typische Prüfungsfalle:** Zwei Browser mit derselben zugrunde liegenden Engine (z. B. zwei Chromium-basierte Browser) sind **kein vollständiger Engine-Vergleich** – sie teilen viele technische Eigenschaften. Ein echter Vergleich dokumentiert Browser, Engine (soweit bekannt), Version, Betriebssystem, Breite und Simulationsgrenze, und trennt Darstellungs-, Fokus-, Eingabe- und Funktionsabweichungen.

Ein Befund enthält Auswirkung, Reproduktionsweg, Priorität, Verantwortlichkeit, Entscheidung. **Blockierend** bedeutet, dass ein Kernziel oder eine sichere Nutzung nicht möglich ist – hohe Priorität sollte nicht inflationär vergeben werden. Nach einer Korrektur wird nicht nur der ursprüngliche Fall wiederholt, sondern auch benachbarte Funktionen geprüft (**Regressionstest**, vgl. Teil 2, Abschnitt 1).

---

## 3. Gesamtauftrag, Dokumentation und Abnahme

> **Grundprinzip:** Ein Umzugsunternehmen, das am Zielort Kisten mit unklarer, teils widersprüchlicher Beschriftung abstellt, hat zwar "geliefert" – aber niemand kann ohne Rückfrage sagen, was fertig gepackt und was noch unvollständig ist. Konsolidierung schafft genau diese Eindeutigkeit vor der Übergabe.

### 3.1 Konsolidierung und Rückverfolgbarkeitsmatrix

Eine **Rückverfolgbarkeitsmatrix** verbindet: Muss-Anforderung, umgesetzten UI-Bereich, zugehörigen Test/Ergebnis, Status (umgesetzt/teilweise/nicht umgesetzt/nicht prüfbar), Restabweichung und Entscheidung.

> **Wichtig für die Prüfung:** Nicht umgesetzte Anforderungen werden **nicht aus der Matrix entfernt** – sie zeigen Restumfang und Risiko. Würden sie entfernt, wirkt die Abdeckung besser als sie tatsächlich ist, und die Abnahme kann keine informierte Entscheidung treffen. Veraltete Wireframes oder doppelte Dateien werden nicht still gelöscht oder neben dem aktuellen Stand belassen – stattdessen wird eine eindeutige Pflegeautorität (und ggf. ein Archivstatus) gekennzeichnet.

### 3.2 Reproduzierbarer Auslieferungsstand

**Reproduzierbar** heißt: Eine fremde Person kann mit der Dokumentation denselben demonstrierbaren Stand starten – ohne mündliches Zusatzwissen. Dazu gehören: Voraussetzungen/unterstützte Umgebung, Bezugs-/Installationsschritte, eindeutiger Startbefehl, freigegebene synthetische Testdatenquelle, dokumentierte Testkonten für eine nicht-produktive Umgebung (ohne echte Passwörter oder produktive Zugangsdaten), bekannte Einschränkungen, erwartetes Ergebnis eines **Smoke-Tests** (kurzer Durchlauf der wichtigsten Start-/Kernfunktionen – ersetzt keine vollständigen Funktions-, Accessibility- oder Regressionstests). "Bei uns läuft es" ist **kein Nachweis**.

### 3.3 Nutzer- und Entwicklungsdokumentation

| Zielgruppe | Inhalt |
| --- | --- |
| **Nutzerkurzanleitung** | Aufgabe/Einstieg, Kernabläufe in fachlicher Sprache, erkennbare Zustände/Korrekturwege, Hilfe-/Kontaktstelle – keine unnötigen internen Implementierungsdetails |
| **Entwicklungsdokumentation** | Struktur/zentrale Entscheidungen, Designsystem/Datenvertrag, Rollen-/Sicherheitsgrenze, Testausführung/bekannte Grenzen, Erweiterungspunkte, Bibliotheken/Module mit Version/Quelle/Lizenz/Auswahlgrund |

> **Wichtig für die Prüfung:** Offene Backendaufgaben bleiben offen dokumentiert – eine UI-Simulation darf **nicht** als fertige Persistenz, Authentication oder Authorization beschrieben werden (direkter Bezug zu Abschnitt 1.5). 🔴 Wird ein Framework eingesetzt, wird zusätzlich der vorgegebene Anwendungsrahmen mit seinen Folgen für Start, Build, Test und Wartung dokumentiert – Bibliothek und Framework bleiben dabei begrifflich getrennt (vgl. Teil 2, Abschnitt 1.3). 🟡

### 3.4 Soll-Ist-Abnahme

Eine **Abnahme** bewertet den gelieferten Stand gegen vereinbarte Kriterien – kein allgemeines "Gefällt mir". Je Abnahmeszenario: Ausgangslage, Soll, Durchführung, Beleg, Ist, Abweichung. Für dieses Lern-/Projektmodell werden folgende drei Abnahmeentscheidungen verwendet (kein allgemeingültiger Rechtsbegriff, sondern ein operatives Bewertungsmodell für dieses Projekt):

| Entscheidung | Bedeutung im Lernprojekt | IHK-Relevanz |
| --- | --- | --- |
| **Abnahmefähig** | Vereinbarte Muss-Kriterien sind erfüllt; dokumentierte geringfügige Restabweichungen verhindern den vorgesehenen Einsatz nicht | 🔴 |
| **Bedingt abnahmefähig** | Der vereinbarte Zweck ist nur unter konkret dokumentierten Bedingungen oder Einschränkungen erreichbar | 🔴 |
| **Nicht abnahmefähig** | Eine blockierende Abweichung verhindert den vereinbarten Zweck | 🔴 |

> **IHK-Typfrage:** *"Ein Projekt erhält die Abnahmeentscheidung 'bedingt abnahmefähig'. Was fehlt dieser Formulierung noch, um vollständig zu sein?"*
> **Musterantwort:** "Bedingt abnahmefähig" allein ist nicht genau genug – ohne konkrete Bedingung, verantwortliche Stelle und Termin oder Auslöser kann niemand erkennen, wann die Einschränkung als erledigt gilt. Die Formulierung würde sonst nur eine offene Entscheidung verschieben, statt sie greifbar zu machen. Vollständig wäre z. B.: "Bedingt abnahmefähig für Demonstration mit synthetischen Daten; nicht für Produktivbetrieb, solange Persistenz und serverseitige Authorization fehlen – Bedingung: Umsetzung durch das Backend-Team bis [Termin], Verantwortlichkeit: [Rolle/Person]."

### 3.5 Nutzen, Aufwand und Sensitivität

Eine einfache Wirtschaftlichkeitsbetrachtung trennt dokumentierte Werte von Schätzungen. Beispielrechnung: Fälle pro Monat × geschätzte Minutenersparnis ÷ 60 = monatliche Zeitersparnis in Stunden, multipliziert mit einem begründeten kalkulatorischen Stundensatz. Eine **Sensitivitätsbetrachtung** verändert eine unsichere Annahme (z. B. Minutenersparnis halbieren) und zeigt, ob die Entscheidung stabil bleibt.

> **Wichtig für die Prüfung:** Ein berechneter Schätzwert ist **kein gemessener Fakt** – auch eine korrekte Formel bleibt von Annahmen abhängig. Werden Fallzahl oder Zeitersparnis nicht gekennzeichnet und variiert, behauptet die Zahl eine Genauigkeit, die nicht belegt ist.

### 3.6 Präsentation und Übergabe

Entscheidungslinie: (1) Problem/Nutzergruppen, (2) wichtigste Anforderung/Gestaltung, (3) Live-Demo der Kernabläufe, (4) Testbelege/Korrekturen, (5) Accessibility-/Datenschutz-/Backendgrenzen, (6) Nutzen/Aufwand und Abnahmeempfehlung. Die Demo braucht einen Rückfallpfad (reproduzierbarer lokaler Stand oder vorab dokumentierter Ablauf) – Folien über ein Produkt ersetzen nicht die Produktdemonstration.

---

## 4. Zusammenspiel der Konzepte in LF10a Teil 3

Die drei Module schließen die LF10a-Serie ab, indem sie die in Teil 1 und Teil 2 entwickelte Oberfläche auf Vertrauenswürdigkeit prüfen und übergabefähig machen: **Rollen, Datenschutz und sichere UI** (Abschnitt 1) ziehen die Grenze zwischen dem, was die Oberfläche anzeigen darf, und dem, was nur ein Backend verbindlich durchsetzen kann – dieselbe Grenze, die in Teil 1 (Navigation) und Teil 2 (Clientvalidierung) bereits als "Ausblenden/Client-geprüft ist nicht geschützt" auftauchte. **Funktions-, UX- und Accessibility-Tests** (Abschnitt 2) wenden die in Teil 1 eingeführten UI-Zustände und die WCAG-Tabelle konkret auf die Testphase an. **Gesamtauftrag, Dokumentation und Abnahme** (Abschnitt 3) fasst alles zusammen: Eine Abnahme ist nur so belastbar wie die Rückverfolgbarkeit zwischen Anforderung (Teil 1), Umsetzung (Teil 1/2) und Testbeleg (Abschnitt 2) – und eine ehrliche Übergabe benennt offen, wo UI-Simulation endet und echte Backend-Absicherung erst beginnen müsste.

---

## 5. Typische Prüfungsfallen

| # | Falle | Richtigstellung | IHK-Relevanz |
| --- | --- | --- | --- |
| 1 | Nach erfolgreicher Anmeldung ist jede weitere Aktion automatisch autorisiert | Der Server setzt bei jeder geschützten Aktion die Authorization verbindlich durch – Authentication und Authorization sind getrennte, wiederkehrende Prüfungen | 🔴 |
| 2 | Unterschiedliche Menüs pro Rolle sind ein vollständiges Rollenmodell | Ohne Zuordnung von Aktion, Daten und serverseitiger Regel bleibt offen, welche Zugriffe wirklich erlaubt sind | 🔴 |
| 3 | Einwilligung ist immer eine passende Rechtsgrundlage | Die Wahl hängt von Zweck, Erforderlichkeit und einer tatsächlich passenden Rechtsgrundlage ab – pauschale Einwilligung kann verschleiern, dass ein Feld gar nicht benötigt wird | 🟡 |
| 4 | Freitext darf mit `innerHTML` ausgegeben werden | `innerHTML` interpretiert Markup – für reinen Text ist `textContent` die sichere Standardwahl | 🔴 |
| 5 | Ein lokaler Rollenschalter beweist Authentication | Er simuliert nur einen UI-Zustand, prüft keine Identität und schützt keine Daten | 🔴 |
| 6 | Ein grüner Accessibility-Scan ist ein vollständiger Nachweis | Automatisierung erkennt nur ausgewählte technische Regeln, nicht Verständlichkeit, Fokusreihenfolge-Sinnhaftigkeit oder tatsächliche Tastaturbedienung | 🔴 |
| 7 | Zwei Browser mit derselben Engine sind ein vollständiger Engine-Vergleich | Sie teilen viele technische Eigenschaften – ein echter Vergleich dokumentiert tatsächlich unterschiedliche Engines | 🟡 |
| 8 | Ein UX-Befund ist automatisch ein fehlgeschlagenes Akzeptanzkriterium | UX-Test und Akzeptanztest beantworten unterschiedliche Fragen – ein UX-Befund kann eine neue Anforderung auslösen, ohne ein Kriterium zu verfehlen | 🟡 |
| 9 | Nicht umgesetzte Anforderungen gehören nicht in die Rückverfolgbarkeitsmatrix | Gerade sie zeigen Restumfang und Risiko – ihr Fehlen würde die Abdeckung besser erscheinen lassen, als sie ist | 🔴 |
| 10 | Eine überzeugende Demo ist bereits produktionsreif | Lokale Fixtures/Rollenschalter zeigen Abläufe, beweisen aber weder Persistenz noch Authentication/Authorization | 🔴 |

---

## 6. Vertiefung und Ausblick (freiwillig – für den ersten Durchgang nicht erforderlich)

- **OAuth/OpenID Connect im Detail**: Wie Authentication und Authorization in produktiven Systemen über etablierte Protokolle statt eigener Lösungen umgesetzt werden.
- **Content Security Policy (CSP)**: Wie eine zusätzliche Browser-Schutzschicht auch bei Fehlern in der Ausgabe-Kodierung greifen kann.
- **Automatisierte Accessibility-Testwerkzeuge im Detail**: Wie Tools wie axe-core oder Lighthouse technisch funktionieren und wo genau ihre Erkennungsgrenzen liegen.
- **A/B-Tests als UX-Verfahren**: Wie sich quantitative Vergleichsverfahren zu den in Abschnitt 2.4 genannten qualitativen Verfahren verhalten.
- **Explorative Testtechniken**: Strukturiertes, aber nicht vollständig vorab skriptgebundenes Testen als Ergänzung zu geplanten Testfällen.

---

## 7. Selbsttest

| # | Frage | Kurzantwort |
| --- | --- | --- |
| 1 | Was unterscheidet Authentication und Authorization? | Authentication prüft eine Identität; Authorization entscheidet über eine konkrete Aktion/einen Datenzugriff |
| 2 | Warum reicht Ausblenden einer UI-Funktion nicht als Zugriffsschutz? | Client-Code und Adressen lassen sich manipulieren – nur ein Backend kann eine Aktion verbindlich ablehnen |
| 3 | Was bedeutet Datenminimierung nach Art. 5 Abs. 1 lit. c DSGVO? | Personenbezogene Daten auf das für den Zweck notwendige Maß beschränken |
| 4 | Warum ist `textContent` für Nutzereingaben sicherer als `innerHTML`? | `textContent` interpretiert den Wert immer als reinen Text, `innerHTML` interpretiert ihn als Markup |
| 5 | Was unterscheidet Darstellung und Funktionalität bei einem GUI-Test? | Darstellung: Anordnung/Kontrast/visuelle Konsistenz; Funktionalität: Navigation/Ereignisse/Zustandswechsel |
| 6 | Was unterscheidet UX-Test und Akzeptanztest? | UX-Test untersucht tatsächliche Nutzung; Akzeptanztest prüft vorab vereinbarte Kriterien |
| 7 | Was macht einen Auslieferungsstand reproduzierbar? | Voraussetzungen, Startbefehl, Testdaten, bekannte Einschränkungen und Smoke-Test sind so dokumentiert, dass eine fremde Person ohne Zusatzwissen starten kann |
| 8 | Nenne die drei Abnahmeentscheidungen dieses Lernprojekt-Modells. | Abnahmefähig, bedingt abnahmefähig, nicht abnahmefähig – ein operatives Modell für dieses Projekt, kein allgemeingültiger Rechtsbegriff |
| 9 | Warum ist ein berechneter Nutzen-Schätzwert kein gemessener Fakt? | Er bleibt von Annahmen (Fallzahl, Zeitersparnis) abhängig – eine Sensitivitätsbetrachtung prüft, ob die Empfehlung bei veränderten Annahmen stabil bleibt |

---

## 8. IHK-Cheatsheet

| Begriff | Kurzdefinition |
| --- | --- |
| **Identity/Authentication/Authorization/Rolle** | Wer nutzt / Identität wird nachgewiesen+geprüft / Aktion wird erlaubt / bündelt Verantwortung |
| **Zugangstoken** | Zeitlich/im Umfang begrenztes Zugriffsmittel, vom Server validiert – nicht in URL/Fehlermeldung/Logausgabe/Repository |
| **Rollen-/Aktions-/Datenmatrix** | Aktion + benötigte/nicht benötigte Daten + UI-Darstellung + serverseitige Regel je Rolle |
| **Ausblenden ≠ Authorization** | Verbessert Orientierung, ersetzt keine serverseitige Prüfung |
| **Datenminimierung** | Art. 5 Abs. 1 lit. c DSGVO – Daten auf notwendiges Maß beschränken |
| **textContent vs. innerHTML** | Text sicher darstellen / Markup interpretieren (Risiko bei ungeprüften Eingaben) |
| **Teststrategie** | Was wird mit welcher Testart/Priorität/Begründung geprüft – beginnt bei Risiken, nicht beim Werkzeug |
| **Rückverfolgbarkeit** | Anforderung ↔ Risiko ↔ Testfall ↔ Ergebnis ↔ Korrektur/Nachtest |
| **Darstellung vs. Funktionalität** | Zwei getrennte, verbundene Prüfbereiche bei GUI-Tests |
| **Beobachtung/Interpretation/Vorschlag** | Drei zu trennende Schritte im UX-Test |
| **Smoke-Test** | Kurzer Durchlauf der Kernfunktionen – ersetzt keine vollständigen Tests |
| **Rückverfolgbarkeitsmatrix** | Muss-Anforderung ↔ UI-Bereich ↔ Test/Ergebnis ↔ Status ↔ Restabweichung |
| **Abnahmefähig/bedingt/nicht abnahmefähig** | Drei operative Abnahmeentscheidungen dieses Lernprojekt-Modells (kein universeller Rechtsbegriff) – "bedingt" braucht Bedingung + Verantwortlichkeit + Termin |
| **Sensitivitätsbetrachtung** | Prüft, ob eine Nutzen-/Aufwandsempfehlung bei veränderter Annahme stabil bleibt |

---

## 9. Prüfungstaktik

| Aufgabentyp | Typische Formulierung | Was die Prüfung hören will |
| --- | --- | --- |
| Rollenmodell bewerten | "Bewerten Sie dieses Rollenkonzept" | Konkret benennen, was serverseitig zusätzlich durchgesetzt werden muss |
| Datenschutzfrage einordnen | "Ist dieses Feld erforderlich?" | Zweck, Erforderlichkeit und Rechtsgrundlage getrennt benennen, nicht pauschal entscheiden |
| Sichere Ausgabe bewerten | "Bewerten Sie diese Codezeile" | Konkretes Risiko (Markup-Interpretation) benennen, sichere Alternative nennen |
| Teststrategie begründen | "Warum diese Testart für dieses Risiko?" | Auswirkung und Wahrscheinlichkeit des konkreten Fehlers als Begründung |
| Abnahmeentscheidung vervollständigen | "Ist diese Abnahme vollständig dokumentiert?" | Bedingung, Verantwortlichkeit, Termin/Auslöser einfordern |

---

## 10. Merksätze für das Fachgespräch

> Die UI zeigt erlaubte Wege verständlich an – vertrauenswürdige Authorization und Datenbegrenzung müssen im Gesamtsystem durchgesetzt werden.

> Ausgeblendet ist nicht geschützt, simuliert ist nicht authentisiert.

> Gute Tests beantworten eine begründete Risikofrage und hinterlassen einen reproduzierbaren Nachweis.

> Ein grüner Scan ist ein Teilnachweis, kein Beweis vollständiger Barrierefreiheit.

> Abnahmefähigkeit entsteht aus einem reproduzierbaren Stand, einer vollständigen Belegkette und offen dokumentierten Grenzen.

---

```yaml
dokument: LF10a-Teil3-wiki-artikel
lernfeld: "LF10a Teil 3 (Module 10a.8-10a.10)"
titel: "Sicherheit, Test & Abnahme"
typ: "Typ A – Kompakter Prüfungs-Wiki (FIAE-fokussiert)"
status: final
stand: 2026-09-14
quellen_intern:
  - "LF10a.8-10 Lernskripte (Rollen/Datenschutz/sichere UI, Funktions-/UX-/Accessibility-Tests, Gesamtauftrag/Dokumentation/Abnahme)"
  - "WCAG-2.2-AA-Tabelle in Abschnitt 2.3 bewusst inhaltsgleich mit der final korrigierten Tabelle aus LF10a Teil 1, Abschnitt 4.4 übernommen (dort explizit als Pflegeautorität gekennzeichnet, inkl. 2.4.11-Ergänzung aus Teil-1-Review) - Rohmaterial-Tabelle in LF10a.9 selbst war noch ohne 2.4.11, deshalb bewusst nicht wörtlich übernommen"
  - "Bewusste Rückbezüge zu Teil 1 (UI-Zustände, Ausblenden-ist-nicht-Authorization, Akzeptanztest-Abgrenzung) und Teil 2 (Clientvalidierung-ist-nicht-Sicherheit, Regressionstest, Bibliothek/Framework) statt Wiederholung"
quellen_fachlich:
  - titel: "Datenschutz-Grundverordnung (EU) 2016/679, Art. 5 Abs. 1 lit. c"
    herausgeber: "Europäische Union (EUR-Lex)"
    status: "Rohmaterial verlinkt Primärquelle; Artikelnummer und Grundsatz der Datenminimierung stabiles, etabliertes Recht, keine gesonderte Web-Verifikation nötig"
  - titel: "WCAG 2.2 (W3C)"
    herausgeber: "W3C"
    status: "Werte identisch zur bereits in LF10a Teil 1 web-verifizierten Tabelle übernommen, keine erneute Verifikation nötig"
  - titel: "Rollen-/Autorisierungsmodell, Teststrategie/Rückverfolgbarkeit, Abnahmekonzept"
    herausgeber: "Rohmaterial + etabliertes Software-Engineering-/Security-Standardwissen"
    status: "stabile Konzepte, keine Web-Verifikation nötig"
review_historie:
  - runde: 1
    datum: 2026-09-14
    ergebnis: "Erstdraft erstellt aus den drei LF10a.8-10-Lernskripten. Als ein zusammenhängender Artikel (Teil 3 von 3, letzter Teil der LF10a-Serie) verfasst. WCAG-Tabelle bewusst aus der final korrigierten Teil-1-Fassung übernommen statt aus der leicht abweichenden Rohmaterial-Fassung in LF10a.9 (die noch kein 2.4.11 enthielt), um serienweite Konsistenz zu wahren. Zahlreiche bewusste Querverweise zu Teil 1 und Teil 2 eingebaut, um die 'Ausblenden/Client-geprüft ist nicht sicher'-Kernlinie über die gesamte Serie sichtbar zu halten. Von Anfang an gehedgte Formulierungen basierend auf den umfangreichen Lektionen aus Teil 1 und Teil 2 Review-Historien."
  - runde: 2
    datum: 2026-09-14
    ergebnis: "3 Reviews eingearbeitet. Wichtigster Fund (2 Reviews, gut begründet): innerHTML/XSS-Musterantwort war zu pauschal ('Skriptcode wird eingebracht') - technisch präzisiert (Event-Handler-Attribute statt automatischer Skriptausführung, XSS-Begriff explizit benannt), Ausgabe-Kodierung nicht mehr pauschal als reine Backend-Pflicht dargestellt. Authentisierung/Authentifizierung-Behauptung einer Review eigenständig recherchiert: Quellen widersprechen sich tatsächlich über die Zuordnungsrichtung (BSI-Quellen und andere Quellen nennen teils entgegengesetzte Zuordnungen) - keine einseitige 'Korrektur' vorgenommen, stattdessen Hinweis auf uneinheitlichen Sprachgebrauch ergänzt. Abnahmekategorien (2 Reviews) explizit als Lernprojekt-Modell statt universelle Rechtsterminologie gekennzeichnet. Weitere Korrekturen: Reflow um 256px-Detail bei horizontaler Leserichtung ergänzt, Zielgröße-Ausnahmen vollständiger aufgezählt (dadurch entsteht eine geringe zusätzliche Präzisionsdifferenz zur kompakteren Teil-1-Tabelle - bewusst in Kauf genommen, da Teil 3 die Testperspektive vertieft, ohne der Teil-1-Tabelle zu widersprechen); 'neue' Autorisierungsentscheidung leicht entschärft (Durchsetzung statt zwingend neuer Datenbankabfrage); Zugangstoken-Definition präzisiert (Logausgabe ergänzt); Testidentitäten-Formulierung geklärt (keine produktiven statt 'ohne jegliche' Geheimnisse); Datenschutz-Prüfliste um Rechtsgrundlage ergänzt; Farbmarkierungs-Ankündigung entfernt, da im gesamten LF10a-Fließtext (auch Teil 1/2) faktisch nicht angewendet - serienweites Muster, hier lokal korrigiert statt falsche Erwartung zu wecken. Eine vorgeschlagene Änderung (sichtbarer-Fokus-Formulierung) wurde geprüft, aber zugunsten exakter Konsistenz mit der Teil-1-Pflegeautorität nicht übernommen."
  - runde: 3
    datum: 2026-09-14
    ergebnis: "Auf ausdrücklichen Wunsch des Auftraggebers zwei gezielte Nacharbeiten: (1) Authentisierung/Authentifizierung-Terminologie in Abschnitt 1.1 auf englische Fachbegriffe (Identity/Authentication/Authorization) umgestellt, da Englisch im Programmierkontext ohnehin die gebräuchliche, eindeutige Terminologie ist (OAuth, HTTP 401, authenticate()-Middleware) - löst die uneinheitliche deutsche Zuordnung elegant, statt sie nur zu kommentieren. Alle weiteren 'Autorisierung'-Fundstellen im Artikel konsistent auf 'Authorization' umgestellt. (2) IHK-Relevanz-Markierungen (🔴/🟡/🟢) tatsächlich in den Fließtext eingebaut (Begriffstabelle 1.1, Testarten-Tabelle 2.1, Abnahme-Tabelle 3.4, mehrere Wichtig-Blöcke) statt nur die Ankündigung zu entfernen - Auftraggeber plant, Teil 1 und Teil 2 in einem separaten Durchgang ebenfalls entsprechend nachzuziehen. WCAG-Tabelle in 2.3 bewusst ohne Relevanz-Spalte belassen, da Teil-1-Pflegeautorität ebenfalls keine hat."
  - runde: 4
    datum: 2026-09-14
    ergebnis: "Eigene Abschlussprüfung nach externem Re-Review (drei weitere Reviews, überwiegend Bestätigung, einige reine Stil-/Formatierungshinweise auf Weisung des Auftraggebers ignoriert, da keine Fachfehler). Zwei echte Selbstkonsistenz-Reste gefunden und behoben: Selbsttest 8 und der Cheatsheet-Eintrag zu den Abnahmeentscheidungen nannten die drei Kategorien noch ohne den in 3.4 bewusst ergänzten Modellcharakter-Vorbehalt ('kein universeller Rechtsbegriff, sondern Lernprojekt-Modell') - angeglichen. Keine weiteren Fachfehler gefunden."
freigabe: "Final gesetzt nach 4 Runden (3 externe Prüfrunden + 1 eigene Abschlussprüfung, 2026-09-14) – Freigabe durch Autor:in bestätigt. Serie LF10a (Teil 1-3) damit vollständig final."
```