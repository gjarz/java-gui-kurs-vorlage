# Java: Objektorientierung und grafische Oberflächen

Kursvorlage für ITL1/ITL2 in der 2. Klasse APC. Du bearbeitest alle Übungen in **deinem eigenen Repository**, das du aus dieser Vorlage erzeugst. Deine Abgabe ist die `README.md` im jeweiligen Lab-Ordner.

## Auf einen Blick

| Lab | Thema | Einheiten | Technik |
|---|---|---|---|
| [00](lab-00-setup/AUFGABE.md) | Setup, Git, Markdown | 1 | VS Code, Maven, GitHub |
| [01](lab-01-vererbung/AUFGABE.md) | Vererbung | 1 | Java |
| [02](lab-02-kontoverwaltung/AUFGABE.md) | Kontoverwaltung (Konsole) | 2 | Polymorphie, Konsole |
| [03](lab-03-erste-gui/AUFGABE.md) | Erste GUI: Aufgabenliste | 1 | JavaFX |
| [04](lab-04-kontoverwaltung-gui/AUFGABE.md) | Kontoverwaltung mit GUI | 2 | JavaFX |
| [05](lab-05-designplanung/AUFGABE.md) | Designplanung Bruchrechner | 1 | Layoutentwurf |
| [06](lab-06-bruchrechner/AUFGABE.md) | Designumsetzung Bruchrechner | 2 | JavaFX, Lambdas |
| [07](lab-07-hangman/AUFGABE.md) | Hangman | 3 | JavaFX, Menü, Dateien |
| [08](lab-08-db-anbindung/AUFGABE.md) | Datenbankanbindung | 1 | MariaDB, JDBC |
| [09](lab-09-buchhaltung/AUFGABE.md) | Haushaltsbuch mit Datenbank | 3 | JavaFX, JDBC, Fehlerbehandlung |

Jeder Lab-Ordner enthält zwei Dateien:

- `AUFGABE.md` beschreibt, was du tun sollst. Du änderst sie nicht.
- `README.md` ist deine Abgabe. Du füllst sie aus.

## Einmalige Einrichtung

1. Installiere das **JDK 21** (zum Beispiel Eclipse Temurin), **Git** und **Visual Studio Code**.
2. Installiere in VS Code die Erweiterungen aus `.vscode/extensions.json`. VS Code bietet sie beim Öffnen des Ordners automatisch an. Die wichtigste ist das *Extension Pack for Java*.
3. Folge der Anleitung in [Lab 00](lab-00-setup/AUFGABE.md). Sie führt dich durch das Anlegen deines Repositories.

## Ablauf pro Lab

1. Lies `AUFGABE.md` im Lab-Ordner.
2. Programmiere im Ordner `src/`.
3. Mache regelmäßig Commits mit aussagekräftigen Nachrichten (`git commit -m "Klasse Employee ergänzt"`).
4. Fülle die `README.md` im Lab-Ordner aus. Lege Screenshots im Unterordner `docs/` ab und verlinke sie relativ.
5. Prüfe die Checkliste am Ende von `AUFGABE.md`.
6. Führe `git push` aus und trage den Link zu deinem Repository in der Moodle-Abgabe ein.

## Programme starten und testen

Öffne in VS Code den **Hauptordner** dieses Repositories. VS Code erkennt alle Lab-Projekte selbst.

| Aktion | Befehl im Lab-Ordner |
|---|---|
| Kompilieren | `mvn compile` |
| Tests ausführen | `mvn test` |
| GUI-Programm starten | `mvn javafx:run` |
| Konsolenprogramm | Run-Knopf über `main` in VS Code |

Starte GUI-Programme am sichersten mit `mvn javafx:run` im jeweiligen Lab-Ordner. Der Befehl setzt auch den richtigen Arbeitsordner.

## Materialien

- [Git und Markdown](materialien/git-und-markdown.md)
- [VS Code einrichten und Tests schreiben](materialien/vscode-und-tests.md)
- [Lambda-Ausdrücke](materialien/lambda-ausdruecke.md)
- [JavaFX Übersicht](materialien/javafx-uebersicht.md)
- [JDBC und SQL](materialien/jdbc-und-sql.md)

## Regeln für die Abgabe

- Dein Repository ist **privat**. Gib es für den Lehrer frei: *Settings, Collaborators, Add people*, GitHub-Name `gjarz`.
- Zugangsdaten (Passwörter, `.env`, `db.properties`) kommen **nie** ins Repository. Die `.gitignore` schützt dich, solange du die vorgegebenen Dateinamen verwendest.
- Du darfst KI-Werkzeuge verwenden. Du musst jede Zeile erklären können und gibst im Abschnitt „Quellen und Hilfsmittel“ deiner README an, wofür du sie genutzt hast. In den Fachgesprächen erklärst du deinen Code selbst.
