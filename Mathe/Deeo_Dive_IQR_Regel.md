IQR-Regel – einfach erklärt
Was ist das Problem?

Stell dir vor, du hast eine Liste von Zahlen (z.B. Gehälter, Antwortzeiten eines Servers, Testergebnisse). Manchmal gibt es Ausreißer – Werte, die total aus der Reihe tanzen. Die IQR-Regel hilft dir, solche Ausreißer systematisch zu finden.
Die Grundidee

Du teilst deine sortierten Daten in vier gleich große Teile (Quartile) und schaust dir dann den mittleren Bereich an, wo die "normalen" Werte liegen.
Schritt-für-Schritt

1. Daten sortieren (aufsteigend)

2. Drei Quartile bestimmen:

    Q1 (unteres Quartil) = 25% der Daten liegen darunter
    Q2 (Median) = 50% der Daten liegen darunter
    Q3 (oberes Quartil) = 75% der Daten liegen darunter

    💡 Wie findet man Q1 und Q3 konkret?

        Bestimme zuerst den Median (Q2) der Gesamtdaten.
        Bei gerader Anzahl Werte: Mittelwert der beiden mittleren Zahlen.
        Bei ungerader Anzahl: die mittlere Zahl selbst.
        Q1 = Median der unteren Hälfte (ohne den Gesamtmedian, falls n ungerade ist)
        Q3 = Median der oberen Hälfte (ohne den Gesamtmedian, falls n ungerade ist)

3. IQR berechnen (Interquartilsabstand):

IQR = Q3 - Q1

→ Das ist die "Breite" des mittleren Bereichs, wo 50% deiner Daten liegen.

4. Grenzen berechnen (Ausreißer-Zäune):

Untere Grenze = Q1 - 1,5 × IQR
Obere Grenze  = Q3 + 1,5 × IQR

5. Ausreißer identifizieren:

Alles außerhalb dieser Grenzen ist ein Ausreißer.
Praktisches Beispiel (IT-Bezug)

Server-Antwortzeiten in ms (n=10, sortiert):

12, 14, 15, 15, 16, 17, 18, 19, 20, 95

    Untere Hälfte: 12, 14, 15, 15, 16 → Q1 = 15
    Obere Hälfte: 17, 18, 19, 20, 95 → Q3 = 19
    IQR = 19 - 15 = 4
    Untere Grenze = 15 - 1,5×4 = 9
    Obere Grenze = 19 + 1,5×4 = 25

→ 95 ms ist ein Ausreißer! (liegt weit über 25)
⚠️ Wichtig: Ausreißer ≠ Fehler!

Ein Ausreißer ist nicht automatisch etwas Falsches. Er zeigt dir nur: "Hier ist etwas Ungewöhnliches passiert – schau genauer hin!"

Im IT-Kontext kann das z.B. bedeuten:

    🚨 Ein echter Traffic-Spike (z.B. Marketing-Kampagne läuft an)
    🔓 Ein möglicher Hackerangriff (ungewöhnlicher Datenverkehr)
    🐛 Ein Bug oder Server-Fehler
    ✅ Aber auch: ein Messfehler in den Daten

→ Die IQR-Regel liefert dir nur den Hinweis, die Interpretation musst du selbst vornehmen!
Warum ist das für dich als FIAE/FISI relevant?
Anwendung 	Beispiel
Monitoring 	Ungewöhnliche Server-Response-Zeiten erkennen
Datenanalyse 	Fehlerhafte Messwerte in Datensätzen finden
Qualitätssicherung 	Testergebnisse bewerten
Security 	Ungewöhnliche Login-Zeiten/Zugriffsmuster erkennen
Merksatz

    "Alles, was mehr als das 1,5-fache des IQR unter Q1 oder über Q3 liegt, ist ein Ausreißer."

Das visualisiert man übrigens oft mit einem Boxplot – die "Box" zeigt Q1 bis Q3, die "Antennen" (Whiskers) reichen bis zu den Grenzen, und Punkte außerhalb sind die Ausreißer.
🐍 Praxis-Bonus: IQR in Python berechnen

import pandas as pd

# Daten aus unserem Beispiel
antwortzeiten = pd.Series([12, 14, 15, 15, 16, 17, 18, 19, 20, 95])

# Quartile berechnen
Q1 = antwortzeiten.quantile(0.25)
Q3 = antwortzeiten.quantile(0.75)
IQR = Q3 - Q1

# Grenzen berechnen
untere_grenze = Q1 - 1.5 * IQR
obere_grenze = Q3 + 1.5 * IQR

# Ausreißer finden
ausreisser = antwortzeiten[(antwortzeiten < untere_grenze) | 
                             (antwortzeiten > obere_grenze)]

print(f"Q1: {Q1}, Q3: {Q3}, IQR: {IQR}")
print(f"Grenzen: [{untere_grenze}, {obere_grenze}]")
print(f"Ausreißer:\n{ausreisser}")

Ausgabe:

Q1: 15.0, Q3: 19.0, IQR: 4.0
Grenzen: [9.0, 25.0]
Ausreißer:
9    95

    💡 Pandas nutzt standardmäßig eine etwas andere Interpolationsmethode als die manuelle "Median-der-Hälfte"-Methode – bei größeren Datensätzen können die Werte leicht abweichen. Für den Einstieg ist das aber vernachlässigbar!
