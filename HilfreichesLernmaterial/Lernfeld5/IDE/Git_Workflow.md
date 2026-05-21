 Git-Workflow in der Praxis: Vom Ändern bis zum Synchronisieren

Damit die drei Zustände und die Remote-Befehle nicht nur graue Theorie bleiben, schauen wir uns jetzt einen kompletten Arbeitszyklus Schritt für Schritt an.
📊 Die Zustände als Kreislauf (erweiterte Sicht)
text

[Working Directory]          [Staging Area]            [Lokales Repository]        [Remote-Server (z. B. GitHub)]
    modified  ── git add ──>    staged   ── git commit ──>    committed
                                                                   │
                                                                   git push ──>  remote
                                                                   │
                                                                   git pull <──  remote

    Modified – Datei wurde verändert, Git hat die Änderung bemerkt, aber noch nichts vorgemerkt.

    Staged – Die Änderung wurde mit git add in die „Vormerkliste“ aufgenommen.

    Committed – Ein unveränderlicher Snapshot wurde in der lokalen Historie gespeichert.

    Remote – Der Commit wurde mit git push auf den gemeinsamen Server hochgeladen (bzw. mit git pull holst du dir neue Commits von dort).

🧭 Von Null auf synchronisiert – ein durchgängiges Beispiel

Stell dir vor, du arbeitest an einer Datei index.html in einem Teamprojekt.

1. Status prüfen (jederzeit möglich)
bash

git status

Git sagt dir z. B.:
text

Changes not staged for commit:
  modified:   index.html

→ Die Datei ist modified.

2. Änderung vormerken (stage)
bash

git add index.html

Jetzt ist die Datei staged. Ein erneutes git status zeigt:
text

Changes to be committed:
  modified:   index.html

3. Snapshot erstellen (commit)
bash

git commit -m "Header-Layout korrigiert"

Die Änderung ist nun committed und dauerhaft in deiner lokalen Historie gespeichert. Der Commit hat einen eindeutigen Hash (z. B. a1b2c3d).

4. Lokale Commits auf den Remote-Server hochladen
bash

git push origin main

    origin = Name des Remote-Servers (Standard)

    main = Der Branch, auf den du pushen möchtest

Jetzt liegt dein Commit auch auf dem Server, und alle Teammitglieder können ihn sehen.

5. Änderungen anderer herunterladen und integrieren
bash

git pull

Dieser Befehl ist eigentlich eine Kombination aus zwei Schritten:

    git fetch – Holt alle neuen Commits vom Remote, verändert aber noch nichts an deinen eigenen Dateien.

    git merge – Führt die neuen Commits mit deinem aktuellen Branch zusammen.

    ⚠️ Achtung Merge-Konflikt:
    Wenn du und ein Teammitglied dieselbe Zeile in einer Datei verändert habt, kann Git nicht automatisch entscheiden, welche Version gelten soll. Dann entsteht ein Konflikt. Git markiert die betroffenen Stellen in der Datei, und du musst manuell die richtige Version auswählen, die Datei erneut git add und abschließend git commit ausführen.

💻 Kompakter Befehlsüberblick
Befehl	Bedeutung / Wirkung
git status	Zeigt Zustände aller Dateien an (modified, staged, …)
git add <datei>	Datei in die Staging-Area aufnehmen (modified → staged)
git commit -m "Text"	Snapshot der Staging-Area erstellen (staged → committed)
git push origin main	Lokale Commits auf den Remote-Server hochladen
git pull	Neue Commits vom Remote holen und in den aktuellen Branch integrieren
git log --oneline	Zeigt die lokale Commit-Historie kompakt an
✅ Erreichst du jetzt diese drei Kompetenzen? Teste dich selbst

    K1:

        Kannst du erklären, was passiert, wenn du eine Datei bearbeitest, ohne git add auszuführen? (→ sie ist modified)

        Was versteht man unter der Staging-Area? (→ staged = vorgemerkte Änderungen für den nächsten Commit)

        Wann ist eine Änderung endgültig und unveränderlich in Git gespeichert? (→ nach commit)

    K3 (add/commit):

        Führe in einem Testprojekt eine kleine Änderung durch, stage sie mit git add und committe sie mit einer sprechenden Nachricht. Funktioniert es?

    K3 (push/pull):

        Richte ein Remote-Repository ein (z. B. auf GitHub) und pushe einen Commit. Hole anschließend mit git pull eine Änderung eines anderen Branches (oder simuliere es). Verstehst du den Unterschied zwischen fetch und pull?

Wenn du diese Fragen sicher beantworten und die Befehle ausführen kannst, sind die drei Lernziele erreicht. Die oben stehende Tabelle kannst du dir als Spickzettel aufbewahren. Viel Erfolg! 💪