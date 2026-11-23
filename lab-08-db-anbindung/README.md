# Lab 08: Datenbankanbindung

| | |
|---|---|
| Name | |
| Klasse | |
| Datum | |

## 1. Datenbankserver

Gewählte Variante (Docker, lokal, Raspberry Pi):

Version des Servers:

Beschreibe in Stichworten, wie du den Server eingerichtet hast. Zeige keine Passwörter.

![Server läuft](docs/server.png)

## 2. Umwandlung des Oracle-Skripts

| Oracle | MariaDB | Meine Lösung |
|---|---|---|
| `NUMBER` | | |
| `VARCHAR2` | | |
| `to_date(...)` | | |
| `SET TERMOUT` | | |
| `DROP TABLE` | | |
| `COMMIT` | | |

Wie hast du die Datumswerte für `SCOTT` und `ADAMS` gelöst?

Kontrolle der Zeilenzahlen:

| Tabelle | Erwartet | Gefunden |
|---|---|---|
| `emp` | 14 | |
| `dept` | 4 | |
| `salgrade` | 5 | |

## 3. Einbindung in VS Code

Beschreibe die Schritte in Stichworten.

![Verbindung](docs/vscode-verbindung.png)

![Abfrage mit JOIN](docs/vscode-abfrage.png)

## 4. Zugriff mit Java

Beschreibe in Stichworten, wie dein Programm aufgebaut ist: Konfiguration, Verbindung, Abfrage, Ausgabe.

![Ausgabe von EmployeeLeser](docs/employeeleser.png)

### Fehlerfälle

| Fall | Reaktion des Programms |
|---|---|
| falsches Passwort | |
| Server nicht erreichbar | |
| `db.properties` fehlt | |

## 5. Zugangsdaten

Wie schützt du die Zugangsdaten? Zeige die Ausgabe von `git status`, die belegt, dass `db.properties` nicht erfasst wird.

## 6. Reflexion

1. Warum `PreparedStatement` statt Textverkettung?
2. Wofür ist `try-with-resources` gut?
3. Warum gehören Zugangsdaten nicht in das Repository?

## 7. Probleme und Lösungen

| Problem | Lösung |
|---|---|
| | |

## 8. Quellen und Hilfsmittel

Nenne Webseiten, Mitschüler und KI-Werkzeuge, die du genutzt hast, und wofür.
