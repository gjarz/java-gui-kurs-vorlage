# Tests schreiben, Schritt für Schritt

Diese Anleitung zeigt dir am Beispiel `fullName()` aus Lab 01, wie du einen Test von der leeren Datei bis zum grünen Haken schreibst. Danach schreibst du die übrigen Tests von Lab 01 nach demselben Muster selbst.

Du brauchst dafür nur die Klasse `Person` aus Lab 01. Eine Kurzfassung zu JUnit steht in [VS Code und Tests](vscode-und-tests.md).

## Was ein Test ist

Ein Test ist ein kleines Programm, das dein Programm benutzt und das Ergebnis prüft. Du schreibst ihn einmal. Danach prüfst du mit einem Klick, ob nach jeder Änderung noch alles stimmt.

Jeder Test hat drei Teile:

| Teil | Frage | Beispiel |
|---|---|---|
| Vorbereiten | Welche Objekte und Daten brauche ich? | `new Person("Huber", "Anna")` |
| Ausführen | Welche Methode will ich prüfen? | `person.fullName()` |
| Prüfen | Was muss herauskommen? | `assertEquals("Anna Huber", ...)` |

## Ausgangslage

Du hast im Ordner `lab-01-vererbung/src/main/java/kurs/vererbung/` die Klasse `Person` mit einem Konstruktor und der Methode `fullName()`. Diese Version genügt für die Anleitung. Sie enthält absichtlich den Fehler, den du in Schritt 4 findest:

```java
package kurs.vererbung;

public class Person {

    private String lastName;
    private String firstName;

    public Person(String lastName, String firstName) {
        this.lastName = lastName;
        this.firstName = firstName;
    }

    public void setName(String lastName, String firstName) {
        this.lastName = lastName;
        this.firstName = firstName;
    }

    public String fullName() {
        return lastName + " " + firstName;
    }
}
```

Die Aufgabe verlangt „Vorname Nachname“, zum Beispiel `Anna Huber`. Die Methode liefert aber `Huber Anna`. Ein Test deckt das auf.

## Schritt 1: Die Testdatei anlegen

Testklassen liegen in einem eigenen Ordner, getrennt vom Programm. Das schreibt Maven so vor.

1. Öffne im Explorer von VS Code den Ordner `lab-01-vererbung/src/test/java`.
2. Lege dort die Paketordner `kurs/vererbung` an. Das Paket ist **dasselbe** wie bei `Person`.
3. Erstelle im Ordner `vererbung` die Datei `PersonTest.java`.

Das Ergebnis sieht so aus:

```text
lab-01-vererbung/
└── src/
    ├── main/java/kurs/vererbung/Person.java
    └── test/java/kurs/vererbung/PersonTest.java
```

Der Name der Testklasse ist der Name der geprüften Klasse plus `Test`. Endet der Name nicht auf `Test`, findet Maven den Test nicht.

## Schritt 2: Das Gerüst schreiben

Schreibe in `PersonTest.java`:

```java
package kurs.vererbung;

import static org.junit.jupiter.api.Assertions.assertEquals;

import org.junit.jupiter.api.Test;

class PersonTest {

}
```

Zeile für Zeile:

| Zeile | Bedeutung |
|---|---|
| `package kurs.vererbung;` | Die Testklasse liegt im selben Paket wie `Person`. Du brauchst deshalb keinen Import für `Person`. |
| `import static ...assertEquals;` | Holt die Prüfmethode `assertEquals` in die Datei. |
| `import org.junit.jupiter.api.Test;` | Holt die Markierung `@Test`. |
| `class PersonTest` | Die Klasse darf ohne `public` auskommen. JUnit 5 braucht es nicht. |

Wenn VS Code `org.junit` rot unterstreicht, fehlt die Bibliothek. Prüfe, ob die `pom.xml` im Lab-Ordner die Abhängigkeit `junit-jupiter` enthält und lade das Maven-Projekt neu (Maven-Ansicht, Symbol *Reload*).

## Schritt 3: Den ersten Test schreiben

Füge in die Klasse eine Methode ein:

```java
    @Test
    void fullNameLiefertVornameVorNachname() {
        // Vorbereiten
        Person person = new Person("Huber", "Anna");

        // Ausführen
        String ergebnis = person.fullName();

        // Prüfen
        assertEquals("Anna Huber", ergebnis);
    }
```

Was passiert hier:

- `@Test` markiert die Methode als Test. Ohne `@Test` läuft sie nie.
- Der Methodenname beschreibt, was gelten soll. Ein guter Name erklärt den Fehler schon, bevor du die Meldung liest.
- Die Methode hat keinen Rückgabewert und keine Parameter.
- `assertEquals(erwartet, tatsächlich)` vergleicht beide Werte. Der **erwartete** Wert steht **zuerst**. Wenn du die Reihenfolge vertauschst, stimmt die Fehlermeldung nicht mehr.
- Du schreibst den erwarteten Wert **von Hand** hin: `"Anna Huber"`. Berechne ihn nie mit demselben Code, den du prüfst, sonst prüft der Test nichts.

## Schritt 4: Den Test ausführen und den Fehler lesen

**In VS Code:**

1. Über der Methode steht die Zeile **Run Test | Debug Test**. Klicke auf **Run Test**.
2. Alternativ öffnest du die Ansicht **Testing** (Reagenzglas-Symbol links) und klickst auf den Pfeil neben `PersonTest`.

**Im Terminal** im Ordner `lab-01-vererbung`:

```bash
mvn test
```

Der Test schlägt fehl (rotes Kreuz). Die Meldung lautet:

```text
expected: <Anna Huber> but was: <Huber Anna>
```

So liest du sie:

| Teil | Bedeutung |
|---|---|
| `expected: <Anna Huber>` | Das hast du im Test als richtig festgelegt. |
| `but was: <Huber Anna>` | Das hat dein Programm geliefert. |

Die Reihenfolge ist vertauscht. Der Fehler liegt in `Person.fullName()`, nicht im Test. Ein roter Test, der einen echten Fehler findet, ist ein **guter** Test.

## Schritt 5: Den Fehler beheben

Ändere in `Person.java` die Methode:

```java
    public String fullName() {
        return firstName + " " + lastName;
    }
```

Führe den Test erneut aus. Jetzt erscheint der grüne Haken. Im Terminal steht am Ende:

```text
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
BUILD SUCCESS
```

Dein Test sichert ab: Verändert später jemand `fullName()` und vertauscht die Namen wieder, schlägt der Test sofort an.

## Schritt 6: Weitere Fälle testen

Ein Test prüft **eine** Sache. Für weitere Situationen schreibst du weitere Tests in dieselbe Klasse.

**Fall: Der Name ändert sich.** Prüfe, dass `fullName()` nach `setName` den neuen Namen liefert:

```java
    @Test
    void fullNameNachSetNameLiefertNeuenNamen() {
        Person person = new Person("Huber", "Anna");

        person.setName("Moser", "Jakob");

        assertEquals("Jakob Moser", person.fullName());
    }
```

**Fall: Viele Namen mit demselben Test.** Statt drei fast gleiche Methoden zu schreiben, nimmst du einen parametrisierten Test. Er läuft einmal pro Zeile:

```java
    @ParameterizedTest
    @CsvSource({
            "Huber, Anna, Anna Huber",
            "Moser, Jakob, Jakob Moser",
            "Winkler-Hermaden, Magdalena, Magdalena Winkler-Hermaden"
    })
    void fullNameFuerMehrerePersonen(String nachname, String vorname, String erwartet) {
        assertEquals(erwartet, new Person(nachname, vorname).fullName());
    }
```

Dafür brauchst du zwei weitere Importe:

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;
```

Jede Zeile in `@CsvSource` ist ein Durchlauf. Kommas trennen die Werte. JUnit setzt sie der Reihe nach in die Parameter der Methode ein. Enthält ein Wert selbst ein Komma, setzt du ihn in einfache Anführungszeichen: `'12,5'`.

Nach dem Ausführen zeigt VS Code fünf grüne Tests: den ersten, den zweiten und drei Durchläufe des parametrisierten Tests.

## Die komplette Testklasse

So sieht `PersonTest.java` am Ende aus:

```java
package kurs.vererbung;

import static org.junit.jupiter.api.Assertions.assertEquals;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;

