# VS Code einrichten und Tests schreiben

## Installation

| Werkzeug | Hinweis |
|---|---|
| JDK 21 | zum Beispiel *Eclipse Temurin 21*. Prüfe mit `java -version`. |
| Visual Studio Code | Erweiterungen aus `.vscode/extensions.json` installieren. Die wichtigste ist das *Extension Pack for Java*. |
| Apache Maven | Installiere Maven und prüfe mit `mvn -version`. Unter Windows geht `winget install Apache.Maven`, unter Linux `sudo apt install maven`. |
| Git | Prüfe mit `git --version`. |

Zeigt `mvn -version` ein anderes Java als `java -version`, setzt du die Umgebungsvariable `JAVA_HOME` auf das JDK 21.

## Ein Maven-Projekt verstehen

```text
lab-01-vererbung/
├── pom.xml                      Beschreibung des Projekts und seiner Bibliotheken
└── src/
    ├── main/
    │   ├── java/                dein Programm, in Paketordnern (kurs/vererbung/Person.java)
    │   └── resources/           Bilder, Textdateien, Stylesheets
    └── test/
        └── java/                deine Tests
```

Die Ordnerstruktur ist ein Standard. Halte dich daran, dann findet Maven alles selbst.

| Befehl im Lab-Ordner | Wirkung |
|---|---|
| `mvn compile` | übersetzt dein Programm |
| `mvn test` | führt alle Tests aus |
| `mvn javafx:run` | startet ein JavaFX-Programm (nur Labs mit GUI) |
| `mvn clean` | löscht den Ordner `target` |

Beim ersten Mal lädt Maven Bibliotheken aus dem Internet. Das dauert einen Moment.

## Programme starten

- **Konsolenprogramm:** Klicke über der `main`-Methode auf **Run**. Für Programme mit `Scanner` ist die Einstellung `java.debug.settings.console` auf `integratedTerminal` gesetzt, damit du im Terminal tippen kannst.
- **GUI-Programm:** Starte mit `mvn javafx:run` im Lab-Ordner oder über die Klasse `Launcher`.

## Tests mit JUnit 5

Ein Test prüft automatisch, ob ein Stück Code das tut, was es soll. Du schreibst ihn einmal und führst ihn nach jeder Änderung aus.

Eine Klasse `Mathe` im Ordner `src/main/java/kurs/demo/`:

```java
package kurs.demo;

public final class Mathe {

    private Mathe() {
    }

    /** Größter gemeinsamer Teiler nach Euklid. */
    public static long ggt(long a, long b) {
        if (a < 0 || b < 0) {
            throw new IllegalArgumentException("Nur nichtnegative Zahlen erlaubt.");
        }
        return b == 0 ? a : ggt(b, a % b);
    }
}
```

Die passenden Tests liegen im Ordner `src/test/java/kurs/demo/`:

```java
package kurs.demo;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertThrows;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;

class MatheTest {

    @Test
    void ggtVonZwoelfUndAchtzehnIstSechs() {
        assertEquals(6, Mathe.ggt(12, 18));
    }

    @ParameterizedTest
    @CsvSource({"7, 13, 1", "0, 5, 5", "48, 36, 12"})
    void ggtFuerMehrereFaelle(long a, long b, long erwartet) {
        assertEquals(erwartet, Mathe.ggt(a, b));
    }

    @Test
    void negativeZahlLoestExceptionAus() {
        assertThrows(IllegalArgumentException.class, () -> Mathe.ggt(-1, 5));
    }
}
```

| Baustein | Bedeutung |
|---|---|
| `@Test` | markiert eine Testmethode |
| `assertEquals(erwartet, tatsaechlich)` | prüft auf Gleichheit. Der **erwartete Wert steht zuerst**. |
| `assertThrows(Klasse, () -> ...)` | prüft, dass eine Exception auftritt |
| `assertTrue`, `assertFalse`, `assertNotNull` | weitere Prüfungen |
| `@ParameterizedTest` mit `@CsvSource` | derselbe Test mit mehreren Eingaben |

### Tests ausführen

- In VS Code öffnest du die Ansicht **Testing** (Reagenzglas-Symbol) und klickst auf den Pfeil.
- Auf der Konsole: `mvn test` im Lab-Ordner.

### Gute Tests

- Jeder Test prüft **eine** Sache und hat einen Namen, der sie beschreibt.
- Teste auch die **Grenzfälle** (0, negative Zahlen, leere Eingaben) und die **Fehlerfälle**.
- Teste die Fachlogik, nicht die Oberfläche. Deshalb trennst du beide.
- Gehe nach dem Muster vor: Vorbereiten, Ausführen, Prüfen.

## Häufige Probleme

| Problem | Lösung |
|---|---|
| `mvn` wird nicht gefunden | Maven ist nicht installiert oder nicht im `PATH`. Terminal neu starten. |
| VS Code zeigt rote Fehler, obwohl alles stimmt | Führe *Java: Clean Java Language Server Workspace* aus (Befehlspalette `Strg+Shift+P`). |
| `release version 21 not supported` | Dein JDK ist älter als 21. Installiere JDK 21 und setze `JAVA_HOME`. |
| Maven-Projekt wird nicht erkannt | Öffne den **Hauptordner** des Repositories, nicht einen Unterordner. |
| Umlaute oder `€` erscheinen falsch | Starte das Programm im Terminal statt in der Debug-Konsole. Nutze unter Windows das *Windows Terminal*. Alternativ startest du Java mit `-Dstdout.encoding=UTF-8`. |
| Tests werden nicht gefunden | Der Dateiname endet nicht auf `Test` oder die Datei liegt nicht in `src/test/java`. |
