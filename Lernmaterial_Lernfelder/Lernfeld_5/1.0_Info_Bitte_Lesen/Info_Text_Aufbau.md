# LF5 – Software Configuration and Data Management: Übersicht

> Diese Datei erklärt kurz, wie sich LF5 zusammensetzt und warum die Artikel aus unterschiedlichen Quellen stammen bzw. teilweise noch fehlen. Kein Lerninhalt, nur Orientierung.

---

## Warum die Struktur uneinheitlich aussieht

LF5 wurde **nicht komplett neu geschrieben**. Ein Teil der Unterthemen war bereits vor diesem Wiki-Projekt im Repo vorhanden (vermutlich aus einer früheren Bearbeitung), ein Teil wurde in diesem Projekt neu erstellt, und ein Teil steht noch aus. Um Doppelarbeit zu vermeiden, wurde beim Start dieses Projekts zunächst der bestehende Repo-Stand gesichtet, bevor neue Artikel geschrieben wurden.

## Statusübersicht

| Unterthema | Status | Herkunft |
| --- | --- | --- |
| **LF5.1** – Requirements & Analysis | ✅ Final | Neu erstellt in diesem Projekt |
| **LF5.2** – System Design Modeling (ERM/Chen-Notation, PAP, Nassi-Shneiderman, UML Use-Case/Klasse/Sequenz) | ✅ Vorhanden | Bereits im Repo, vor diesem Projekt entstanden |
| **LF5.3** – Development Tools & Version Control (IDE/Compiler/Git, Branching-Modelle) | ✅ Vorhanden | Bereits im Repo, vor diesem Projekt entstanden |
| **LF5.4** – Writing the Logic | ✅ Final | Neu erstellt in diesem Projekt |
| **LF5.5** – Ensuring Quality | ✅ Final | Neu erstellt in diesem Projekt |
| **LF5.6 Teil A** – SDLC Frameworks (Wasserfall, V-Modell, Spiralmodell) | ✅ Vorhanden | Bereits im Repo, vor diesem Projekt entstanden ("Vorgehensmodelle_Softwareentwicklung.md") |
| **LF5.6 Teil B** – Agile Methodologies (Scrum, Kanban, XP) | ⚪ Noch offen | Fehlt komplett, muss noch geschrieben werden |

## Warum LF5.2/5.3/5.6-Teil-A nicht in diesem Projekt neu geschrieben wurden

Beim Sichten des GitHub-Repos zu Beginn der LF5-Arbeit stellte sich heraus, dass diese Themen bereits in solider, teilweise sogar als "Final" markierter Qualität vorlagen. Ein Neuschreiben hätte nur Doppelarbeit bedeutet und das Risiko widersprüchlicher Parallelversionen geschaffen. Stattdessen wurde der Fokus auf die tatsächlich fehlenden Lücken gelegt (LF5.1, LF5.4, LF5.5) sowie auf den noch offenen Rest (LF5.6 Teil B).

## Bewusste Abgrenzung zu LF8

LF8 baut inhaltlich auf LF5 auf (nicht umgekehrt) – LF5 legt die Grundlagen (Requirements Engineering, UML-Notation, Sprachparadigmen, Python-Basics, Git, Testing), LF8 vertieft darauf aufbauend (z. B. OOP-Design-Patterns, Client-Server-Architektur). Wo sich Themen berühren (v. a. UML-Klassendiagramme, OOP-Grundlagen), verweisen die LF5-Artikel bewusst auf LF8 statt Inhalte zu duplizieren, und umgekehrt.

---

*Stand: 2026-09-11*

*Zusatz* ***WICHTIG***

# Infoblatt: Katalog-Update 2025 – Was sich für bestehende Wiki-Artikel ändert

> **Zweck dieses Blatts:** Kurze, eigenständige Ergänzung zu drei bestehenden Wiki-Dateien, die durch den neuen AP2-Prüfungskatalog (gültig seit Sommer 2025) fachlich überholte Prüfungsrelevanz-Aussagen enthalten. Die Originaldateien werden **nicht verändert** – dieses Blatt kann als Verweis/Anhang danebengelegt oder bei Gelegenheit als kurzer Einschub in die jeweilige Datei übernommen werden.
>
> **Stand:** 2026-09-15

---

## 1. Betrifft: `Lernfeld_5/Programmablaufplan (PAP - DIN 66001).md`

**Im Text steht sinngemäß:** "...ob am Whiteboard oder in der IHK-Prüfung."

**Aktueller Stand:** Der Programmablaufplan (PAP) wurde mit dem AP2-Prüfungskatalog 2025 aus dem Katalog für Fachinformatiker Anwendungsentwicklung **gestrichen**. Für die Darstellung von Kontrollstrukturen sind seither nur noch **Pseudocode** und das **UML-Aktivitätsdiagramm** vorgesehen.

