# app/

Dateien, die die GradeMyCards-App zur Laufzeit von hier lädt.

## video_tutorials.json

Katalog der YouTube-Tutorials. Die App holt die Datei beim Start über

    https://raw.githubusercontent.com/aglaeser77-code/grademycards-website/main/app/video_tutorials.json

und legt sie intern ab (`VideoCatalog` im App-Repo). Ob ein Download nötig ist,
entscheidet der HTTP-ETag von raw.githubusercontent.com — der ändert sich mit
jedem Commit an dieser Datei. Ein Commit hier reicht also, damit die App beim
nächsten Start aktualisiert; ein App-Update ist nicht nötig.

### Aufbau

    schema   Formatversion. Die App akzeptiert nur schema == 1.
    groups[] Überschriften im Video-Tab, in der Reihenfolge von "order".
    videos[] Die Videos selbst.

Ein Video-Eintrag:

    id      stabiler Schlüssel, nur intern (kein Anzeigetext)
    group   id aus groups[]; unbekannt/fehlend -> Video steht ohne Überschrift oben
    phases  in welchen Fragmenten das Video in der Hilfe erscheint:
            1 Card Definition, 2 Card Analysis, 3 Guideline Editor,
            4 Reports, 5 Corners, 6 Edges, 7 Surface
            Mehrfachnennung ist erlaubt (z. B. [1, 2]).
    order   Sortierung innerhalb der Gruppe
    url     Volle YouTube-URL (Shorts gehen auch). LEER = Eintrag wird von
            der App ignoriert; so stehen geplante Videos schon hier drin.
    title   Titel je Sprachcode. Fehlt die Sprache, nimmt die App "en".
            OHNE Serien-Prefix ("How to - ", "Getting Started - "): das
            steht schon in der Gruppenueberschrift darueber, und die ist
            uebersetzt. Auf YouTube bleibt das Prefix natuerlich im Titel.

### Ein Video ergänzen

1. Eintrag in `videos[]` anlegen, `url` setzen, `phases` passend wählen.
2. `title` mindestens für "en" füllen — fehlende Sprachen fallen auf "en" zurück.
3. Committen und pushen. Fertig.

Sprachen wie in der App: de, en, es, fr, it, ja, nl, pt.
