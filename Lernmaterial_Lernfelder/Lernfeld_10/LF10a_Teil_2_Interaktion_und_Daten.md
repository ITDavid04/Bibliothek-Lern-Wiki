# LF10a Teil 2 – Interaktion & Daten

> **Zielgruppe:** Umschüler FIAE, 2. Lehrjahr (Aufbaustufe), Dualis-Institut Hamburg
> **Prüfungsrelevanz:** Schwerpunkt AP2 im Prüfungsbereich "Planen eines Softwareproduktes" (schriftlich, 90 Minuten, 10% Gewichtung) – insbesondere "Programmspezifikationen anwendungsgerecht festlegen" und "Maßnahmen zur Qualitätskontrolle planen und durchführen". Projektbezogene Inhalte (Interaktionslogik, Validierung, Datenanbindung) können außerdem im Fachgespräch relevant werden, sofern sie Teil der eigenen betrieblichen Projektarbeit sind.
> **Lernzeit:** Ca. 2,5–3 Stunden Lesen/Durcharbeiten, +1,5–2 Stunden für Zustandsmodell-/Validierungs-Übungen
> **Status:** Final
> **Stand:** 2026-09-14
>
> Die 🔴/🟡/🟢-Markierungen in diesem Artikel sind eine didaktische Einschätzung zur Prüfungsvorbereitung, keine offizielle IHK-Gewichtung.
>
> **Hinweis zur Abgrenzung:** Zweiter Teil der dreiteiligen LF10a-Serie (Teil 1: Konzeption & Struktur; Teil 2: Interaktion & Daten; Teil 3: Sicherheit, Test & Abnahme). Baut auf den Architekturmustern und UI-Zuständen aus Teil 1 auf – hier werden sie technisch mit JavaScript/DOM umgesetzt.

---

## IHK-Kernfragen dieses Artikels

