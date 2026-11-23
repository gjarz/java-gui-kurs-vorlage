# Lab 00: Setup, Git und Markdown

**Dauer:** 1 Einheit

## Lernziele

- Du richtest JDK, VS Code, Maven und Git ein.
- Du legst dein Abgabe-Repository auf GitHub an.
- Du schreibst eine Dokumentation in Markdown.
- Du startest ein erstes Java-Programm in VS Code.

## Aufgabenstellung

### 1. Werkzeuge prüfen

Öffne ein Terminal und prüfe die Versionen:

```bash
java -version
javac -version
git --version
mvn -version
```

Das JDK muss mindestens Version 21 haben. Fehlt etwas, installiere es nach [VS Code und Tests](../materialien/vscode-und-tests.md).

### 2. Repository anlegen

1. Erstelle einen GitHub-Account, falls du noch keinen hast.
2. Öffne die Kursvorlage auf GitHub und klicke **Use this template, Create a new repository**.
3. Nenne das Repository `java-gui-<nachname>` und stelle es auf **Private**.
4. Gib das Repository für `gjarz` frei: *Settings, Collaborators, Add people*.
5. Klone es auf deinen Rechner:

   ```bash
   git clone https://github.com/<dein-name>/java-gui-<nachname>.git
   ```

6. Öffne den geklonten Ordner in VS Code (*File, Open Folder*) und installiere die empfohlenen Erweiterungen.

### 3. Hello World

Lege in `lab-00-setup/src/main/java/kurs/hallo/` die Klasse `HalloWelt` an. Sie gibt deinen Namen und das heutige Datum aus. Verwende dafür `java.time.LocalDate`.

Starte das Programm über den Run-Knopf oberhalb der `main`-Methode.

### 4. Dokumentation

Fülle die `README.md` in diesem Ordner aus. Sie muss enthalten:

- eine Überschrift mit mehreren Ebenen,
- eine nummerierte und eine nicht nummerierte Liste,
- einen Codeblock mit Sprachangabe,
- eine Tabelle,
- einen Link,
- mindestens einen Screenshot, der in `docs/` liegt und relativ eingebunden ist.

### 5. Abgabe

Committe und pushe deine Arbeit. Trage den Repository-Link in Moodle ein.

```bash
git add .
git commit -m "Lab 00 abgeschlossen"
git push
```

## Checkliste

- [ ] `java -version` zeigt 21 oder höher.
- [ ] Das Repository ist privat und für `gjarz` freigegeben.
- [ ] `HalloWelt` läuft in VS Code.
- [ ] Die `README.md` enthält alle geforderten Markdown-Elemente.
- [ ] Der Screenshot erscheint in der Vorschau (`Strg+Shift+V`).
- [ ] Der letzte Stand ist auf GitHub sichtbar.

## Hilfe

[Git und Markdown](../materialien/git-und-markdown.md)
