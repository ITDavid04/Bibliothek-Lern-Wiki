# **🔧 Überarbeitete Anleitung: Python-Code mit GitHub Actions automatisch prüfen lassen**

*(Für Umschüler – Schritt für Schritt, einfach erklärt mit echten Fehlern!)*

---

## **📌 Worum geht’s?**

Du lernst Python und willst **sauberen Code** schreiben, der keine Fehler hat?
Mit **GitHub Actions** kannst du einen **automatischen Checker (Linter)** einrichten, der **jeden Code prüft**, den du hochlädst – **bevor andere ihn sehen!**

🔹 **Was du lernst:**
✅ Ein **GitHub-Konto** erstellen
✅ Ein **Python-Projekt** auf GitHub hochladen
✅ Einen **automatischen Linter (flake8)** einrichten
✅ **Echte Fehler** erkennen und beheben

🔹 **Was du brauchst:**

- Einen **Computer mit Internet**
- **Geduld** (jeder Schritt ist einfach!)
- **Keine Vorkenntnisse** nötig!

---

---

## **📌 Schritt 1: GitHub-Konto erstellen (falls noch nicht vorhanden)**

GitHub ist wie eine **Cloud für Programmierer**, wo du deinen Code speicherst.

👉 **So geht’s:**

1. **Öffne [github.com](https://github.com) in deinem Browser.**
2. Klicke auf **"Sign up"** (oben rechts).
3. Gib deine **E-Mail-Adresse** ein und folge den Anweisungen.
   - Wähle einen **Benutzernamen** (z. B. `DeinName123`).
   - Wähle ein **sicheres Passwort**.
   - Bestätige deine E-Mail.
4. **Fertig!** Du hast jetzt ein GitHub-Konto.

⚠️ **Tipp:**

- Nutze eine **E-Mail-Adresse, die du oft checkst** (falls du mal dein Passwort vergisst).
- GitHub schickt dir manchmal E-Mails – schau regelmäßig in dein Postfach!

---

---

## **📌 Schritt 2: Python-Code auf GitHub hochladen**

Wir brauchen **irgendeinen Python-Code**, den der Linter prüfen soll. Dafür erstellen wir ein **Mini-Projekt**.

### **🔹 Option A: Code direkt auf GitHub schreiben (einfachste Methode)**

1. **Logge dich auf [github.com](https://github.com) ein.**
2. Klicke oben rechts auf **"+" → "New repository"**.
3. **Gib folgende Daten ein:**
   - **Repository name:** `python-lint-test` (oder einen anderen Namen)
   - **Description:** `Ein Testprojekt für Python-Linting`
   - **Public/Private:** Wähle **"Public"** (damit alle sehen können, dass du lernst 😊)
   - **Häkchen bei:** ✅ **Add a README file** (wichtig!)
   - **Klicke auf "Create repository"**

4. **Jetzt sind wir im leeren Projekt.**
   - Klicke auf **"Add file" → "Create new file"**.
   - **Dateinamen:** `test.py` (Python-Dateien enden immer mit `.py`)
   - **Inhalt:**

     ```python
     def hallo_welt():
         print("Hallo Welt!")

     hallo_welt()
     ```

   - Klicke unten auf **"Commit new file"**.

✅ **Fertig!** Dein erster Python-Code ist jetzt auf GitHub.

---

### **🔹 Option B: Code von deinem Computer hochladen (falls du Python lokal installiert hast)**

Falls du **Python auf deinem Computer installiert** hast, kannst du den Code auch von dort hochladen.

1. **Installiere Git auf deinem Computer:**
   - [Git herunterladen und installieren](https://git-scm.com/downloads)
   - Folge den Schritten (einfach "Next" klicken).

2. **Öffne die **Git Bash** (Windows) oder das **Terminal** (Mac/Linux).**
   *(Achtung: In der normalen Windows-Eingabeaufforderung funktioniert `echo -e` nicht! Nutze immer Git Bash.)*

3. **Gib folgende Befehle ein:**

   ```bash
   git config --global user.name "DeinName"
   git config --global user.email "deine@email.de"

   ```
   *(Ersetze die E-Mail mit deiner GitHub-E-Mail.)*

4. **Lade den Code hoch:**

   ```bash
   git clone https://github.com/DeinBenutzername/python-lint-test.git
   cd python-lint-test
   echo -e 'def hallo_welt():\n    print("Hallo Welt!")\n\nhallo_welt()' > test.py
   git add .
   git commit -m "Erster Python-Code"
   git push
   ```

   *(Der `-e`-Parameter sorgt dafür, dass `\n` als Zeilenumbruch interpretiert wird. **Wichtig:** Nutze Git Bash, nicht die Windows-Eingabeaufforderung!)*

✅ **Fertig!** Dein Code ist jetzt auf GitHub.

---

---

## **📌 Schritt 3: Den automatischen Linter einrichten**

Jetzt kommt der **coole Teil**: Wir sagen GitHub, dass es **automatisch prüfen soll**, ob dein Code Fehler hat!

### **🔹 Schritt 3.1: Eine neue Datei für GitHub Actions erstellen**

1. **Gehe zu deinem Repository auf GitHub.**
2. Klicke auf **"Add file" → "Create new file"**.
3. **Gib folgenden Pfad ein:**

   ```
   .github/workflows/lint.yml
   ```

   *(Das ist der Ort, wo GitHub Actions seine Regeln speichert.)*

4. **Füge diesen Code ein:**

   ```yaml
   name: Python Linting

   on: [push]

   jobs:
     lint:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v4
         - name: Set up Python
           uses: actions/setup-python@v5
           with:
             python-version: '3.x'
         - name: Install dependencies
           run: |
             python -m pip install --upgrade pip
             pip install flake8
         - name: Run flake8 (nur auf Python-Dateien!)
           run: flake8 . --filename '*.py'
   ```

   *(Kopiere einfach den Text und füge ihn ein!)*

5. **Klicke auf "Commit new file" (unten).**

✅ **Fertig!** Jetzt prüft GitHub **jeden neuen Code**, den du hochlädst!

---

### **🔹 Schritt 3.2: Überprüfen, ob der Linter funktioniert**

1. **Ändere deinen Code ein bisschen** (wir bauen **einen echten Fehler** ein!):
   - Öffne `test.py` und ändere es zu:

     ```python
     def hallo_welt():
         print("Diese Zeile ist absichtlich viel zu lang, um einen Linting-Fehler zu demonstrieren")  # Fehler: Zeile zu lang!

     hallo_welt()

     ```

   - **Speichere die Datei** und lade sie hoch:
     - Klicke auf **"Commit changes"** (unten rechts).

2. **Warte 1-2 Minuten.**
3. **Gehe zu "Actions" (oben im Repository).**
   - Du siehst jetzt einen **laufenden Job** namens **"Python Linting"**.
   - **Klicke auf den Eintrag "Python Linting"** und dann auf den Job **"lint"**, um die Fehlermeldungen zu sehen.
   - Falls du den Lauf verpasst hast, klicke auf **"Re-run all jobs"**, um ihn neu zu starten.

4. **Du siehst jetzt eine Fehlermeldung wie:**

   ```
   test.py:2:80: E501 line too long (85 > 79 characters)

   ```
   → **Das bedeutet:** Die Zeile ist zu lang (85 Zeichen, aber nur 79 erlaubt)!

5. **Behebe den Fehler:**
   - Kürze die Zeile auf unter 80 Zeichen:

     ```python
     def hallo_welt():
         print("Hallo Welt!")  # Jetzt nur noch 20 Zeichen

     hallo_welt()

     ```
   - **Speichere die Datei und lade sie neu hoch.**
   - **Warte, bis der Linter läuft.**
   - **Jetzt sollte alles grün sein!** ✅

---
---

## **📌 Schritt 4: Fehlermeldungen verstehen und beheben**

Der Linter zeigt dir **genau an, wo das Problem ist**.

### **🔹 Beispiel-Fehlermeldung:**

```

test.py:2:80: E501 line too long (85 > 79 characters)
```

**Was bedeutet das?**

- **`test.py:2:80`** → Fehler in **Zeile 2, Spalte 80**
- **`E501`** → Der Fehlercode (hier: "Zeile zu lang")
- **`line too long (85 > 79 characters)`** → Die Zeile ist **85 Zeichen lang**, aber nur **79 erlaubt**

### **🔹 Häufige Fehler und wie man sie behebt:**

| Fehlercode | Bedeutung | Beispiel | Lösung |
|------------|-----------|----------|--------|
| **E501** | Zeile zu lang | `print("Diese Zeile ist viel zu lang")` | Kürze die Zeile oder teile sie auf. |
| **W291** | Überflüssige Leerzeichen am Zeilenende | `print("Hallo") ` | Entferne die Leerzeichen. |
| **E201/E202** | Zu viele Leerzeichen vor/nach Klammern | `print( "Hallo" )` | Schreibe `print("Hallo")`. |
| **E111** | Falsche Einrückung (4 Leerzeichen pro Ebene) | `  print("Hallo")` | Nutze **4 Leerzeichen** pro Ebene. |

🔹 **Tipp:**

- Wenn du eine Fehlermeldung nicht verstehst, **kopiere den Fehlercode (z. B. E501) und suche ihn bei Google**!
- Beispiel: *"flake8 E501 line too long"* → Du findest sofort Erklärungen.

---
---

## **📌 Wichtige Begriffe erklärt (für Anfänger)**

| Begriff | Erklärung |
|---------|-----------|
| **Repository** | Ein Ordner auf GitHub, wo dein Code gespeichert ist. |
| **Commit** | Ein "Speichern" deines Codes mit einer Nachricht (z. B. "Erster Code"). |
| **Push** | Deinen Code von deinem Computer auf GitHub hochladen. |
| **Pull** | Code von GitHub auf deinen Computer laden. |
| **Linter** | Ein Programm, das deinen Code auf Fehler prüft (wie ein Lehrer!). |
| **flake8** | Ein beliebter Python-Linter, der Syntax- und Stilfehler findet. |
| **GitHub Actions** | Automatische Abläufe, die z. B. deinen Code prüfen, sobald du ihn hochlädst. |
| **Workflow** | Eine Anleitung für GitHub Actions (z. B. "Prüfe den Code mit flake8"). |
| **Branch** | Eine Version deines Codes (z. B. `main` ist die Hauptversion). |

---
---

## **📌 Häufige Probleme & Lösungen**

| Problem | Lösung |
|---------|--------|
| **"404: Repository not found"** | Du hast die URL falsch geschrieben. Prüfe, ob der Repository-Name stimmt. |
| **Workflow startet nicht** | Warte 1-2 Minuten und aktualisiere die Seite. Falls immer noch nichts passiert: Prüfe, ob die YAML-Datei korrekt ist (Leerzeichen, keine Tabs!). |
| **Fehler in der YAML-Datei** | Die Einrückungen müssen mit **Leerzeichen** gemacht werden (keine Tabs!). |
| **Flake8 findet keine Fehler, obwohl welche da sind** | Der Linter prüft nur **Python-Dateien** (`.py`). Falls du andere Dateien hast, ignoriere sie. |
| **Lokale flake8-Installation funktioniert nicht** | Stelle sicher, dass Python installiert ist (`python --version`). Falls nicht, installiere es von [python.org](https://www.python.org/downloads/). |
| **Git-Befehle funktionieren nicht** | Öffne die **Git Bash** (Windows) oder das **Terminal** (Mac/Linux) und navigiere mit `cd` in den richtigen Ordner. |
| **echo -e funktioniert nicht** | Nutze **Git Bash** (Windows) oder das Terminal (Mac/Linux). In der normalen Windows-Eingabeaufforderung funktioniert `echo -e` nicht! |

---
---

## **📌 Tipps & Tricks für den Alltag**

🔹 **Tipp 1: Lokale Installation von flake8 (für schnelles Feedback)**
Falls du Python auf deinem Computer installiert hast, kannst du **flake8 lokal ausführen**, bevor du den Code hochlädst:

```bash
pip install flake8
flake8 .
```

→ So siehst du die Fehler **bevor** du sie hochlädst!

🔹 **Tipp 2: Automatische Korrektur mit `autopep8`**
Falls du Fehler beheben willst, kannst du `autopep8` installieren:

```bash
pip install autopep8
autopep8 --in-place --aggressive test.py
```

→ Das korrigiert **einige Fehler automatisch**!

🔹 **Tipp 3: Workflow optimieren**

- **Nur bei bestimmten Branches prüfen** (z. B. `main` oder `develop`):

  ```yaml
  on:
    push:
      branches: [ "main" ]
  ```

- **Nur bei Pull Requests prüfen** (wenn jemand deinen Code überprüft):

  ```yaml
  on:
    pull_request:
      branches: [ "main" ]
  ```

🔹 **Tipp 4: README-Datei aktualisieren**
Füge in deine `README.md` ein, dass dein Projekt **automatisch geprüft wird**:

```markdown
## 🔍 Code-Qualität
Dieses Projekt wird automatisch mit **flake8** geprüft.
✅ Keine Fehler? Super!
❌ Fehler? Behebe sie und lade neu hoch!
```

🔹 **Tipp 5: Mit anderen zusammenarbeiten**

- Lade Freunde oder Kollegen ein, dein Repository zu sehen (`Settings → Collaborators`).
- Sie können **Code-Review** machen und dir helfen!

---
---

## **🎉 Herzlichen Glückwunsch!**

Du hast gerade gelernt, wie man:
✔️ Ein GitHub-Konto erstellt
✔️ Python-Code hochlädst
✔️ Einen **automatischen Linter** einrichtet
✔️ **Echte Fehler** erkennt und behebt

**Jetzt kannst du:**

- **Sofort Fehler sehen**, bevor du sie hochlädst!
- **Sauberen Code schreiben**, der den Standards entspricht.
- **Mit anderen zusammenarbeiten**, ohne dass sie deine Fehler korrigieren müssen.

🚀 **Nächste Schritte:**

- Probiere aus, **mehrere Python-Dateien** zu prüfen!
- Lerne **andere Linter** wie `black` (formatiert deinen Code automatisch).
- Baue **komplexere Workflows** (z. B. Tests automatisch ausführen).

---

### **💬 Brauchst du Hilfe?**

Falls etwas nicht klappt:

1. **Google ist dein Freund!** Suche nach Fehlermeldungen.

2. **Frage in Foren** wie [Stack Overflow](https://stackoverflow.com/) oder [GitHub Discussions](https://github.com/orgs/community/discussions).

3. **Schreib mir eine Nachricht** – ich helfe gern!

**Du schaffst das!** 💪😊