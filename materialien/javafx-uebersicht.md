# JavaFX Übersicht

Dieses Blatt ersetzt die Übersicht der Swing-Klassen aus dem Vorjahr. Du schlägst hier Klassen nach, wenn du eine Oberfläche baust.

## Aufbau einer Anwendung

| Klasse | Rolle |
|---|---|
| `Application` | Basisklasse deiner Anwendung. Du überschreibst `start(Stage stage)`. |
| `Stage` | das Fenster |
| `Scene` | der Inhalt eines Fensters |
| `Parent`, `Pane` (Layout) | Container, die andere Elemente anordnen |
| `Node` | alles, was auf dem Bildschirm erscheint |

Starte die Anwendung über eine Klasse `Launcher`, die nicht von `Application` erbt (siehe [Lab 03](../lab-03-erste-gui/AUFGABE.md)).

## Von Swing zu JavaFX

| Swing | JavaFX |
|---|---|
| `JFrame` | `Stage` und `Scene` |
| `JPanel` | `Pane`, `VBox`, `HBox`, `GridPane` |
| `JButton` | `Button` |
| `JLabel` | `Label` |
| `JTextField` | `TextField` |
| `JTextArea` | `TextArea` |
| `JComboBox` | `ComboBox` |
| `JList` | `ListView` |
| `JTable` | `TableView` |
| `JMenuBar`, `JMenu`, `JMenuItem` | `MenuBar`, `Menu`, `MenuItem` |
| `JCheckBoxMenuItem` | `CheckMenuItem` |
| `JRadioButtonMenuItem` | `RadioMenuItem` |
| `JOptionPane` | `Alert` |
| `JFileChooser` | `FileChooser` |
| `ImageIcon` in `JLabel` | `Image` und `ImageView` |
| `ActionListener` | `EventHandler`, meist als Lambda in `setOnAction` |
| `BorderLayout` | `BorderPane` |
| `GridBagLayout`, `GridLayout` | `GridPane` |
| `BoxLayout` | `VBox`, `HBox` |

## Layout-Container

| Container | Ordnet an | Typische Verwendung |
|---|---|---|
| `VBox` | untereinander | Formulare, Spalten |
| `HBox` | nebeneinander | Eingabezeile, Button-Leisten |
| `BorderPane` | oben, links, Mitte, rechts, unten | Hauptgerüst eines Fensters |
| `GridPane` | Zeilen und Spalten | Formulare, Rechner-Tasten |
| `StackPane` | übereinander | Hintergründe, zentrierter Inhalt |
| `FlowPane` | fließend mit Umbruch | Sammlungen kleiner Elemente |
| `ScrollPane` | scrollbarer Bereich | lange Inhalte |
| `SplitPane` | verschiebbare Teilung | Liste und Detailansicht |

Wichtige Einstellungen: `setPadding(new Insets(12))` (Innenabstand), `setSpacing(8)` bei `VBox` und `HBox`, `setHgap(8)` und `setVgap(8)` bei `GridPane`, `HBox.setHgrow(element, Priority.ALWAYS)` (Element nimmt freien Platz).

## Steuerelemente

| Element | Verwendung | Wichtige Methoden |
|---|---|---|
| `Label` | Text anzeigen | `setText`, `setWrapText` |
| `Button` | Aktion auslösen | `setOnAction`, `setDisable`, `fire` |
| `TextField` | einzeilige Eingabe | `getText`, `setText`, `clear`, `setPromptText` |
| `TextArea` | mehrzeiliger Text | `appendText`, `setEditable(false)` |
| `ComboBox<T>` | Auswahl aus Liste | `getItems`, `getValue`, `setValue` |
| `ListView<T>` | Liste | `getSelectionModel().getSelectedItem()` |
| `TableView<T>` | Tabelle | `getColumns`, `setItems` |
| `CheckBox`, `RadioButton` | Optionen | `isSelected` |
| `DatePicker` | Datumsauswahl | `getValue()` liefert ein `LocalDate` |
| `ImageView` | Bild | `setImage`, `setFitWidth`, `setPreserveRatio` |

## Events

Ein Event-Handler ist ein Lambda, das du an ein Element hängst:

```java
button.setOnAction(e -> statusZeile.setText("Gedrückt"));
eingabe.setOnAction(e -> button.fire());                 // Enter im Textfeld
liste.setOnKeyPressed(e -> {
    if (e.getCode() == KeyCode.DELETE) {
        aufgaben.remove(liste.getSelectionModel().getSelectedItem());
    }
});
```

Auf die Eingabe beschränken, zum Beispiel auf einen einzelnen Buchstaben:

```java
eingabe.setTextFormatter(new TextFormatter<String>(aenderung ->
        aenderung.getControlNewText().matches("[A-Za-z]?") ? aenderung : null));
```

Der `TextFormatter` lehnt jede Änderung ab, für die die Funktion `null` liefert.

## Listen und Tabellen

Eine `ListView` oder `TableView` zeigt eine `ObservableList`. Ändert sich die Liste, ändert sich die Anzeige von selbst.