| # | Frage | Abschnitt |
| --- | --- | --- |
| 1 | Wie hängen DOM, Ereignis und Anwendungszustand zusammen, und warum ist `submit` robuster als `click`? | [→ 1. Interaktion und UI-Zustände](#1-interaktion-und-ui-zustände) |
| 2 | Was unterscheidet syntaktische, semantische und feldübergreifende Validierungsregeln, und warum ist Clientvalidierung keine Sicherheitsgrenze? | [→ 2. Validierung und Fehlerrückmeldung](#2-validierung-und-fehlerrückmeldung) |
| 3 | Was gehört in einen Datenvertrag, und welche Zustände braucht eine asynchrone Datenanbindung mindestens? | [→ 3. Datenanbindung und API-Zustände](#3-datenanbindung-und-api-zustände) |

---

## 1. Interaktion und UI-Zustände

> **Grundprinzip:** Ein Fahrkartenautomat, bei dem der Bildschirm, die Kartenausgabe und die Geldrückgabe jeweils unabhängig voneinander "wissen", was gerade passiert, kann leicht widersprüchliche Zustände anzeigen – Karte ausgegeben, aber Bildschirm zeigt noch "bitte warten". Erst eine gemeinsame, zentrale Zustandsquelle verhindert das.

### 1.1 DOM, Ereignis und Zustand

Der **DOM (Document Object Model)** ist die Objektstruktur, über die JavaScript auf das HTML-Dokument zugreift. Ein **Ereignis** meldet eine Benutzer- oder Systemaktion (`click`, `submit`, `input`, `keydown`). Ein **UI-Zustand** beschreibt die aktuell fachlich relevante Situation der Oberfläche.

Eine Interaktion ist ein Zustandsübergang: Ausgangszustand + Ereignis (+ Bedingung) → neuer Zustand + sichtbare Rückmeldung. Beispiel: Im Zustand "Entwurf" führt das Absenden gültiger Daten zum Zustand "wird gespeichert" – die Schaltfläche wird deaktiviert, der Vorgang verständlich angekündigt.

> **Wichtig für die Prüfung:** Der sichtbare DOM ist **kein** Zustandsmodell. Wenn mehrere Funktionen Klassen, Texte und Attribute unabhängig verändern, kann die Oberfläche gleichzeitig widersprüchliche Situationen anzeigen. Ein expliziter fachlicher Zustand ist die gemeinsame Quelle, aus der die Darstellung abgeleitet wird – nicht umgekehrt.

### 1.2 Ereignisbehandlung strukturieren

Keine HTML-Inlinehandler (`onclick="…"`) – ein **Listener** wird im JavaScript registriert. Empfohlene Trennung in drei Schritte: (1) **DOM lesen** (Eingaben/Ziel ermitteln), (2) **Zustand ändern** (fachliche Entscheidung), (3) **darstellen** (sichtbare UI aktualisieren).

```js
form.addEventListener("submit", handleSubmit);

function handleSubmit(event) {
  event.preventDefault();
  if (!(event.target instanceof HTMLFormElement)) return;
  const data = new FormData(event.target);
  state.action = event.submitter?.name ?? "default"; // bei mehreren Submit-Buttons
  state.saving = true;
  render();
}
```

Bei mehreren Submit-Buttons mit unterschiedlicher Bedeutung liefert `event.submitter` den konkret verwendeten Button – die zentrale Verarbeitung bleibt dabei am Formular, ohne die Bedeutung des jeweiligen Submitters zu verlieren.

> **Wichtig für die Prüfung:** Ein Formular wird über `submit` verarbeitet statt über den `click` einer bestimmten Schaltfläche – aber nicht, weil `click` die Eingabetaste grundsätzlich verpasst: Beim impliziten Absenden löst der Browser laut HTML-Standard zuerst ein `click`-Event auf dem Standard-Submit-Button aus, bevor das `submit`-Event auf dem Formular feuert. Ein einzelner `click`-Listener auf dem Standard-Button erreicht die Eingabetaste in einfachen Formularen also durchaus. `submit` ist trotzdem die robustere Stelle, weil die Logik an die **Formularaktion** statt an einen einzelnen Submitter gebunden wird: Das deckt mehrere Submit-Buttons, `form.requestSubmit()` und spätere Strukturänderungen zuverlässiger ab als ein Listener auf genau einer Schaltfläche. Programmatisches `form.submit()` (ohne "request") löst weder `click` noch `submit` aus und umgeht die native Constraint-Validierung – dafür ist `form.requestSubmit()` die passendere Wahl.

### 1.3 Bibliothek, Modul und Framework

| Begriff | Kernidee |
| --- | --- |
| **Bibliothek** | Stellt wiederverwendbare Funktionen bereit – der Anwendungscode ruft sie gezielt auf |
| **Modul** | Kapselt zusammengehörigen Code und stellt definierte Schnittstellen/Exporte bereit |
| **Framework** | Gibt Anwendungsrahmen und zentrale Abläufe vor – ruft an festgelegten Stellen den Anwendungscode auf |

"Vorhanden" bedeutet nicht automatisch "geeignet". Bei jeder Abhängigkeit werden dokumentiert: Zweck im Projekt, Version/Quelle, Lizenz, Wartungs-/Abhängigkeitsfolgen, Einfluss auf Semantik/Barrierefreiheit, begründete Alternative.

> **Wichtig für die Prüfung:** Bibliothek und Framework sind **keine austauschbaren Begriffe** – bei einer Bibliothek ruft der Anwendungscode Funktionen auf, bei einem Framework ruft dieses den eigenen Code an vorgesehenen Stellen auf ("Inversion of Control"). Diese Abgrenzung ist in realen Paketen nicht immer trennscharf; entscheidend ist, welche Rolle die Abhängigkeit im eigenen Projekt tatsächlich übernimmt.

### 1.4 Rückmeldung, Live-Region und Fokusführung

Fokusregeln: Beim Öffnen Fokus an sinnvollen Anfang setzen; beim Schließen zum auslösenden Element zurückführen; bei mehreren Fehlern Orientierung zur Zusammenfassung ermöglichen; nach Erfolg nicht überraschend springen; der Fokusindikator muss sichtbar bleiben, und wird ein fokussiertes Element entfernt oder deaktiviert, muss der Fokus nachvollziehbar auf ein sinnvolles Ersatz- oder Folgeelement gesetzt werden.

> **IHK-Typfrage:** *"Ein Team registriert für einen Speichern-Button einen `click`-Listener statt eines `submit`-Listeners auf dem umgebenden Formular. Ist das fachlich robust, und wie sollte die Implementierung geändert werden?"*
> **Musterantwort:** Ein `click`-Listener auf dem Standard-Submit-Button erreicht die Eingabetaste in einfachen Formularen durchaus, da der Browser beim impliziten Absenden zuerst ein `click`-Event auf diesem Button auslöst. Die Bindung ist aber an genau diese eine Schaltfläche geknüpft statt an die Formularaktion selbst – bei mehreren Submit-Buttons mit unterschiedlicher Bedeutung, bei programmatischem `form.requestSubmit()` oder bei späteren Strukturänderungen des Formulars ist das nicht zuverlässig genug. Die Implementierung sollte deshalb einen `submit`-Listener auf dem `<form>`-Element registrieren und `event.preventDefault()` aufrufen, um das native Neuladen zu verhindern; dadurch werden native Validierung, Tastaturbedienung und unterschiedliche Submitter einheitlich behandelt.

---

## 2. Validierung und Fehlerrückmeldung

> **Grundprinzip:** Ein Türsteher, der Ausweise nur oberflächlich anschaut (Clientvalidierung), verhindert vielleicht die meisten Probleme – aber wer wirklich verlässlich draußen bleiben soll, braucht eine Kontrolle, die sich nicht umgehen lässt (Servervalidierung). Beide Ebenen haben unterschiedliche Aufgaben.

### 2.1 Regelarten und Durchsetzungsort

Drei Regelarten beschreiben, **was** geprüft wird – der Durchsetzungsort beschreibt separat, **wo** verbindlich geprüft werden muss:

| Regelart | Bedeutung |
| --- | --- |
| **Syntaktisch** | Form/Wertebereich, z. B. Pflichtfeld, maximale Länge |
| **Semantisch** | Fachliche Bedeutung, z. B. Enddatum nicht vor Startdatum |
| **Feldübergreifend** | Beziehung mehrerer Werte (oft zugleich eine semantische Regel) |

Jede Regel wird vor der Implementierung dokumentiert: betroffenes Feld, fachlicher Zweck, gültige/ungültige Beispiele, erwartete Meldung, Zuständigkeit Client/Server.

> **Wichtig für die Prüfung:** "Serverseitig" ist **keine vierte, gleichrangige Regelart**, sondern der **Durchsetzungsort**: Clientseitig für unmittelbares Feedback, serverseitig als verbindliche Instanz – dieselbe syntaktische, semantische oder feldübergreifende Regel kann auf beiden Seiten geprüft werden müssen. Clientvalidierung verbessert die Bedienung, ist aber **keine Sicherheitsgrenze** – Code im Browser kann verändert oder umgangen werden, Eingaben können am Browser vorbei direkt an ein Backend gesendet werden. Jede sicherheits- oder geschäftsrelevante Regel muss zusätzlich serverseitig durchgesetzt werden (vertieft in LF10a Teil 3).

### 2.2 Native HTML-Validierung zuerst

HTML bietet `required`, passende Eingabetypen, `min`/`max`/`minlength`/`maxlength`/`pattern`. Diese Funktionen sind in Browser und Tastatur integriert und sollten **nicht pauschal mit `novalidate` abgeschaltet werden**. Native Regeln reichen aber nicht für jede Fachlogik – ergänzende Logik sollte eine konkrete Lücke schließen (z. B. feldübergreifende Regeln, die HTML nicht abbilden kann).

### 2.3 Fachliche Regeln und Race Conditions

```js
function validatePeriod(start, end) {
  if (!start || !end) return null;
  if (end < start) return "Das Enddatum darf nicht vor dem Startdatum liegen.";
  return null;
}
```

Eine **Race Condition** entsteht, wenn das Ergebnis von der zeitlichen Reihenfolge nebenläufiger Vorgänge abhängt – bei einer asynchronen Feldprüfung kann eine ältere Antwort eine neuere Eingabe überschreiben. Gegenmaßnahmen: Anfragen abbrechen, Sequenz-ID prüfen, nur das Ergebnis zur aktuellen Eingabe übernehmen.

### 2.4 Gute Fehlermeldungen und Testwerte

Eine hilfreiche Fehlermeldung nennt: (1) das Problem, (2) die erwartete Korrektur, (3) bei Bedarf die Auswirkung. "Ungültig" reicht nicht – besser: "Gib eine Personenzahl zwischen 1 und 60 ein." Gültige Werte bleiben nach einem Fehler **erhalten**, Fehler werden nicht nur farblich markiert.

Für einen Wertebereich werden häufig mindestens fünf repräsentative Testwerte verwendet: knapp unter der Grenze, genau auf der unteren Grenze, normaler gültiger Wert, genau auf der oberen Grenze, knapp über der Grenze. **Negativtests** prüfen erwartete Ablehnung, **Grenztests** die Übergänge gültig/ungültig, **Korrekturtests** ob nach einem Fehler ohne Datenverlust fortgesetzt werden kann. Beim **modellbasierten Testen** werden Testfälle systematisch aus einem vereinfachten Modell (z. B. der Regelmatrix) abgeleitet – das Modell ersetzt die Testdurchführung nicht.

> **IHK-Typfrage:** *"Ein Formular zeigt bei ungültiger Eingabe nur die Meldung 'Fehler'. Bewerten Sie das und schlagen Sie eine bessere Meldung für ein Feld 'Personenzahl (1–60)' vor."*
> **Musterantwort:** "Fehler" benennt weder das konkrete Problem noch die erwartete Korrektur und ist deshalb nicht hilfreich – die Person weiß nicht, was sie ändern soll. Besser: "Gib eine Personenzahl zwischen 1 und 60 ein." Diese Meldung nennt das Problem (Wert außerhalb des gültigen Bereichs) und die erwartete Korrektur (Bereich 1–60) in einem Satz. Die Meldung sollte technisch mit dem Feld verbunden sein (z. B. über `aria-describedby`), und der bereits eingegebene, fehlerhafte Wert sollte erhalten bleiben statt das Feld zu leeren.

---

## 3. Datenanbindung und API-Zustände

> **Grundprinzip:** Ein Kellner, der Bestellungen direkt an zufällige Küchenmitarbeiter weiterreicht statt über eine feste Ausgabestelle, macht die Küche unnötig kompliziert auszutauschen. Eine feste Ausgabestelle (Zugriffsschicht) lässt die Küche im Hintergrund wechseln, ohne dass sich am Bestellvorgang etwas ändert.

### 3.1 Datenvertrag

Ein **Datenvertrag** beschreibt die zwischen UI und Datenquelle erwartete Struktur und Bedeutung – mehr als ein Beispiel-JSON. Für jedes Feld mindestens: Name/fachliche Bedeutung, Datentyp/Format, Pflicht/optional; Beispielwert und Verwendung in der UI erleichtern zusätzlich Umsetzung und Prüfung, sind aber eher UI-Spezifikation als Kernbestandteil des Vertrags selbst. Für robuste Datenanbindung gehören außerdem erwartete Leerwerte, Fehlerantworten und die Reaktion auf Vertragsabweichungen zum Datenvertrag. Ein Datenvertrag ist **keine Datenbankbeschreibung** – er beschreibt die vereinbarte Schnittstelle zwischen den beteiligten Komponenten und enthält nur, was für den vereinbarten Zweck benötigt wird.

### 3.2 Fixture, Mock-Service und Test-API

| Variante | Stärke | Grenze |
| --- | --- | --- |
| **Fixture** | Einfach, offline, reproduzierbar | Simuliert Transport/Schreibvorgänge nur begrenzt |
| **Mock-Service** | Realistische Zustände steuerbar | Zusätzlicher Aufbau und Pflege |
| **Test-API** | Echter Transport und Vertrag | Verfügbarkeit, Umgebung, sichere Testdaten nötig |

Produktive Endpunkte und Produktionszugangsdaten gehören nicht in Test-Fixtures oder unkontrollierte Lernumgebungen. Externe/fremde Systeme dürfen nur verwendet werden, wenn ihre Nutzung ausdrücklich vorgesehen und autorisiert ist und geeignete, dafür bestimmte Testdaten verwendet werden.

### 3.3 Zugriffsschicht (Repository) und Bezug zu Teil 1

```js
export async function listBookings() { /* Transportdetail */ }
export async function createBooking(input) { /* Transportdetail */ }
```

Eine solche Zugriffsschicht wird häufig **Repository** genannt: Sie bietet der UI fachlich benannte Operationen und verbirgt, ob Daten aus Fixture, Mock-Service oder Test-API stammen. Die Muster aus Teil 1 lassen sich direkt kombinieren: In einer Drei-Schichten-Architektur gehört die UI zur Präsentationsschicht, fachliche Verarbeitung zur Logikschicht, dauerhaftes Speichern zur Datenhaltungsschicht; Client-Server beschreibt die Verteilung laufender Rollen über eine Schnittstelle; MVC kann zusätzlich Verantwortungen innerhalb der Anwendung ordnen. **Keines dieser Muster ersetzt den Datenvertrag oder eine klare UI-/Backend-Grenze.**

### 3.4 Asynchrone Zustände und die `fetch()`-Falle

```js
async function loadItems() {
  renderState("loading");
  try {
    const response = await fetch("./fixtures/items.json");
    if (!response.ok) {
      renderState("technical-error", { status: response.status });
      return;
    }
    let items;
    try {
      items = await response.json();
    } catch {
      renderState("contract-error"); // Antwort ist kein gültiges JSON
      return;
    }
    if (!Array.isArray(items)) {
      renderState("contract-error"); // Antwort ist JSON, aber kein Array
      return;
    }
    renderState(items.length ? "data" : "empty", items);
  } catch (error) {
    renderState("technical-error"); // z. B. Netzwerk-/Transportfehler oder bewusster Abbruch (siehe AbortController, Abschnitt 3.5)
  }
}
```

Der äußere `catch`-Block fängt technische Probleme wie Netzwerkfehler ab. HTTP-Fehlerstatus und Vertragsverletzungen (ungültiges JSON, falsche Struktur) werden davon getrennt und jeweils dem passenden Zustand zugeordnet – ein vereinfachtes Lernbeispiel könnte mehrere technische Unterfälle zunächst zusammenfassen, sollte das aber transparent machen statt sie unbemerkt zu vermischen.

**Bei einer Datenabfrage** (wie im Beispiel oben) benötigt die UI mindestens: **Laden**, **Daten vorhanden**, **keine Daten vorhanden**, **Vertragsverletzung** (`contract-error`), **technischer/unbekannter Fehler** – je nach Anwendung kommt ein **erwartbarer fachlicher Fehler** hinzu. **Bei einer Datenübermittlung** (Schreibvorgang) werden typischerweise eigene Zustände wie `saving` (verhindert Mehrfachübermittlung), `success` und ein fachlicher `business-error` benötigt – das sind zwei unterschiedliche, wenn auch verwandte Zustandsmaschinen. Jeder Zustand nennt den nächsten sinnvollen Schritt.

> **Wichtig für die Prüfung:** `fetch()` erfüllt sein Promise **auch bei vielen HTTP-Fehlerstatus** – `response.ok` muss deshalb separat geprüft werden, sonst kann eine Fehlerantwort wie ein Erfolg weiterverarbeitet werden. Zusätzlich muss geprüft werden, ob der Antwortkörper überhaupt dem Datenvertrag entspricht (im Beispiel: ist `items` ein Array?) – eine Vertragsverletzung bekommt einen eigenen Zustand (`contract-error`) und darf **nicht** als leerer Datenbestand erscheinen. "Keine Daten" (erfolgreiche Anfrage, leeres Ergebnis) und "Fehler" (Ergebnis unbekannt) sind unterschiedliche Situationen mit unterschiedlichen nächsten Schritten.

### 3.5 Race Conditions, optimistische UI und Verantwortungsgrenzen

Bei schnell wechselnden Filtern könnte eine ältere, verspätete Antwort das neuere Ergebnis überschreiben – Gegenmaßnahmen: `AbortController`, Anfrage-ID, Vergleich mit dem aktuellen Filter. Eine **optimistische UI** zeigt Erfolg bereits vor endgültiger Bestätigung durch die Datenquelle – wirkt schnell, braucht aber eine Rücknahme-/Fehlerstrategie; für kritische Aktionen ist eine pessimistische Bestätigung oft sicherer. Optimistische UI ist eine Vertiefung, keine Pflichtannahme.

Verantwortungsgrenzen beim Testen: **UI** (verständlicher Zustand, Eingabeschutz, Wiederholung), **Backend** (verbindliche Fachregel, Persistenz, Autorisierung), **Betrieb** (Erreichbarkeit, Monitoring, Protokollierung), **Fachprozess** (Entscheidung, Eskalation).

> **IHK-Typfrage:** *"Eine Liste zeigt bei einer leeren Kategorie denselben Bildschirm wie bei einem Netzwerkfehler. Bewerten Sie das."*
> **Musterantwort:** Das ist problematisch, weil beide Situationen unterschiedliche Ursachen und unterschiedliche sinnvolle nächste Schritte haben. Bei einer leeren Kategorie war die Anfrage erfolgreich – es gibt schlicht keine Einträge, ein sinnvoller nächster Schritt wäre z. B. "Kategorie wechseln" oder "Filter zurücksetzen". Bei einem Netzwerkfehler ist unbekannt, ob überhaupt Daten existieren – der sinnvolle nächste Schritt ist "Erneut versuchen". Werden beide Fälle gleich dargestellt, kann eine Person fälschlich glauben, es gebe tatsächlich keine Daten, obwohl nur die Anfrage gescheitert ist (oder umgekehrt unnötig einen Fehler vermuten). Die beiden Zustände "keine Daten vorhanden" und "technischer Fehler" müssen deshalb unterscheidbar dargestellt werden.

---

## 4. Zusammenspiel der Konzepte in LF10a Teil 2

Die drei Module bilden zusammen die technische Umsetzung der in Teil 1 konzipierten Struktur: **Interaktion und UI-Zustände** (Abschnitt 1) liefern das Zustandsmodell, mit dem Ereignisse nachvollziehbar verarbeitet werden. **Validierung** (Abschnitt 2) stellt sicher, dass nur sinnvolle Daten in dieses Zustandsmodell gelangen – mit der wichtigen Einschränkung, dass Clientvalidierung allein nicht genügt. **Datenanbindung** (Abschnitt 3) verbindet das Zustandsmodell mit einer austauschbaren, gekapselten Datenquelle und definiert, welche zusätzlichen Zustände (Laden, Fehler, Vertragsverletzung) dabei entstehen. Die Architekturmuster aus Teil 1 (Drei-Schichten, Client-Server, MVC) ziehen sich als Ordnungsrahmen durch alle drei Module – das Repository-Pattern aus Abschnitt 3.3 **ergänzt** diese Muster um eine gekapselte Zugriffsschicht für Daten, statt einfach eine "Anwendung" von ihnen zu sein: Drei-Schichten beschreibt strukturelle Aufteilung, Client-Server die Verteilung, MVC die Verantwortung innerhalb der Anwendung, Repository die Abstraktion des Datenzugriffs – sie spielen zusammen, ohne in einer festen Hierarchie zueinander zu stehen. Die durchgängige Warnung "Ausgeblendet/Client-geprüft ist nicht geschützt/sicher" aus Teil 1 setzt sich hier fort und wird in Teil 3 (Rollen, Datenschutz, sichere UI) vertieft.

---

## 5. Typische Prüfungsfallen

| # | Falle | Richtigstellung |
| --- | --- | --- |
| 1 | Der sichtbare DOM ist das Zustandsmodell | Ein expliziter fachlicher Zustand ist die gemeinsame Quelle, aus der die Darstellung abgeleitet wird – nicht umgekehrt |
| 2 | Ein `click`-Listener auf die Speichern-Schaltfläche deckt das Formular vollständig ab | Erreicht die Eingabetaste beim Standard-Button meist durchaus, bindet die Logik aber an genau diesen Submitter statt an die Formularaktion – bei mehreren Buttons oder `requestSubmit()` unzuverlässig |
| 3 | Bibliothek und Framework sind austauschbare Begriffe | Bibliothek: Anwendungscode ruft auf; Framework: ruft eigenen Code an vorgesehenen Stellen auf |
| 4 | Clientvalidierung ist eine vertrauenswürdige Sicherheits- oder Autorisierungsgrenze | Sie verbessert nur die Bedienung – Code im Browser kann umgangen werden, sicherheitsrelevante Regeln müssen serverseitig gelten |
| 5 | Native HTML-Validierung sollte durch eigene Logik ersetzt werden | Native Funktionen sind in Browser/Tastatur integriert und sollten nicht pauschal per `novalidate` abgeschaltet werden |
| 6 | Ein normaler gültiger Testwert beweist die Regel | Viele Fehler liegen genau am Übergang zwischen gültig und ungültig – Grenztests sind nötig |
| 7 | `fetch()` wirft bei jedem HTTP-Fehler eine Exception | Viele Fehlerstatus erfüllen das Promise regulär – `response.ok` muss separat geprüft werden |
| 8 | Eine leere Liste und ein Fehler sind derselbe Bildschirm | Bei leerer Liste war die Anfrage erfolgreich, bei einem Fehler ist das Ergebnis unbekannt – beide brauchen unterschiedliche nächste Schritte |
| 9 | Ein Beispiel-JSON ist bereits ein Datenvertrag | Ohne Bedeutung, Format, Pflichtstatus und Fehlerfälle bleibt offen, wie die UI auf Abweichungen reagieren soll |

---

## 6. Vertiefung und Ausblick (freiwillig – für den ersten Durchgang nicht erforderlich)

- **Optimistische UI im Detail**: Konkrete Rücknahmestrategien (Rollback, Re-Sync) bei fehlgeschlagener Bestätigung durch die Datenquelle.
- **`AbortController` im Detail**: Wie laufende `fetch()`-Anfragen gezielt abgebrochen werden, um Race Conditions zu vermeiden.
- **Modellbasiertes Testen im Detail**: Wie aus einer Regelmatrix systematisch eine vollständige, überschneidungsfreie Testfallmenge abgeleitet wird.
- **State-Management-Bibliotheken**: Wie umfangreichere Anwendungen das in Abschnitt 1 beschriebene Zustandsprinzip mit dedizierten Werkzeugen strukturieren.
- **Web Components und Zugriffsschichten**: Wie sich das Repository-Pattern aus Abschnitt 3.3 mit gekapselten UI-Komponenten kombinieren lässt.

---

## 7. Selbsttest

| # | Frage | Kurzantwort |
| --- | --- | --- |
| 1 | Was ist ein UI-Zustand, und warum reicht der sichtbare DOM als Modell nicht aus? | Die fachlich relevante Situation der Oberfläche; der DOM kann bei unabhängigen Änderungen widersprüchliche Situationen anzeigen |
| 2 | Warum ist `submit` robuster als ein `click`-Listener auf eine Schaltfläche, obwohl `click` die Eingabetaste beim Standard-Button meist auch erreicht? | Bindet an die Formularaktion statt an einen einzelnen Submitter – deckt mehrere Submit-Buttons, `requestSubmit()` und Strukturänderungen zuverlässiger ab |
| 3 | Was unterscheidet Bibliothek und Framework? | Bibliothek: Anwendungscode ruft auf; Framework: ruft eigenen Code an vorgesehenen Stellen auf |
| 4 | Nenne drei Regelarten bei der Validierung, und was ist der Durchsetzungsort? | Syntaktisch, semantisch, feldübergreifend – Durchsetzungsort (client- oder serverseitig) ist eine separate Dimension, keine vierte Regelart |
| 5 | Warum ist Clientvalidierung keine Sicherheitsgrenze? | Code im Browser kann verändert/umgangen werden, Eingaben können direkt ans Backend gesendet werden |
| 6 | Nenne fünf typische Testwerte für einen Wertebereich. | Knapp unter Grenze, auf unterer Grenze, normaler gültiger Wert, auf oberer Grenze, knapp über Grenze |
| 7 | Was gehört mindestens in einen Datenvertrag, und was ist eher optionale Ergänzung? | Mindestens: Name/Bedeutung, Datentyp/Format, Pflicht/optional; Beispielwert und UI-Verwendung erleichtern die Umsetzung, sind aber eher UI-Spezifikation als Kernbestandteil |
| 8 | Warum reicht `response.ok` allein nicht, um Datenkorrektheit sicherzustellen? | Es prüft nur den HTTP-Status, nicht ob der Antwortkörper dem Datenvertrag entspricht (dafür: eigener `contract-error`-Zustand) |
| 9 | Was unterscheidet Race Condition bei Validierung und bei Datenanbindung im Kern? | Beide entstehen, wenn das Ergebnis vom zeitlichen Ablauf nebenläufiger Vorgänge abhängt – bei Validierung kann eine veraltete Prüfantwort, bei Datenanbindung eine veraltete Ladeantwort den aktuellen Zustand überschreiben; Lösung jeweils über Abbruch, Sequenz-/Anfrage-ID oder Vergleich mit aktuellem Zustand |

---

## 8. IHK-Cheatsheet

| Begriff | Kurzdefinition |
| --- | --- |
| **DOM/Ereignis/Zustand** | Objektstruktur / Aktion-Meldung / fachlich relevante Situation |
| **Listener** | Im JavaScript registrierte Reaktion auf ein Ereignis (kein Inline-Handler) |
| **submit vs. click** | `submit` bindet an die Formularaktion (mehrere Submitter, `requestSubmit()`), `click` nur an eine Schaltfläche – Enter erreicht click beim Standard-Button meist trotzdem |
| **Bibliothek / Framework** | Wird aufgerufen / ruft eigenen Code auf (Inversion of Control) |
| **Clientvalidierung ≠ Sicherheit** | Nur Bedienkomfort – sicherheitsrelevante Regeln müssen serverseitig gelten |
| **Native HTML-Validierung** | `required`, `min`/`max`, `pattern` – nicht pauschal per `novalidate` abschalten |
| **Race Condition** | Ergebnis hängt von Reihenfolge nebenläufiger Vorgänge ab – Lösung: Abbruch/ID/Vergleich |
| **Datenvertrag** | Bedeutung, Typ, Pflichtstatus, Fehlerfälle – mehr als Beispiel-JSON |
| **Fixture / Mock-Service / Test-API** | Statisch offline / steuerbar simuliert / echter Transport in Testumgebung |
| **Repository-Pattern** | Zugriffsschicht mit fachlich benannten Operationen, kapselt Datenquelle |
| **response.ok** | Prüft nur HTTP-Status, nicht Datenvertrag-Konformität |
| **contract-error** | Eigener Zustand für Vertragsverletzung, nicht gleich "keine Daten" |
| **Optimistische UI** | Erfolg vor endgültiger Bestätigung angezeigt – braucht Rücknahmestrategie |

---

## 9. Prüfungstaktik

| Aufgabentyp | Typische Formulierung | Was die Prüfung hören will |
| --- | --- | --- |
| Ereignisbehandlung bewerten | "Bewerten Sie diese Listener-Registrierung" | Konkrete scheiternde Bedienwege benennen, nicht nur "das ist schlecht" |
| Validierungsregel einordnen | "Welche Art von Regel ist X?" | Syntaktisch/semantisch/feldübergreifend korrekt zuordnen, Durchsetzungsort (Client/Server) getrennt davon benennen |
| Fehlermeldung verbessern | "Verbessern Sie diese Meldung" | Problem + erwartete Korrektur konkret benennen, keine vage Formulierung |
| API-Zustand einordnen | "Welcher Zustand liegt hier vor?" | Zwischen leer/Fehler/Vertragsverletzung sauber unterscheiden, Begründung liefern |
| Architekturbezug herstellen | "Wo gehört diese Komponente hin?" | Bezug zu Drei-Schichten/Client-Server/MVC aus Teil 1 explizit herstellen |

---

## 10. Merksätze für das Fachgespräch

> Ein Ereignis ist erst dann sauber verarbeitet, wenn Zustand, sichtbare Rückmeldung und nächster Bedienpunkt zusammenpassen.

> Eine gute Validierung erklärt nicht nur, dass etwas falsch ist, sondern wie die Eingabe fachlich korrigiert werden kann.

> Clientvalidierung ist Komfort, nicht Sicherheit – die verbindliche Regel gilt serverseitig.

> Eine Datenanbindung ist erst robust, wenn Vertrag, Zustände und Verantwortungsgrenzen gemeinsam entworfen sind.

> Leer und fehlerhaft sind zwei verschiedene Geschichten – beide brauchen ihren eigenen nächsten Schritt.

---

```yaml
dokument: LF10a-Teil2-wiki-artikel
lernfeld: "LF10a Teil 2 (Module 10a.5-10a.7)"
titel: "Interaktion & Daten"
typ: "Typ A – Kompakter Prüfungs-Wiki (FIAE-fokussiert)"
status: final
stand: 2026-09-14
quellen_intern:
  - "LF10a.5-7 Lernskripte (Interaktion/UI-Zustände, Validierung/Fehlerrückmeldung, Datenanbindung/API-Zustände)"
  - "Bewusster Rückbezug zu LF10a Teil 1 (Architekturmuster, UI-Zustände) statt Wiederholung - Repository-Pattern als ergänzende Zugriffsschicht, nicht als Anwendung der dort eingeführten Muster"
  - "Offizielle FIAE-AP2-Bezeichnung 'Planen eines Softwareproduktes' aus Teil-1-Korrektur übernommen (bereits web-verifiziert)"
quellen_fachlich:
  - titel: "WHATWG HTML Living Standard, WHATWG Fetch Standard"
    herausgeber: "WHATWG"
    status: "Rohmaterial verlinkt Primärquellen; fetch()/response.ok-Verhalten und AbortController sind stabile Web-Plattform-Standards, keine Web-Verifikation nötig"
  - titel: "Architekturmuster (Repository-Pattern, Drei-Schichten/Client-Server/MVC aus LF10a Teil 1), Validierungs-/Teststrategien"
    herausgeber: "Rohmaterial + etabliertes Software-Engineering-Standardwissen"
    status: "stabile Konzepte, keine Web-Verifikation nötig"
review_historie:
  - runde: 1
    datum: 2026-09-14
    ergebnis: "Erstdraft erstellt aus den drei LF10a.5-7-Lernskripten. Als ein zusammenhängender Artikel (Teil 2 von 3) verfasst. Bewusste Rückbezüge zu Teil 1 (Architekturmuster, UI-Zustände, offizielle AP2-Bezeichnung) statt Wiederholung. Von Anfang an vorsichtig und kontextbezogen formuliert, basierend auf den Lektionen aus Teil 1 und den LF7/LF9-Review-Historien (keine Absolutheiten ohne Einschränkung, konsistente Cheatsheet-/Selbsttest-Formulierungen von Beginn an)."
  - runde: 2
    datum: 2026-09-14
    ergebnis: "3 Reviews eingearbeitet. Wichtigster Fund (2 Reviews, eigenständig web-verifiziert via HTML-Spec): 'click scheitert bei Enter'-Aussage war zu absolut - beim impliziten Absenden löst der Browser laut HTML-Standard zuerst ein click-Event auf dem Standard-Submit-Button aus, bevor submit feuert. submit bleibt trotzdem robuster, aber aus anderen Gründen (mehrere Submitter, requestSubmit(), Strukturänderungen) - an vier Stellen korrigiert (Wichtig-Block, Typfrage, Prüfungsfalle, Selbsttest, Cheatsheet). Zweitwichtigster Fund (2 Reviews, gut begründet): 'vier Regelarten' vermischte Regelart mit Durchsetzungsort - 'serverseitig' ist kein Regeltyp auf gleicher Ebene wie syntaktisch/semantisch/feldübergreifend, sondern eine separate Dimension - Abschnitt 2.1 umstrukturiert. Dritter Fund (1 Review, klare interne Inkonsistenz): API-Zustände als 'sechs' benannt, aber contract-error im Code macht sieben - korrigiert, Codebeispiel zusätzlich verbessert (HTTP-Fehler/JSON-Parse-Fehler/Array-Vertragsverletzung sauber getrennt statt im catch-Block vermischt). Weitere Korrekturen: Repository-Pattern nicht mehr als 'Anwendung' der Architekturmuster dargestellt, sondern als Ergänzung ohne Hierarchie; AP1-Erwähnung aus Kopf entfernt (Artikelinhalt ist AP2-spezifisch); Fixture/Mock/Test-API-Absolutheit ('fremde Systeme grundsätzlich ausgeschlossen') abgeschwächt; Moduldefinition präzisiert; Selbsttest 9 (Race Condition) allgemeiner und korrekter gefasst. Zum wiederholten Mal behauptete eine Review Claude-Chat-Links im Dokument - beim Abgleich nicht bestätigt."
  - runde: 3
    datum: 2026-09-14
    ergebnis: "3 Reviews eingearbeitet. Zwei interne Inkonsistenzen aus Runde 2 nachgezogen (Prüfungstaktik nannte noch 'serverseitig' als Regelart, YAML-Quellenblock noch 'konkrete Anwendung' statt 'ergänzende Zugriffsschicht' fürs Repository-Pattern). Fetch-Beispiel: irreführender 'Timeout'-Kommentar korrigiert (fetch() hat keinen eingebauten Timeout-Mechanismus) und um event.submitter ergänzt (macht die 'mehrere Submitter'-Aussage im Code sichtbar). API-Zustände konzeptionell in Lese- und Schreib-Zustandsmaschine getrennt statt einer vermischten Liste (u. a. weil 'erfolgreiche Übermittlung' im gezeigten loadItems()-Beispiel gar nicht vorkommt). Fokusregel präzisiert (sichtbarer Indikator vs. Fokus-Ersatz beim Entfernen als zwei getrennte Aspekte). 'Fünf Testklassen' zu 'fünf Testwerten' präzisiert (keine feste allgemeine Kategorie). Datenvertrag-Definition erweitert (Fehler-/Leerwertverhalten ergänzt, UI-Sichtbarkeit als optional statt Kernbestandteil gekennzeichnet). Zum wiederholten Mal (jetzt mehrfach über mehrere Runden) behauptete eine Review sehr konkret Claude-Chat-Links und einen abgeschnittenen YAML-Block - beide Behauptungen beim Abgleich erneut nicht bestätigt."
  - runde: 4
    datum: 2026-09-14
    ergebnis: "Eigene Abschlussprüfung: Abschnittsüberschrift 2.4 trug noch 'Testklassen' statt der in Runde 3 korrigierten 'Testwerte' - nachgezogen. Selbsttest 7 listete 'Beispielwert, Verwendung in der UI' noch als Kernbestandteil des Datenvertrags, obwohl 3.1 das gerade als optionale Ergänzung statt Kernbestandteil präzisiert hatte - angeglichen. Keine weiteren Fachfehler gefunden."
freigabe: "Final gesetzt nach 4 Runden (3 externe Prüfrunden + 1 eigene Abschlussprüfung, 2026-09-14) – Freigabe durch Autor:in bestätigt"
```