# poker-tracker — „Ein Spitzhackenschlag“

Poker-Rangliste der Freundesrunde (Cash Game, 8 Spieler, Standard-Buy-in 10 €).
Eine statische Webseite auf GitHub Pages, ohne Anmeldung für alle mit dem Link erreichbar:

https://valentinmarketkings.github.io/poker-tracker/

## Wie es funktioniert

- **Seite:** `docs/index.html`, eine Datei mit Design, Logik und Animationen.
- **Daten:** `docs/data/results.json` (Spieler, Abende, `updatedAt`). Die Seite lädt die Datei beim Öffnen.
  Der JSON-Block in der HTML-Datei ist nur ein Offline-Fallback (Spieler ohne Abende).
- **Hosting:** GitHub Pages, deployt per Workflow `.github/workflows/pages.yml` bei jedem Push auf `main`
  (Inhalt von `docs/`). Ein Deploy dauert etwa eine Minute.

## Eintragen — zwei Wege

1. **Direkt in der Seite (Valentin):** Footer → „Eintragen einrichten“ → einmalig einen GitHub
   Fine-grained-Token mit *Contents: Read and write* nur für dieses Repository hinterlegen. Der Token
   bleibt im Browser (`localStorage`), nie im Repo. Danach erscheint der goldene Knopf; Speichern schreibt
   `results.json` per GitHub-API als Commit, der Workflow deployt, nach etwa einer Minute sehen es alle.
   Bis dahin zeigt der eigene Browser den frischen Stand aus einem lokalen Zwischenspeicher.
2. **Über Claude:** Ergebnisse nennen, Claude ändert `docs/data/results.json`, committet und pusht.

Leser ohne Token sehen nur die Ansicht. Ein Token, der in falsche Hände gerät, kann nur dieses eine
Repository ändern; im Zweifel bei GitHub widerrufen.

## Datenmodell

```json
{
  "version": 1, "currency": "EUR", "defaultBuyIn": 10, "groupName": "Ein Spitzhacken schlag",
  "players": [{"id": "valentin", "name": "Valentin", "color": "#C48800"}],
  "sessions": [{"id": "2026-09-12-erste", "date": "2026-09-12", "title": "",
                "entries": [{"player": "valentin", "buyIn": 10, "cashOut": 20}]}],
  "updatedAt": "2026-09-13T00:00:00Z"
}
```

Gewinn pro Spieler und Abend = `cashOut − buyIn`. Das Formular prüft die Nullsumme und lässt Abweichungen
nur mit ausdrücklichem Haken zu. Titel (Fisch, Sponsor, Bankomat, Stammgast) werden nur bei eindeutigem
Stand vergeben.

## Titel

Der Gruppenname steht in den Daten als „Ein Spitzhacken schlag“ (mit Leerzeichen, Valentins Wunsch): die
Seite setzt das letzte Wort immer auf eine eigene Zeile. Der Browser-Tab heißt „Ein Spitzhackenschlag“.

## Animationen

- **Intro** beim Öffnen (einmal pro Browser-Tab, Tipp überspringt): Spotlight, drei Karten fliegen ein und
  drehen sich um, der Titel klappt Buchstabe für Buchstabe auf, dazu Chip- und Geldregen im Canvas.
- **Dauerregen** aus Chips, Geldscheinen (10/20/50 €) und Münzen fällt **hinter** den Inhaltsflächen
  (Valentins Wunsch, Lesbarkeit); **Stürme** (Speichern eines Abends, „Make it rain“, Tipp auf den
  Chip-Stapel) sind dichter, bleiben aber ebenfalls hinten. Nur im Intro fällt der Regen vorne.
- Podiumskarten drehen sich vom Rücken auf die Vorderseite, Zahlen zählen hoch, Chart-Linien zeichnen
  sich, Medaillen drehen ein, LED-Laufband mit Fakten aus den Daten, Gold-Schimmer auf Rand und Titel.
- **Aus-Schalter** im Footer („Animationen aus“, pro Gerät), außerdem automatisch aus bei
  `prefers-reduced-motion`. Der Regen pausiert, wenn der Tab im Hintergrund ist.

## Easter Eggs („Keiner mag Hooles“)

Auf Valentins Wunsch an zwölf Stellen leicht versteckt, damit die Runde sie nach und nach findet:

1. Link-Vorschau in WhatsApp/iMessage (`og:description`) und Seitenbeschreibung.
2. Kommentar ganz oben im Quelltext.
3. Konsolen-Banner beim Laden.
4. Tab-Titel wird „Keiner mag Hooles 🐟“, sobald der Tab im Hintergrund ist.
5. Seltener „Hooles-Schein“ (0 €) im Geldregen, etwa jeder 22. Schein.
6. Rückseite der Podiumskarten (während des Umdrehens) und der Intro-Karten.
7. Extra-Kachel „0 · Leute, die Hooles mögen“ in Hooles' aufgeklappter Statistik.
8. Laufband-Varianten, wenn Hooles führt oder den letzten Abend gewonnen hat.
9. Sieger-Meldung beim Speichern, wenn Hooles gewinnt.
10. Fast unsichtbare Flüsterzeile unter dem Footer (bei Maus darüber lesbar).
11. Druckansicht zeigt die Flüsterzeile schwarz.
12. Geste: Hooles' Chip lange gedrückt halten, oder am Rechner „hooles“ tippen → Meldung, Chips wackeln, Sturm.

## Spielerfarben

Die acht Farben sind mit dem dataviz-Validator geprüft (Helligkeitsband, Chroma, Farbfehlsichtigkeit
benachbarter Paare, Kontrast ≥ 3:1). Die Reihenfolge ist Teil der Prüfung; neue Spieler werden hinten
angehängt und gegen den Vorgänger geprüft.

## Historie

Bis 2026-09-13 lief das Sheet als Claude-Artefakt (selbst-speichernde Seite). Verworfen, weil Leser dort
einen Claude-Account brauchen.