```java
ObservableList<String> aufgaben = FXCollections.observableArrayList();
ListView<String> liste = new ListView<>(aufgaben);
aufgaben.add("Neue Aufgabe");   // erscheint sofort in der Liste
```

Eine Tabelle braucht für jede Spalte eine Funktion, die den Wert einer Zeile liefert:

```java
record Buchung(String text, long cent) { }

TableView<Buchung> tabelle = new TableView<>();
TableColumn<Buchung, String> spalteText = new TableColumn<>("Beschreibung");
spalteText.setCellValueFactory(zelle -> new ReadOnlyStringWrapper(zelle.getValue().text()));
tabelle.getColumns().add(spalteText);
tabelle.setItems(FXCollections.observableArrayList(new Buchung("Miete", 65000)));
```

## Formulare mit GridPane

```java
GridPane grid = new GridPane();
grid.setHgap(8);
grid.setVgap(8);
grid.add(new Label("Buchstabe"), 0, 0);       // Spalte 0, Zeile 0
grid.add(eingabe, 1, 0);                      // Spalte 1, Zeile 0
DatePicker von = new DatePicker(LocalDate.now().withDayOfMonth(1));
grid.add(new Label("Von"), 0, 1);
grid.add(von, 1, 1);
```

## Menüleiste

```java
MenuBar menueLeiste = new MenuBar();

Menu datei = new Menu("Datei");
MenuItem beenden = new MenuItem("Beenden");
beenden.setAccelerator(KeyCombination.keyCombination("Ctrl+Q"));
beenden.setOnAction(e -> Platform.exit());
datei.getItems().add(beenden);

CheckMenuItem verlauf = new CheckMenuItem("Verlauf anzeigen");
verlauf.setSelected(true);
verlauf.selectedProperty().addListener((beobachtet, alt, neu) -> verlaufBox.setVisible(neu));

ToggleGroup versuche = new ToggleGroup();              // nur ein Eintrag gleichzeitig
RadioMenuItem fuenf = new RadioMenuItem("5 Versuche");
fuenf.setToggleGroup(versuche);
RadioMenuItem neun = new RadioMenuItem("9 Versuche");
neun.setToggleGroup(versuche);
neun.setSelected(true);

Menu einstellungen = new Menu("Einstellungen");
einstellungen.getItems().addAll(verlauf, fuenf, neun);
menueLeiste.getMenus().addAll(datei, einstellungen);
```

Setze die Menüleiste mit `wurzel.setTop(menueLeiste)` in ein `BorderPane`.

## Dialoge

```java
Alert alert = new Alert(Alert.AlertType.CONFIRMATION, "Wirklich alles löschen?",
        ButtonType.YES, ButtonType.NO);
alert.showAndWait()
        .filter(knopf -> knopf == ButtonType.YES)
        .ifPresent(knopf -> aufgaben.clear());
```

| `AlertType` | Verwendung |
|---|---|
| `INFORMATION` | Hinweis |
| `WARNING` | Eingabefehler, Warnung |
| `ERROR` | Fehler, der die Aktion verhindert |
| `CONFIRMATION` | Rückfrage mit Ja und Nein |

Dateien auswählen:

```java
FileChooser dialog = new FileChooser();
dialog.setTitle("Wortliste öffnen");
dialog.getExtensionFilters().add(new FileChooser.ExtensionFilter("Textdateien", "*.txt"));
File auswahl = dialog.showOpenDialog(stage);     // null, wenn der Benutzer abbricht
if (auswahl != null) {
    Path pfad = auswahl.toPath();
}
```

## Bilder

Lege Bilder in `src/main/resources` und lade sie über den Klassenpfad. Der Pfad beginnt mit `/`:

```java
Image bild = new Image(Objects.requireNonNull(
        HangmanApp.class.getResourceAsStream("/hangman/hangman_0.png")));
ImageView ansicht = new ImageView(bild);
ansicht.setFitWidth(240);
ansicht.setPreserveRatio(true);
```

## Gestaltung mit CSS

```java
scene.getStylesheets().add(App.class.getResource("style.css").toExternalForm());
button.getStyleClass().add("primary");
```

```css
.button.primary {
    -fx-background-color: #2563eb;
    -fx-text-fill: white;
}
```

Die Eigenschaften beginnen mit `-fx-`. Die Referenz steht im *JavaFX CSS Reference Guide* von OpenJFX.

## Oberflächen mit FXML

JavaFX kann Oberflächen auch in XML-Dateien (FXML) beschreiben, die du mit dem Programm *Scene Builder* per Drag and drop zeichnest. In diesem Kurs baust du die Oberflächen im Code, damit du siehst, was passiert. FXML ist ein sinnvoller nächster Schritt, wenn du den Aufbau verstanden hast.

## Eine Regel für alle Oberflächen

Die Oberfläche stellt dar und nimmt Eingaben an. Regeln und Berechnungen gehören in eigene Klassen ohne JavaFX. So lassen sie sich testen und wiederverwenden.