**Vorgeschlagener Einschub** (z. B. als Hinweisbox direkt unter der Überschrift):

> ⚠️ **Hinweis zum aktuellen Prüfungsstand (2025):** Der PAP wurde aus dem AP2-Prüfungskatalog für Anwendungsentwicklung gestrichen und ist **nicht mehr Bestandteil der schriftlichen Prüfung**. Als Struktur- und Verständnishilfe (z. B. zur eigenen Vorbereitung oder im Fachgespräch) bleibt er weiterhin nützlich. Für die Prüfung selbst sind **Pseudocode** und das **UML-Aktivitätsdiagramm** die relevanten Notationen – siehe *Pseudocode & Algorithmenstrukturen*.

---

## 2. Betrifft: `Lernfeld_5/Nassi_Schneider_Diagramm.md`

**Aktueller Stand:** Das Struktogramm (Nassi-Shneiderman-Diagramm) wurde ebenfalls mit dem Katalog 2025 gestrichen.

**Vorgeschlagener Einschub** (analog zu PAP):

> ⚠️ **Hinweis zum aktuellen Prüfungsstand (2025):** Das Struktogramm wurde aus dem AP2-Prüfungskatalog für Anwendungsentwicklung gestrichen und ist **nicht mehr Bestandteil der schriftlichen Prüfung**. Es bleibt als didaktisches Hilfsmittel zum Verständnis von Kontrollstrukturen wertvoll, wird aber in aktuellen Prüfungsaufgaben nicht mehr verlangt. Relevant sind seither **Pseudocode** und das **UML-Aktivitätsdiagramm**.

---

## 3. Betrifft: `SQLITE/3.SQLite_in_der_IHK_Prüfung.md`

### 3.1 Gewichtungsangabe präzisieren

**Im Text steht:** "50 Prozent deiner Abschlussnote macht die betriebliche Projektarbeit aus... Die schriftliche Prüfung (Teil 2) steuert weitere 30 Prozent bei."

**Prüfung:** Die 50%-Angabe für die Projektarbeit ist korrekt. Die 30%-Angabe ist **nicht falsch, aber missverständlich**: Sie stimmt nur, wenn man alle drei schriftlichen AP2-Einzelbereiche zusammenzählt (Planen eines Softwareproduktes 10% + Entwicklung und Umsetzung von Algorithmen 10% + Wirtschafts- und Sozialkunde 10% = 30%). Ohne diese Aufschlüsselung liest es sich wie eine einzelne "schriftliche Prüfung".

**Vorgeschlagene Präzisierung:**

> Die betriebliche Projektarbeit macht 50% deiner Abschlussnote aus. Die drei schriftlichen AP2-Einzelbereiche – Planen eines Softwareproduktes (10%), Entwicklung und Umsetzung von Algorithmen (10%) und Wirtschafts- und Sozialkunde (10%) – summieren sich auf weitere 30%. Die übrigen 20% stammen aus AP1.

### 3.2 Belegsatz-Konzept ergänzen

**Fehlt aktuell:** Der Text erwähnt SQL-Prüfungsaufgaben, aber nicht das seit 2025 gültige **Belegsatz/Beiblatt-Konzept**, das die Prüfungsvorbereitung konkret verändert.

**Vorgeschlagene Ergänzung:**

> Seit 2025 liegt der schriftlichen AP2-Prüfung ein **standardisiertes SQL-Beiblatt** bei, das die relevante Syntax (Tabellenstruktur, Index, Manipulation, Projektion, Selektion, Sortieren, Gruppieren) als Referenz enthält. Das bedeutet nicht, dass Syntaxwissen unwichtig wird – gefragt ist vor allem die **Fähigkeit, die im Beiblatt referenzierte Syntax korrekt anzuwenden und zu kombinieren**, nicht jedes Detail auswendig zu kennen. Neu explizit im Katalog benannt: das **Erkennen von Anomalien und Redundanzen** in Datenbanken – ein Thema, das bereits in `SQL/4.SQL_Normalisierung_und_Constraints.md` behandelt wird.

---

## Zusammenfassung für schnelles Nacharbeiten

| Datei | Ein-Satz-Korrektur |
|---|---|
| `Programmablaufplan (PAP...).md` | PAP seit 2025 nicht mehr Prüfungsstoff – Hinweisbox ergänzen |
| `Nassi_Schneider_Diagramm.md` | Struktogramm seit 2025 nicht mehr Prüfungsstoff – Hinweisbox ergänzen |
| `SQLITE/3.SQLite_in_der_IHK_Prüfung.md` | 30%-Angabe aufschlüsseln + Belegsatz-Konzept ergänzen |

Alle drei Änderungen sind reine **Ergänzungen/Hinweisboxen**, keine Umschreibung der bestehenden Inhalte – die didaktische Substanz der Originaldateien bleibt vollständig erhalten.