# Lab 05: Designplanung, Bruchrechner

**Dauer:** 1 Einheit

## Lernziele

- Du gestaltest eine Anwendung nach anerkannten Gestaltungsrichtlinien.
- Du begründest deine Gestaltungsentscheidungen.
- Du dokumentierst Tastenkürzel und Bedienkonzept.

## Aufgabenstellung

Erstelle einen **Layoutentwurf ohne Funktion** für einen **Bruchrechner**. Der Entwurf ist die Grundlage für die Umsetzung in Lab 06. Du programmierst in diesem Lab nichts.

### Anforderungen an den Entwurf

- Alle wichtigen Funktionen sind **auf einen Blick** sichtbar.
- Die Zielgruppe sind **erfahrene Anwender**. Sie arbeiten schnell und bevorzugen Tastatur und Kürzel.
- Du berücksichtigst die Regeln des Schnittstellendesigns (siehe unten).
- Du legst die Tastenkürzel fest und erklärst sie in der Dokumentation.

Der Bruchrechner soll mindestens diese Funktionen bieten. Du darfst eigene ergänzen oder Funktionen begründet weglassen:

- Eingabe von zwei Brüchen (Zähler und Nenner),
- Grundrechenarten Addition, Subtraktion, Multiplikation, Division,
- Ergebnis als gekürzter Bruch, als gemischte Zahl und als Dezimalzahl,
- Verlauf der letzten Rechnungen,
- Eingaben löschen und Ergebnis weiterverwenden.

### Werkzeuge (nach Belieben)

Du wählst, womit du den Entwurf zeichnest:

- Figma oder Penpot (kostenlos im Browser),
- draw.io,
- Scene Builder (zeichnet JavaFX-Oberflächen per Drag and drop),
- eine saubere Handskizze als Foto,
- HTML und CSS oder ein Präsentationsprogramm.

Speichere das Bild als PNG im Ordner `docs/` und binde es relativ in deine README ein.

### Dokumentation

Fülle die `README.md` in diesem Ordner aus:

1. **Entwurf:** das Bild deiner Oberfläche.
2. **Begründung des Entwurfs:** Erkläre jeden Bestandteil der Oberfläche und warum er dort sitzt.
3. **Wahl der Benutzeroberfläche:** Nenne die Programmform (Desktop-App, Web-App im Browser, Konsole oder Terminal-UI) und begründe die Wahl mit Vor- und Nachteilen.
4. **Zielgruppe und Anwendungsbereiche:** Wer benutzt den Bruchrechner, wofür und in welchen Situationen?
5. **Tastenkürzel:** Tabelle aller Kürzel mit Erklärung.
6. **Bezug zu den Regeln:** Zeige für mindestens fünf Regeln, wo du sie in deinem Entwurf umgesetzt hast.

## Hintergrund: Die acht goldenen Regeln (Shneiderman)

| Nr. | Regel | Frage an deinen Entwurf |
|---|---|---|
| 1 | Strebe nach Konsistenz. | Heißen und sehen gleiche Dinge überall gleich aus? |
| 2 | Strebe nach universeller Bedienbarkeit. | Hilft die Oberfläche Anfängern und beschleunigt sie Profis (Kürzel)? |
| 3 | Biete informatives Feedback an. | Sieht der Anwender, was seine Aktion bewirkt hat? |
| 4 | Gestalte Dialoge so, dass sie zu einem Abschluss führen. | Weiß der Anwender, wann eine Rechnung fertig ist? |
| 5 | Verhindere Fehler. | Verhindert der Entwurf ungültige Eingaben wie den Nenner 0? |
| 6 | Erlaube die einfache Umkehrung von Aktionen. | Gibt es Rückgängig oder Löschen? |
| 7 | Gib dem Nutzer das Gefühl der Kontrolle. | Löst der Anwender jede Aktion selbst aus? |
| 8 | Reduziere die Belastung des Kurzzeitgedächtnisses. | Muss sich der Anwender nichts merken, was die Oberfläche anzeigen kann? |

## Hintergrund: Zehn Usability-Heuristiken (Nielsen)

| Nr. | Heuristik |
|---|---|
| 1 | Sichtbarkeit des Systemstatus |
| 2 | Übereinstimmung zwischen System und realer Welt |
| 3 | Nutzerkontrolle und Freiheit |
| 4 | Konsistenz und Standards |
| 5 | Fehlervermeidung |
| 6 | Wiedererkennen statt Erinnern |
| 7 | Flexibilität und Effizienz der Nutzung |
| 8 | Ästhetisches und minimalistisches Design |
| 9 | Hilfe beim Erkennen, Diagnostizieren und Beheben von Fehlern |
| 10 | Hilfe und Dokumentation |

## Checkliste

- [ ] Der Entwurf zeigt alle wichtigen Funktionen auf einen Blick.
- [ ] Die Begründung erklärt jeden Bestandteil.
- [ ] Die Wahl der Oberfläche ist mit Vor- und Nachteilen begründet.
- [ ] Die Tabelle der Tastenkürzel ist vollständig.
- [ ] Mindestens fünf Regeln sind am Entwurf gezeigt.
- [ ] Das Bild liegt in `docs/` und erscheint in der README-Vorschau.
- [ ] Der letzte Stand ist gepusht.