class PersonTest {

    @Test
    void fullNameLiefertVornameVorNachname() {
        Person person = new Person("Huber", "Anna");

        String ergebnis = person.fullName();

        assertEquals("Anna Huber", ergebnis);
    }

    @Test
    void fullNameNachSetNameLiefertNeuenNamen() {
        Person person = new Person("Huber", "Anna");

        person.setName("Moser", "Jakob");

        assertEquals("Jakob Moser", person.fullName());
    }

    @ParameterizedTest
    @CsvSource({
            "Huber, Anna, Anna Huber",
            "Moser, Jakob, Jakob Moser",
            "Winkler-Hermaden, Magdalena, Magdalena Winkler-Hermaden"
    })
    void fullNameFuerMehrerePersonen(String nachname, String vorname, String erwartet) {
        assertEquals(erwartet, new Person(nachname, vorname).fullName());
    }
}
```

## So gehst du bei den anderen Tests vor

Die übrigen Tests von Lab 01 baust du nach demselben Muster. Du brauchst nur die passende Prüfmethode:

| Du willst prüfen ... | Prüfmethode | Hinweis |
|---|---|---|
| einen Text oder eine ganze Zahl | `assertEquals(erwartet, tatsächlich)` | |
| eine Kommazahl (`double`) | `assertEquals(erwartet, tatsächlich, 0.001)` | Der dritte Wert ist die erlaubte Abweichung. `double`-Werte sind nie exakt, `0.1 + 0.2` ergibt `0.30000000000000004`. |
| dass eine Bedingung gilt | `assertTrue(bedingung)` oder `assertFalse(bedingung)` | |
| dass eine **Exception** auftritt | `assertThrows(Klasse.class, () -> aufruf)` | Der Aufruf steht in einem Lambda, sonst fängt dein Test den Fehler nicht ab. |
| dass ein Objekt überall passt, wo die Basisklasse erwartet wird | Variable vom Typ der Basisklasse | `Person p = new Employee(...)`, danach eine Methode von `Person` aufrufen und das Ergebnis prüfen |

Gehe bei jedem Fall so vor:

1. Schreibe den Satz auf, der gelten soll: „Ein negatives Alter löst eine `IllegalArgumentException` aus.“
2. Mache daraus den Methodennamen.
3. Schreibe Vorbereiten, Ausführen, Prüfen.
4. Führe den Test aus. Lass ihn zuerst fehlschlagen, wenn die Methode noch fehlt oder falsch ist. Ein Test, der nie rot war, hat vielleicht nie etwas geprüft.
5. Behebe den Fehler, bis der Test grün ist.

## Häufige Fehler

| Problem | Ursache und Lösung |
|---|---|
| Der Test erscheint in VS Code nicht | Die Datei liegt nicht in `src/test/java`, das Paket stimmt nicht mit dem Ordner überein, oder `@Test` fehlt. |
| `mvn test` meldet `No tests to run` | Der Klassenname endet nicht auf `Test`. |
| `cannot find symbol` bei `Person` | Die Testklasse liegt in einem anderen Paket. Setze `package kurs.vererbung;` oder importiere die Klasse. |
| `package org.junit.jupiter.api does not exist` | Die Abhängigkeit fehlt oder Maven hat das Projekt nicht geladen. Prüfe die `pom.xml` und klicke auf *Reload*. |
| `expected` und `but was` wirken vertauscht | Du hast `assertEquals(ergebnis, "Anna Huber")` geschrieben. Der erwartete Wert gehört nach vorn. |
| Der Test ist grün, obwohl das Programm falsch ist | Du hast den erwarteten Wert mit demselben Code berechnet. Schreibe ihn als festen Wert in den Test. |
| Ein Test schlägt nur manchmal fehl | Er hängt von etwas ab, das sich ändert, zum Beispiel von der Uhrzeit oder einer Zufallszahl. Mache die Abhängigkeit im Test fest. |

## Für die README

Füge in deine README einen Screenshot der Ansicht **Testing** mit grünen Haken ein. Trage in die Tabelle „Tests“ ein, was du erwartet hast und was herauskam. Zeige kurze Ausschnitte deiner Tests, nicht die ganze Klasse.
