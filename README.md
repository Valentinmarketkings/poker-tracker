# poker-tracker — „Die Runde"

Poker-Rangliste der Freundesrunde (Cash Game, 9 Spieler, Standard-Buy-in 10 €).
Eine einzelne HTML-Seite, veröffentlicht als Claude-Artefakt:
https://claude.ai/code/artifact/d1162e97-072f-42a1-b4fb-e665bec6b0b1
 Valentin trägt nach jedem Abend
ein, die Freunde öffnen den Link am Handy.

## Wie es funktioniert

- **Eine Datei:** `src/index.html`. Design, Logik und Daten stecken in derselben Seite.
- **Daten liegen in der Seite selbst**, im Block `<script type="application/json" id="poker-data">`.
  Rendern heißt: JSON lesen, DOM bauen. Leser brauchen keinen Datenbankzugriff.
- **Speichern** (nur der Besitzer des Artefakts): Die Seite holt ihren eigenen Quelltext per
  `fetch`, tauscht den Datenblock aus und veröffentlicht die Seite per `claude.use("artifact")`
  als neue Version. Jede Speicherung ist eine Artefakt-Version, die Historie ist also eingebaut.
- **Modi**, die die Seite selbst erkennt:
  - *writer*: im Artefakt, Besitzer → Eingabe-Button sichtbar.
  - *reader*: im Artefakt, kein Schreibrecht → nur Ansicht, Hinweis im Footer.
  - *local*: die Datei ohne Artefakt-Shell geöffnet → speichert in `localStorage` (zum Testen).
  - *demo*: Beispieldaten, nichts wird gespeichert.

## Datenmodell

```json
{
  "version": 1, "currency": "EUR", "defaultBuyIn": 10, "groupName": "Die Runde",
  "players": [{"id": "valentin", "name": "Valentin", "color": "#C48800"}],
  "sessions": [{"id": "2026-09-19-ab12c", "date": "2026-09-19", "title": "bei Benny",
                "entries": [{"player": "valentin", "buyIn": 10, "cashOut": 23.5}]}]
}
```

Gewinn pro Spieler und Abend = `cashOut − buyIn`. Das Formular prüft, dass Cash-outs und Buy-ins
sich decken (Nullsumme) und lässt Abweichungen nur mit ausdrücklichem Haken zu.

## Bedienung

- **Abend eintragen:** goldener Button unten rechts → Datum, Spieler antippen, Buy-in (Rebuy per
  „+10"), Cash-out → Speichern. Die Seite lädt neu, Konfetti für den Sieger.
- **Bearbeiten / Löschen:** unter jedem Abend im Abschnitt „Abende".
- **Verlauf:** Standard zeigt die Top 3 farbig, alle anderen gedimmt. Namen in der Legende antippen,
  um Linien ein- oder auszublenden. Finger/Maus über dem Chart zeigt alle Bilanzen an dem Abend.
- **Export / Import:** im Footer. Export ist das JSON aus dem Datenblock, Import ersetzt alles.

## Backup

`data/backup.json` ist ein manueller Snapshot. Aktualisieren: Artefakt per `Artifact read` holen,
den Inhalt des Blocks `#poker-data` extrahieren und hier ablegen. Alternativ „Daten exportieren"
in der Seite und den Text hier einfügen.

## Fallback, falls der Artefakt-Link für Freunde nicht funktioniert

Dieselbe Seite läuft ohne Änderung als statische Seite (z. B. GitHub Pages mit `src/` als Root).
Dann gibt es keinen Schreibpfad im Browser; Eintragen läuft über eine Claude-Session, die den
Datenblock in `src/index.html` ändert und pusht, oder über „Daten importieren" im lokalen Modus
plus Commit.

## Spielerfarben

Die neun Farben sind gegen den Filz-Hintergrund `#0F3D2E` mit dem dataviz-Validator geprüft
(Helligkeitsband, Chroma, Farbfehlsichtigkeit benachbarter Paare, Kontrast ≥ 3:1). Reihenfolge
ist Teil der Prüfung, nicht kosmetisch.
