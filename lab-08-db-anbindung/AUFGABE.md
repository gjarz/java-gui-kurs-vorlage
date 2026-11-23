# Lab 08: Datenbankanbindung

**Dauer:** 1 Einheit

## Lernziele

- Du betreibst einen Datenbankserver (MariaDB) auf deinem Rechner, im Container oder auf dem Raspberry Pi.
- Du bindest die Datenbank in VS Code ein und fragst sie ab.
- Du wandelst ein Oracle-Skript in Standard-SQL für MariaDB um.
- Du liest Datensätze mit JDBC aus Java aus.
- Du hältst Zugangsdaten aus dem Repository heraus.

Dieses Lab ist ein technischer Test. Du baust **keine fertige Anwendung**, sondern bereitest die Datenbankanbindung für Lab 09 vor.

## Aufgabenstellung

### Teil 1: Datenbankserver einrichten

Wähle **eine** Variante:

| Variante | Vorgehen |
|---|---|
| A: Docker | Kopiere `.env.example` nach `.env`, trage Passwörter ein und starte mit `docker compose up -d`. |
| B: lokal | Installiere MariaDB (Windows: Installer, Linux und Raspberry Pi: `sudo apt install mariadb-server`). |
| C: Raspberry Pi | Wie B, auf dem Pi. Erlaube den Zugriff von deinem Rechner (`bind-address`) und beachte die Firewall. |

Bei Variante B und C legst du Datenbank und Benutzer selbst an. **Arbeite in der Anwendung nie mit dem Benutzer `root`.**

```sql
CREATE DATABASE scott CHARACTER SET utf8mb4;
CREATE USER 'schueler'@'%' IDENTIFIED BY '<dein-passwort>';
GRANT ALL PRIVILEGES ON scott.* TO 'schueler'@'%';
FLUSH PRIVILEGES;
```

### Teil 2: Das Oracle-Skript umwandeln

Im Ordner `sql/` liegt der Export `scott_oracle.sql` aus einer Oracle-Datenbank. Wandle ihn in Standard-SQL für MariaDB um und speichere ihn als `sql/scott_mariadb.sql`. Das Skript muss sich beliebig oft ausführen lassen.

Diese Unterschiede musst du beachten:

| Oracle | MariaDB |
|---|---|
| `NUMBER(n)`, `NUMBER(p,s)` | passender Ganzzahl- oder `DECIMAL(p,s)`-Typ |
| `VARCHAR2` | `VARCHAR` |
| `to_date('17-12-1980','dd-mm-yyyy')` | Datumsliteral `'1980-12-17'` |
| `SET TERMOUT OFF`, `SET ECHO OFF` | Befehle von SQL*Plus, gibt es nicht |
| `DROP TABLE x;` | schlägt fehl, wenn die Tabelle fehlt. Verwende `IF EXISTS`. |
| `COMMIT;` | wird nicht gebraucht, MariaDB schreibt standardmäßig sofort |

Zwei Zeilen sind knifflig: Für `SCOTT` und `ADAMS` rechnet Oracle mit `to_date('13-JUL-87') - 85` beziehungsweise `- 51`. Löse das mit einem fertigen Datum oder mit `DATE_SUB('1987-07-13', INTERVAL 85 DAY)`. Prüfe, in welchem Jahrhundert `87` liegt.

Beachte die Reihenfolge beim Löschen und Anlegen der Tabellen: `emp` verweist per Fremdschlüssel auf `dept`.

**Kontrolle:** Danach hat `emp` 14 Zeilen, `dept` 4 Zeilen und `salgrade` 5 Zeilen.

### Teil 3: Datenbank in VS Code einbinden

1. Installiere die Erweiterungen **SQLTools** und **SQLTools MySQL/MariaDB/TiDB**.
2. Lege eine Verbindung zu `localhost:3306`, Datenbank `scott`, an.
3. Führe `sql/scott_mariadb.sql` aus.
4. Frage `SELECT * FROM emp;` und eine Abfrage mit `JOIN` auf `dept` ab.

Dokumentiere den Vorgang mit Screenshots (ohne sichtbare Passwörter).

### Teil 4: Zugriff mit Java (JDBC)

1. Kopiere `src/main/resources/db.properties.example` nach `db.properties` und trage deine Werte ein.
2. Schreibe im Paket `kurs.db` die Klasse `EmployeeLeser` mit einer `main`-Methode, die **alle Datensätze der Tabelle `emp` sequenziell** ausliest und tabellarisch auf der Konsole ausgibt.
3. Die Verbindung kommt aus `db.properties`. Die Zugangsdaten stehen **nie** im Quelltext.
4. Verwende `try-with-resources` und `PreparedStatement`. Ein Beispiel für das Grundgerüst findest du in [JDBC und SQL](../materialien/jdbc-und-sql.md).
5. Fehler (falsches Passwort, Server nicht erreichbar) führen zu einer verständlichen Meldung, nicht zu einem Absturz mit Stacktrace.

Erweitere dein Programm:

- Die Ausgabe nennt statt der Abteilungsnummer den Abteilungsnamen (`JOIN` auf `dept`).
- Die Spaltenüberschriften liest du dynamisch aus `ResultSetMetaData`.
- Das Programm nimmt eine Abteilungsnummer als Parameter und gibt nur deren Mitarbeiter aus.

### Erweiterung (freiwillig)

Zeige das Abfrageergebnis in einer JavaFX-`TableView` an, deren Spalten du dynamisch aus `ResultSetMetaData` erzeugst. Die Startklasse trägt den Namen `kurs.db.Launcher`.

## Reflexionsfragen für deine README

1. Warum verwendest du `PreparedStatement` und keine Textverkettung für SQL?
2. Wofür ist `try-with-resources` gut?
3. Warum gehören Zugangsdaten nicht in das Repository, und wie verhinderst du das?

## Checkliste

- [ ] Der Datenbankserver läuft, du arbeitest mit einem eigenen Benutzer.
- [ ] `sql/scott_mariadb.sql` läuft fehlerfrei und mehrfach hintereinander.
- [ ] `emp` hat 14, `dept` hat 4 und `salgrade` hat 5 Zeilen.
- [ ] `EmployeeLeser` gibt alle Mitarbeiter aus.
- [ ] `db.properties` liegt **nicht** im Repository (`git status` prüfen).
- [ ] Die README dokumentiert Einrichtung, Umwandlung und Anbindung mit Screenshots.
- [ ] Der letzte Stand ist gepusht.
