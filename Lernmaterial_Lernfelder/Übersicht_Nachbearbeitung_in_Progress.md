# Wiki-Gesamtaudit: Offene Fragen & Ergänzungsvorschläge (LF1–WiSo)

> **Zweck:** Einmaliger Durchgang durch die komplette Sammlung (`Lernfeld_1` bis `WISO`, inkl. `Mathe_Was_Notwendig_ist`, `SQL`, `SQLITE`), um Unklarheiten, offene Punkte und mögliche Ergänzungen zu identifizieren – kein neuer Lerninhalt, reine Bestandsaufnahme.
>
> **Methodik:** Systematischer Abgleich aller Status-Felder (Kopf vs. YAML) auf Konsistenz, Suche nach Selbst-Markierungen für offene Punkte (TODO/Draft/Review), Stichprobenlese ausgewählter Inhalte, Prüfung interner Links. Kein vollständiger Vier-Runden-Review jeder einzelnen Datei – dafür wäre der Umfang zu groß. Bereiche, die in früheren Sessions bereits mehrfach extern geprüft wurden (LF7, LF9, LF10a, Mathe-Serie, SQL/SQLITE), sind hier nur kurz zusammengefasst, nicht neu geprüft.
>
> **Stand:** 2026-09-16

---

## Gesamteinschätzung

Die Sammlung ist **inhaltlich auffällig gut gepflegt** – fast jede Datei trägt eine ausführliche `review_historie` mit mehreren dokumentierten externen Prüfrunden, oft mit Quellenbelegen. Die hier gefundenen offenen Punkte sind größtenteils **organisatorisch/strukturell** (Status-Widersprüche, kaputte Querverweise, Namenskollisionen), nicht tiefe fachliche Fehler. Eine Ausnahme: LF2.3 ist selbst als noch nicht abschließend geprüft markiert.

| Kategorie | Anzahl Funde |
|---|---|
| 🔴 Status-Widerspruch (Kopf sagt Final, YAML sagt Draft) | 2 |
| 🔴 Kaputte interne Links | 1 (betrifft 2 Dateien) |
| 🟡 Selbst markierter offener/unsicherer Inhalt | 1 |
| 🟡 Veraltete Orientierungs-/Tracking-Datei | 1 |
| 🟡 Zugesagte, aber nicht umgesetzte Korrektur | 2 Dateien |
| 🟡 Namenskollision | 1 |
| 🟢 Kleinere Beobachtungen | 3 |

---

## 1. Status-Widersprüche (Kopf vs. YAML)

Zwei Dateien haben im sichtbaren Kopf "Status: Final", aber im maschinenlesbaren YAML-Block noch "status: draft" stehen. Wer nur das YAML automatisiert ausliest, sieht einen anderen Stand als beim Lesen des Artikels.

| Datei | Kopf | YAML |
|---|---|---|
| `Lernfeld_3/LF3.2_OSI-Modell_Transportprotokolle_Dienste.md` | Final | draft 🟢 korregiert
| `Lernfeld_4/LF4.3A_Cloud_Security_Zero_Trust.md` | Final | draft | 🟢 korregiert

**Vorschlag:** In beiden Dateien das YAML-Feld `status: draft` auf `status: final` setzen – reine Ein-Zeilen-Korrektur, der Inhalt selbst ist laut Review-Historie bereits final freigegeben.

---

## 2. Kaputte interne Querverweise

Die beiden "Einstieg"-Dateien zu LF9 (FISI-Fokus) und LF10 (FIAE-Fokus) verlinken aufeinander – aber mit falschen Dateinamen **und** ohne Ordnerpfad, obwohl sie in unterschiedlichen Lernfeld-Ordnern liegen.

| Datei | Fehlerhafter Link | Tatsächliche Zieldatei |
|---|---|---|
| `Lernfeld_9/0.1_Zusatz_zu_LF9.md` | `[LF10](LF10a_Einstieg.md)` | `Lernfeld_10/0.1_Zusatz_zu_LF10.md` | 🟢 Link entfernt
| `Lernfeld_10/0.1_Zusatz_zu_LF10.md` | `[Einstieg zu LF9](LF9_Einstieg.md)` (2×) | `Lernfeld_9/0.1_Zusatz_zu_LF9.md` | 🟢 Link entfernt

**Vermutliche Ursache:** Die Dateien wurden ursprünglich als `LF9_Einstieg.md`/`LF10a_Einstieg.md` im selben Ordner konzipiert, dann aber beim Hochladen in die getrennten Lernfeld-Ordner verschoben und umbenannt – die Links wurden dabei nicht mitgezogen.

