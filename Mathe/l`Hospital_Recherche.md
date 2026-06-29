# L’Hospitalsche Regel (IT-Umschulung FIAE/FISI, 2. Lehrjahr)

## 1. Worum geht’s?

Die **L’Hospitalsche Regel** hilft dir, **Grenzwerte von Brüchen** zu berechnen, wenn beim Einsetzen etwas *Unbestimmtes* herauskommt, z.B.:

- \(0/0\)  
- \(\infty/\infty\)

Dann darfst du statt des ursprünglichen Bruchs **die Ableitungen** von Zähler und Nenner benutzen.

**Grundidee:**

\[
\lim_{x \to a} \frac{f(x)}{g(x)}
\quad\text{(mit Form } 0/0 \text{ oder } \infty/\infty\text{)}
\quad=\quad
\lim_{x \to a} \frac{f'(x)}{g'(x)},
\]
sofern der rechte Grenzwert existiert (endlich oder uneigentlich \(\pm\infty\)).
Andernfalls kann die Regel keine Aussage treffen.

---

## 2. Einfache Erklärung (ohne Fachchinesisch)

Du willst wissen:

> Wie verhält sich das Verhältnis zweier Größen, wenn die Eingabe gegen einen bestimmten Wert geht?

**IT-Analogie (FIAE/FISI):**

- \(f(x)\): Anzahl der Operationen eines Algorithmus bei Eingabegröße \(x\)  
- \(g(x)\): Eingabegröße \(x\) selbst

Du kennst aus der **O-Notation**, dass dich oft interessiert, wie schnell die Operationen im Vergleich zur Eingabe wachsen.

In der Algorithmik ist meist das asymptotische Verhalten \(x \to \infty\) mit der Form \(\infty/\infty\) relevant.  
Der Fall \(0/0\) kann auftreten, wenn die Funktionen bei \(x = 0\) beide den Wert null haben.

Statt auf die Werte von \(f\) und \(g\) zu schauen, betrachtest du ihre **Änderungsraten**:

- Wie schnell wächst \(f(x)\)? → Ableitung \(f'(x)\)  
- Wie schnell wächst \(g(x)\)? → Ableitung \(g'(x)\)

Die L’Hospital-Regel sagt:  
Wenn der ursprüngliche Bruch unbestimmt ist, darfst du den Grenzwert des Bruchs der **Änderungsraten** \(\frac{f'(x)}{g'(x)}\) verwenden.

---

## 3. Wann darf ich L’Hospital benutzen?

Du darfst L’Hospital verwenden, wenn:

1. Du betrachtest einen Grenzwert eines **Bruchs**:
   \[
   \lim_{x \to a} \frac{f(x)}{g(x)}
   \]

2. Beim Einsetzen von \(x = a\) entsteht:
   - \(0/0\) oder  
   - \(\infty/\infty\)

3. \(f\) und \(g\) sind in einer Umgebung von \(a\) **differenzierbar**, und **\(g'(x) \neq 0\)** in dieser Umgebung (außer eventuell im Punkt \(a\) selbst).

Dann gilt:

\[
\lim_{x \to a} \frac{f(x)}{g(x)} = \lim_{x \to a} \frac{f'(x)}{g'(x)}
\]

**Wichtige Präzisierung:**  

- Der Grenzwert von \(\frac{f'(x)}{g'(x)}\) muss **existieren** – entweder als endliche Zahl oder uneigentlich als \(+\infty\) oder \(-\infty\).  
- Existiert er nicht (und geht auch nicht gegen \(\pm\infty\)), kann L’Hospital **keine Aussage** über den ursprünglichen Grenzwert machen.  
- Der ursprüngliche Grenzwert **kann trotzdem existieren** (z.B. \(\lim_{x\to\infty} \frac{x + \sin x}{x} = 1\), hier liefert L’Hospital \(\frac{1 + \cos x}{1}\), dessen Grenzwert nicht existiert).

---

## 4. Schritt-für-Schritt-Rezept

1. **Grenzwert aufschreiben**

   \[
   \lim_{x \to a} \frac{f(x)}{g(x)}
   \]

2. **Probe: Einsetzen**

   - Setze \(x = a\) in Zähler und Nenner ein.  
   - Prüfe, ob \(0/0\) oder \(\infty/\infty\) entsteht.

3. **Ableiten**

   - Berechne \(f'(x)\).  
   - Berechne \(g'(x)\).

4. **Neuen Grenzwert berechnen**

   \[
   \lim_{x \to a} \frac{f'(x)}{g'(x)}
   \]

5. Falls der neue Grenzwert wieder eine unbestimmte Form liefert → **Regel erneut anwenden** (nach Prüfung, dass weiterhin \(g'(x) \neq 0\) in der Umgebung gilt) oder ggf. andere Methoden (Umformen, Kürzen) nutzen.

---

## 5. Praktische Aufgaben zum Selbsttesten

> **Hinweis:**  
> Erst alle Aufgaben **selbst rechnen**, dann unten im Lösungsabschnitt vergleichen.

### Aufgabe 1 (Basis, Trigonometrie)

Berechne:
\[
\lim_{x \to 0} \frac{\sin x}{x}
\]

---

### Aufgabe 2 (Polynom-Bruch, \(\infty/\infty\); zwei Wege)

Berechne:
\[
\lim_{x \to \infty} \frac{4x^2 - 1}{2x^2 + 3}
\]

Versuche **zwei Lösungswege**:

1. Mit L’Hospital.  
2. Ohne L’Hospital, indem du Zähler und Nenner durch \(x^2\) teilst.

---

### Aufgabe 3 (zweimal L’Hospital)

Berechne:
\[
\lim_{x \to 0} \frac{1 - \cos x}{x^2}
\]

---

### Aufgabe 4 (Exponentialfunktion)

Berechne:
\[
\lim_{x \to 0} \frac{e^x - 1}{x}
\]

---

### Aufgabe 5 (Logarithmus, \(\infty/\infty\))

Berechne:
\[
\lim_{x \to \infty} \frac{\ln x}{x}
\]

---

## 6. Wissenswerte Sidefacts (IHK-Relevanz: <span style="color:red">ROT</span> / <span style="color:gold">GELB</span> / <span style="color:green">GRÜN</span>)

> **Legende:**  

> **<span style="color:red">ROT</span>** = sehr wichtig für IHK (hochrelevant)  
> **<span style="color:gold">GELB</span>** = mittelwichtig (hilfreich, aber selten direkt abgefragt)  
> **<span style="color:green">GRÜN</span>** = nice to know (gut fürs Verständnis)

- **<span style="color:red">ROT</span>:** Sauberes Ableiten ist Pflicht  
  - Ohne richtige Ableitung bringt dir L’Hospital nichts.  
  - Wichtige Standard-Ableitungen:
    - \( (x^n)' = n x^{n-1} \)  
    - \( (e^x)' = e^x \)  
    - \( (\ln x)' = \frac{1}{x} \)  
    - \( (\sin x)' = \cos x \)  
    - \( (\cos x)' = -\sin x \)

- **<span style="color:red">ROT</span>:** Umgang mit Grenzwerten  
  - Grenzwerte sind Basis für Wachstumsverhalten von Funktionen, Laufzeitabschätzungen etc.  
  - Kommt indirekt in vielen Aufgaben vor (Funktionsverläufe, Extremwerte, Konvergenz).

- **<span style="color:gold">GELB</span>:** Erkennen unbestimmter Formen  
  - Du solltest sicher entscheiden können:
    - *Unbestimmt*: \(0/0\), \(\infty/\infty\)  
    - *Nicht unbestimmt*: z.B. \(2/0\) (Polstelle), \(\infty/5\) (Grenzwert \(=\infty\)).

- **<span style="color:gold">GELB</span>:** Mehrfaches Anwenden  
  - In komplexeren Aufgaben kann die Regel **mehrmals** benötigt werden.  
  - Gut, um bei Analysis-Aufgaben in der Prüfung zu punkten.

- **<span style="color:green">GRÜN</span>:** Verbindung zur O-Notation / Algorithmik  
  - Grenzwerte helfen zu verstehen, welche Funktion „schneller wächst“ (z.B. \(\ln x\) vs. \(x\), \(x^2\) vs. \(e^x\)).  
  - Das ist eng verwandt mit dem, was du beim Vergleich von Laufzeiten \(O(\log n)\), \(O(n)\), \(O(n^2)\) machst.

- **<span style="color:green">GRÜN</span>:** Historischer Hintergrund  
  - Benannt nach Guillaume de l’Hospital (französischer Mathematiker), Idee stammt von Johann Bernoulli.

---

## 7. Typische Fehler (Prüfungsfallen)

- **Fehler 1: L’Hospital angewendet ohne unbestimmte Form**  
  Beispiel:
  \[
  \lim_{x \to 1} \frac{x^2}{x} = \frac{1}{1} = 1
  \]
  → direkt ausrechnen, keine L’Hospital-Regel nötig.  
  **Relevanz:** **<span style="color:gold">GELB</span>**

- **Fehler 2: Ableitung falsch oder unvollständig**  
  - Ein kleiner Ableitungsfehler zerstört das Ergebnis.  
  - Besonders wichtig bei \(e^x\), \(\ln x\), \(\sin x\), \(\cos x\).  
  **Relevanz:** **<span style="color:red">ROT</span>**

- **Fehler 3: L’Hospital als „Wahrheitsmaschine“ missverstanden**  
  - Wenn der Grenzwert von \(\frac{f'(x)}{g'(x)}\) **nicht existiert** (und auch nicht uneigentlich \(\pm\infty\) ist), kann L’Hospital **keine Aussage** über den ursprünglichen Grenzwert machen.  
  - Der ursprüngliche Grenzwert kann trotzdem existieren.  
    Beispiel: \(\lim_{x\to\infty} \frac{x + \sin x}{x} = 1\), aber \(\lim_{x\to\infty} \frac{1 + \cos x}{1}\) existiert nicht (und geht auch nicht gegen \(\pm\infty\)).  
  **Relevanz:** **<span style="color:gold">GELB</span>**

---

## 8. Lösungen zu den Aufgaben (Kontrolle)

> **Empfehlung:** Erst wirklich selbst rechnen, dann hier vergleichen.

### Lösung zu Aufgabe 1

\[
\lim_{x \to 0} \frac{\sin x}{x}
\]

1. Einsetzen: \(\sin 0 = 0\), \(x = 0\) → \(0/0\).  
2. Ableiten:
   - \(f(x) = \sin x \Rightarrow f'(x) = \cos x\)  
   - \(g(x) = x \Rightarrow g'(x) = 1\)

\[
\lim_{x \to 0} \frac{\sin x}{x}
= \lim_{x \to 0} \frac{\cos x}{1}
= \cos 0
= 1
\]

**Ergebnis:** \(\boxed{1}\)

---

### Lösung zu Aufgabe 2

\[
\lim_{x \to \infty} \frac{4x^2 - 1}{2x^2 + 3}
\]

**Weg 1: L’Hospital**

1. Für \(x \to \infty\): Zähler \(\to \infty\), Nenner \(\to \infty\) → \(\infty/\infty\).  
2. Ableiten:
   - \(f(x) = 4x^2 - 1 \Rightarrow f'(x) = 8x\)  
   - \(g(x) = 2x^2 + 3 \Rightarrow g'(x) = 4x\)

\[
\lim_{x \to \infty} \frac{4x^2 - 1}{2x^2 + 3}
= \lim_{x \to \infty} \frac{8x}{4x}
\]

Da \(x \neq 0\) (und \(x \to \infty\)), kürzt sich \(x\) heraus:

\[
\frac{8x}{4x} = \frac{8}{4} = 2
\]

Also:

\[
\lim_{x \to \infty} \frac{8x}{4x} = 2
\]

**Weg 2: Ohne L’Hospital (durch \(x^2\) teilen)**

\[
\frac{4x^2 - 1}{2x^2 + 3}
= \frac{4 - \frac{1}{x^2}}{2 + \frac{3}{x^2}}
\]

Für \(x \to \infty\) gilt \(\frac{1}{x^2} \to 0\), also:

\[
\lim_{x \to \infty} \frac{4 - \frac{1}{x^2}}{2 + \frac{3}{x^2}}
= \frac{4 - 0}{2 + 0}
= 2
\]

**Ergebnis:** \(\boxed{2}\)

---

### Lösung zu Aufgabe 3

\[
\lim_{x \to 0} \frac{1 - \cos x}{x^2}
\]

1. Einsetzen: \(1 - \cos 0 = 1 - 1 = 0\), \(x^2 = 0\) → \(0/0\).  
2. Erste Ableitung:
   - \(f(x) = 1 - \cos x \Rightarrow f'(x) = \sin x\)  
   - \(g(x) = x^2 \Rightarrow g'(x) = 2x\)

\[
\lim_{x \to 0} \frac{1 - \cos x}{x^2}
= \lim_{x \to 0} \frac{\sin x}{2x}
\]

Einsetzen: \(\sin 0 = 0\), \(2 \cdot 0 = 0\) → wieder \(0/0\).

3. Zweite Ableitung:
   - \(\sin x \Rightarrow \cos x\)  
   - \(2x \Rightarrow 2\)

\[
\lim_{x \to 0} \frac{\sin x}{2x}
= \lim_{x \to 0} \frac{\cos x}{2}
= \frac{\cos 0}{2}
= \frac{1}{2}
\]

**Ergebnis:** \(\boxed{\frac{1}{2}}\)

---

### Lösung zu Aufgabe 4

\[
\lim_{x \to 0} \frac{e^x - 1}{x}
\]

1. Einsetzen: \(e^0 - 1 = 1 - 1 = 0\), \(x = 0\) → \(0/0\).  
2. Ableiten:
   - \(f(x) = e^x - 1 \Rightarrow f'(x) = e^x\)  
   - \(g(x) = x \Rightarrow g'(x) = 1\)

\[
\lim_{x \to 0} \frac{e^x - 1}{x}
= \lim_{x \to 0} \frac{e^x}{1}
= e^0
= 1
\]

**Ergebnis:** \(\boxed{1}\)

---

### Lösung zu Aufgabe 5

\[
\lim_{x \to \infty} \frac{\ln x}{x}
\]

1. Für \(x \to \infty\): \(\ln x \to \infty\), \(x \to \infty\) → \(\infty/\infty\).  
2. Ableiten:
   - \(f(x) = \ln x \Rightarrow f'(x) = \frac{1}{x}\)  
   - \(g(x) = x \Rightarrow g'(x) = 1\)

\[
\lim_{x \to \infty} \frac{\ln x}{x}
= \lim_{x \to \infty} \frac{1/x}{1}
= \lim_{x \to \infty} \frac{1}{x}
= 0
\]

**Ergebnis:** \(\boxed{0}