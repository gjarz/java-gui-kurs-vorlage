# Lab 04: Kontoverwaltung mit grafischer Oberfläche

**Dauer:** 2 Einheiten

## Lernziele

- Du bettest die Klassen aus Lab 02 in eine JavaFX-Oberfläche ein.
- Du trennst Fachlogik (Model) und Oberfläche (View).
- Du baust ein Formular mit `GridPane`, `ComboBox`, `TextField` und `TextArea`.
- Du zeigst Fehler der Fachlogik als Dialog an.

## Aufgabenstellung

Du erstellst eine neue grafische Anwendung für die Kontoverwaltung. Die Klassen aus Lab 02 verwendest du weiter.

### Vorbereitung

1. Kopiere die Klassen `Konto`, `Girokonto`, `Sparkonto`, `Kreditkonto`, `Kontoart` und `KontoException` aus Lab 02 in das Paket `kurs.konto.model` von Lab 04.
2. Kopiere auch deine Tests aus Lab 02 nach `src/test/java` und führe `mvn test` aus. Alle Tests müssen grün sein, bevor du die Oberfläche baust.
3. Lege die Oberfläche in das Paket `kurs.konto.gui` und die Startklasse `Launcher` in das Paket `kurs.konto`. Die Vorlage von Lab 03 zeigt dir den Aufbau.

Die Oberfläche enthält **keine Berechnungen und keine Regeln**. Sie ruft nur Methoden des Models auf. Löst das Model eine `KontoException` aus, zeigt die Oberfläche die Meldung in einem `Alert` an.

### Teil 1: Konten anlegen

Das Fenster hat links ein Formular und rechts ein langes Textfeld, in dem die Anwendung die Ausgaben zeilenweise anzeigt.

```text
+-----------------------------------------------------------+
| Kontoverwaltung                                           |
+-------------------------------+---------------------------+
| Kontoart        [Girokonto v] | Verlauf                   |
| Kontonummer     [___________] | +---------------------+   |
| Kontoinhaber    [___________] | | Konto 10001000      |   |
| Gebühr / Zinsen [___________] | | angelegt für ...    |   |
| Kontostand      [___________] | |                     |   |
|                               | |                     |   |
| [Konto anlegen]   [Beenden]   | +---------------------+   |
+-------------------------------+---------------------------+
```

- Die `ComboBox` bietet die drei Kontoarten an.
- „Konto anlegen“ prüft die Eingaben, legt ein Konto der gewählten Art an und schreibt eine Zeile in den Verlauf.
- Die Anwendung speichert die Konten nicht dauerhaft. Sie verschwinden beim Schließen.
- „Beenden“ schließt die Anwendung.
- Fehlerhafte Eingaben (leere Felder, ungültige Zahl) zeigt die Anwendung in einem `Alert` an. Sie stürzt dabei nicht ab.

### Teil 2: Konten bedienen

Erweitere die Oberfläche um einen Bedienbereich:

- Eine `ListView` oder `ComboBox` zeigt alle angelegten Konten.
- Ein Feld „Betrag“ mit den Buttons **Einzahlen**, **Abheben** und **Kontoauszug**.
- Eine Auswahl „Zielkonto“ mit dem Button **Überweisen**.
- Jede Aktion schreibt eine Zeile in den Verlauf.
- Ein Menü **Datei** enthält **Beenden** (Strg+Q), ein Menü **Hilfe** enthält **Über**.

Hinweise zu den Steuerelementen findest du in der [JavaFX Übersicht](../materialien/javafx-uebersicht.md).

### Erweiterungen (freiwillig)

- Ein Button **Verlauf speichern** schreibt den Verlauf in eine Textdatei (`FileChooser`).
- Die ListView färbt Konten mit negativem Kontostand rot.
- Die Buttons sind deaktiviert, solange kein Konto ausgewählt ist (`disableProperty().bind(...)`).

## Hinweise

- Das Textfeld rechts ist nicht editierbar: `verlauf.setEditable(false)`. Zeilen fügst du mit `verlauf.appendText("...\n")` an.
- `GridPane` ordnet Elemente in Zeilen und Spalten an: `grid.add(element, spalte, zeile)`.
- Zahlen aus Textfeldern wandelst du mit `new BigDecimal(text.trim().replace(',', '.'))` um. Fange die `NumberFormatException` ab.

## Reflexionsfragen für deine README

1. Warum enthält die Oberfläche keine Geschäftsregeln?
2. Welchen Vorteil bringt es, dass du die Tests aus Lab 02 unverändert wiederverwenden kannst?
3. Wie stellst du sicher, dass eine fehlerhafte Eingabe das Programm nicht beendet?

## Checkliste

- [ ] Die Model-Klassen sind kopiert und alle Tests laufen.
- [ ] Anlegen, Einzahlen, Abheben, Kontoauszug und Überweisen funktionieren in der Oberfläche.
- [ ] Fehler erscheinen als `Alert`, nicht als Absturz.
- [ ] Der Verlauf erscheint zeilenweise im Textfeld rechts.
- [ ] Die README enthält Screenshots der wichtigsten Abläufe und beantwortet die Fragen.
- [ ] Der letzte Stand ist gepusht.