**Vorschlag:** Links auf `../Lernfeld_10/0.1_Zusatz_zu_LF10.md` bzw. `../Lernfeld_9/0.1_Zusatz_zu_LF9.md` korrigieren (oder je nachdem, wie GitHub-relative Links in eurer Darstellung aufgelöst werden).

---

## 3. Lernfeld 2 – Ein Artikel noch nicht final

`LF2.3_Infrastruktur_Energie.md` trägt selbst den Status **"Review"** (nicht Final) und im Kopf den expliziten Hinweis:

> *"Stand: 2026-08-27 – Zahlenangaben zu Kapazitäten, Preisen und Schnittstellen-Bandbreiten sollten vor Verwendung im Unterricht kurz gegengeprüft werden."*

Die Status-Zeile listet bereits mehrere in einer Vorrunde korrigierte Punkte (Bandbreitenformel, PFC, 80-PLUS-Spannungsbezug, Effizienzkurve, SAS-Versionen, SMR/RAID-Absolutheit, DMI-Pauschalisierung), ist aber noch nicht final freigegeben.

**Vorschlag:** Vor dem nächsten Einsatz im Unterricht die genannten Zahlenangaben (Kapazitäten, Preise, Schnittstellen-Bandbreiten) noch einmal gegenchecken und dann auf Final setzen – oder gezielt eine weitere Review-Runde dafür einplanen, wie bei den anderen Lernfeldern üblich.

Alle anderen sieben LF2-Artikel (2.1, 2.2, 2.4–2.8) sind konsistent Final.

---

## 4. Lernfeld 5 – Veraltete Orientierungsdatei

`Lernfeld_5/1.0_Info_Bitte_Lesen/Info_Text_Aufbau.md` (Stand 2026-09-11) ist eine reine Orientierungsdatei ohne Lerninhalt, die den LF5-Aufbau erklärt. Ihre Statustabelle ist aber **nicht mehr aktuell**:

> *"LF5.6 Teil B – Agile Methodologies (Scrum, Kanban, XP) | ⚪ Noch offen | Fehlt komplett, muss noch geschrieben werden"*

Tatsächlich existiert `LF5.6_Agile_Methodologien_Scrum_Kanban_&_XP.md` bereits – mit Status Final (Kopf und YAML übereinstimmend) und zwei dokumentierten externen Review-Runden.

**Vorschlag:** Die Statustabelle in `Info_Text_Aufbau.md` aktualisieren (LF5.6 Teil B auf ✅ Final setzen), damit die Orientierungsdatei nicht länger einen falschen Eindruck vermittelt, falls sie noch gelesen wird.

**Zusätzliche Beobachtung:** An diese Datei ist außerdem euer/mein früheres Infoblatt zu PAP/Struktogramm/SQLite-Belegsatz als Anhang angefügt (siehe Punkt 5) – strukturell etwas vermischt (Orientierungsdatei + Korrekturvorschlag in einer Datei), aber inhaltlich kein Fehler.

---

## 5. PAP & Struktogramm – zugesagte Korrektur nicht umgesetzt

Bereits vor einigen Schritten wurde festgestellt, dass `Lernfeld_5/Programmablaufplan (PAP - DIN 66001).md` und `Lernfeld_5/Nassi_Schneider_Diagramm.md` fälschlich aktuelle IHK-Prüfungsrelevanz behaupten, obwohl beide Notationsformen seit dem AP2-Prüfungskatalog 2025 nicht mehr Prüfungsstoff sind (ersetzt durch Pseudocode/Aktivitätsdiagramm, siehe `Mathe_Was_Notwendig_ist/Part_1`). Ein fertiger Korrekturtext (Hinweisbox) wurde dafür bereits geschrieben und als Anhang in `Lernfeld_5/1.0_Info_Bitte_Lesen/Info_Text_Aufbau.md` hinterlegt.

**Beim jetzigen Durchgang bestätigt:** Diese Hinweisbox wurde **in keiner der beiden Zieldateien tatsächlich eingefügt** – sie steht nur als Vorschlag in der Info-Datei, nicht in den Artikeln selbst.

**Vorschlag:** Die bereits fertig formulierte Hinweisbox aus `Info_Text_Aufbau.md` in beide Dateien direkt einfügen. (Die SQLite-Korrektur aus demselben ursprünglichen Vorschlag wurde bei einem späteren Durchgang bereits nachgeholt.)

