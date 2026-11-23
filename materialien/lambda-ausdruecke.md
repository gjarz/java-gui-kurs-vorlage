# Lambda-Ausdrücke

Ein Lambda-Ausdruck ist eine kurze Schreibweise für ein Stück Code, das du wie einen Wert weitergibst. Du brauchst ihn überall, wo du Verhalten übergibst: bei Event-Handlern in JavaFX, beim Sortieren, bei Filtern und bei der Zuordnung von Rechenoperationen.

## Aufbau

```text
(Parameter) -> Ausdruck
(Parameter) -> { Anweisungen; }
```

| Form | Beispiel |
|---|---|
| ohne Parameter | `() -> System.out.println("Hallo")` |
| ein Parameter | `text -> text.length()` |
| mehrere Parameter | `(a, b) -> a + b` |
| mehrere Anweisungen | `e -> { eingabe.clear(); eingabe.requestFocus(); }` |

Die Parametertypen leitet der Compiler selbst ab.

## Funktionale Interfaces

Ein Lambda passt zu einem Interface mit **genau einer** abstrakten Methode. Java bringt die wichtigsten mit (Paket `java.util.function`):

| Interface | Methode | Bedeutung | Beispiel |
|---|---|---|---|
| `Runnable` | `run()` | führt etwas aus | `() -> System.out.println("Hallo")` |
| `Supplier<T>` | `get()` | liefert einen Wert | `() -> 4.0` |
| `Consumer<T>` | `accept(t)` | verbraucht einen Wert | `name -> System.out.println(name)` |
| `Function<T, R>` | `apply(t)` | wandelt `T` in `R` um | `text -> text.length()` |
| `Predicate<T>` | `test(t)` | liefert `true` oder `false` | `text -> text.isBlank()` |
| `BinaryOperator<T>` | `apply(a, b)` | verknüpft zwei Werte desselben Typs | `(a, b) -> a + b` |
| `DoubleBinaryOperator` | `applyAsDouble(a, b)` | verknüpft zwei `double` | `(a, b) -> a * b` |

In JavaFX ist `EventHandler<ActionEvent>` ein funktionales Interface. Deshalb schreibst du `button.setOnAction(e -> ...)`.

## Beispiele

```java
Runnable gruss = () -> System.out.println("Hallo");
gruss.run();

Function<String, Integer> laenge = text -> text.length();
Predicate<String> istLeer = String::isBlank;
BinaryOperator<Integer> plus = (a, b) -> a + b;
Supplier<Double> zufall = () -> 4.0;
System.out.println(laenge.apply("Java") + " " + istLeer.test(" ") + " " + plus.apply(2, 3));
```

### Mit Listen

```java
List<String> namen = new ArrayList<>(List.of("Mira", "Jakob", "Eva", "Luca"));
namen.sort(Comparator.comparing(String::length).thenComparing(Comparator.naturalOrder()));
namen.removeIf(n -> n.startsWith("J"));
namen.forEach(System.out::println);
```

### Rechenoperationen zuordnen

Statt einer langen `if`-Kette oder eines `switch` hinterlegst du jede Rechenart in einer `Map`:

```java
Map<String, DoubleBinaryOperator> rechenarten = Map.of(
        "+", (a, b) -> a + b,
        "-", (a, b) -> a - b,
        "*", (a, b) -> a * b,
        "/", (a, b) -> a / b);

double ergebnis = rechenarten.get("*").applyAsDouble(6, 7);   // 42.0
```

Eine neue Rechenart ist eine neue Zeile in der Map. Die Auswahl über das Symbol der Schaltfläche ist ein einziger Aufruf.

### Streams

```java
List<String> lang = namen.stream()
        .filter(n -> n.length() > 3)
        .map(String::toUpperCase)
        .toList();
```

## Methodenreferenzen

Tut ein Lambda nichts anderes, als eine Methode aufzurufen, schreibst du sie kürzer mit `::`:

| Lambda | Methodenreferenz |
|---|---|
| `text -> text.isBlank()` | `String::isBlank` |
| `n -> System.out.println(n)` | `System.out::println` |
| `(a, b) -> a.plus(b)` | `Bruch::plus` |

## Auf Variablen zugreifen

Ein Lambda darf Variablen aus seiner Umgebung lesen, aber nur, wenn sie sich nach der Zuweisung nicht mehr ändern (*effectively final*).

```java
int zaehler = 0;
button.setOnAction(e -> zaehler++);     // Fehler: zaehler ändert sich
```

Verwende stattdessen eine **Eigenschaft der Klasse** (Feld), ein `AtomicInteger` oder ein Objekt, dessen Inhalt sich ändert, wie eine `ObservableList`.