---

## 6. Lernfeld 6 – Namenskollision bei "LF6.3"

Zwei unterschiedliche Dateien tragen beide "6.3" im Dateinamen:

- `Lernfeld_6_Service_Desk/LF6.3_ITIL_v4_Service_Management.md`
- `Lernfeld_6_Service_Desk/LF6.3_IT_Lifecycle_Bestandsverwaltung.md`

Beide sind inhaltlich fertig (Status Final) und eigenständig – nur die Nummerierung überschneidet sich, was beim Suchen/Zitieren zu Verwechslungen führen kann ("meinst du das ITIL-6.3 oder das Lifecycle-6.3?").

**Vorschlag:** Einen der beiden Artikel umnummerieren (z. B. IT-Lifecycle/Bestandsverwaltung auf 6.5, falls das in eurer LF6-Gliederung nicht bereits anderweitig belegt ist) – reine Umbenennung, kein Inhaltseingriff.

---

## 7. Kleinere Beobachtungen (kein Handlungsbedarf, nur Notiz)

| Beobachtung | Einordnung |
|---|---|
| Der Ordner `Lernfeld_8_Providing_Data_Across_Systems.md` trägt trotz `.md`-Endung tatsächlich einen **Ordnernamen**, kein Dateiname | Rein kosmetisch/verwirrend beim ersten Hinsehen, technisch unproblematisch |
| `LF7_Infomaterial_Zusatz.md` hat im Kopf kein sichtbares `> **Status:**`-Feld (nur im YAML `status: final`) | Inhaltlich vermutlich fertig, nur redaktionell inkonsistent zum sonst durchgängigen Kopf-Status-Muster |
| Rohdaten-Begleitdateien (`Chen_Notations.md`, `ERM_Modell.md`, `USE_Case_Diagramm.md`, `image.png`, `ChenÜbersicht.png` u. a. in LF5) folgen einem anderen, älteren Format ohne YAML/Status – laut `Info_Text_Aufbau.md` bewusst so belassen, da bereits vor diesem Projekt entstanden | Bewusste Altbestand-Entscheidung, keine Lücke |

---

## 8. Bereiche mit bereits umfangreicher, aktueller Prüfhistorie (nur zur Vollständigkeit erwähnt)

Diese Bereiche wurden in früheren Sessions bereits mehrfach extern geprüft und sind konsistent Final (Kopf = YAML) – hier kein neuer Prüfbedarf identifiziert:

- **LF1** (1.1–1.4), **LF3** (3.1, 3.3–3.5, nur 3.2 s. o.), **LF4** (4.1, 4.2A/B, 4.3B, nur 4.3A s. o.)
- **LF6** (alle 5 Artikel inhaltlich fertig, nur Namenskollision s. o.)
- **LF7** (7.1–7.3, jeweils 9+ Reviews über 3 Runden)
- **LF8** (8.1–8.6, durchgängig Final)
- **LF9, LF10a** (Teil 1–3, mehrfach extern geprüft in dieser Session)
- **WiSo** (1, 2A, 2B, 3A, 3B, 4, 5 – alle Final, teils mit sehr detaillierter Review-Historie, z. B. WisoLF2A mit Sozialversicherungs-Korrekturen)
- **Mathe_Was_Notwendig_ist** (Part 1–3), **SQL** (1–5), **SQLITE** (1–5 + Praxisdateien) – alle in dieser Session mehrfach geprüft

---

## Priorisierte Kurzfassung für schnelles Abarbeiten

| Priorität | Maßnahme | Aufwand |
|---|---|---|
| 🔴 1 | `status: draft` → `status: final` in LF3.2 und LF4.3A | 2× eine Zeile |
| 🔴 2 | Kaputte Links in den LF9/LF10-Einstiegsdateien korrigieren | 2 Dateien, 3 Links |
| 🟡 3 | PAP- und Struktogramm-Hinweisbox tatsächlich einfügen (Text liegt fertig vor) | 2 Dateien, copy-paste |
| 🟡 4 | LF2.3: Zahlenangaben gegenchecken, dann Final setzen | Eine gezielte Prüfrunde |
| 🟡 5 | LF5-Statustabelle aktualisieren (LF5.6 Teil B als erledigt markieren) | Eine Zeile |
| 🟢 6 | LF6.3-Namenskollision auflösen (Umbenennung) | Dateiumbenennung |

Alles Weitere ist entweder bereits in gutem Zustand oder rein kosmetisch ohne Handlungsdruck.