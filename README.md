# Gebhardt Maschinenbau – Karriereseite

Recruiting-Landingpage der **Gebhardt Maschinenbau GmbH**, Egelsbach.
Ausgeschriebene Stellen: **Dreher / Zerspanungsmechaniker** und **Rundschleifer**
(m/w/d) – und ausschließlich diese beiden. Aufbau 1:1 nach der ALWA-Karriereseite.

## Inhalt

- `index.html` – die komplette Seite (self-contained, kein Build-Schritt nötig)
- `impressum.html`, `datenschutz.html`, `legal.css` – Rechtstexte (es gibt noch keine Firmenwebsite)
- `bilder/` – Logo und später das Bildmaterial (siehe `bilder/HIER-BILDER-ABLEGEN.txt`)
- `fonts/` – selbst gehostete Schriften (Exo 2, Archivo; SIL Open Font License)
- `.nojekyll` – GitHub Pages liefert die Dateien 1:1 aus

## Live schalten (GitHub Pages)

1. Repo-Settings → **Pages** → Source: **Deploy from a branch**, Branch: `main` / `/root`
2. Nach ein paar Minuten unter `https://saviold.github.io/Gebhardt-Maschinenbau-GmbH/` erreichbar

## CI

Es gibt noch keine Website – das CI ist deshalb **direkt aus dem Original-Logo** abgeleitet
und mit dem gewünschten Blaugrau kombiniert. Alles steht im `:root`-Block oben in `index.html`.

| Token | Wert | Herkunft |
|-------|------|----------|
| `--brand` | `#00aeef` | Cyan aus dem Logo (Schriftzug, Linie) |
| `--brand-light` | `#a9dff7` | Hellblau aus dem Logo (großes „G“) |
| `--brand-900` | `#1e2f3d` | Blaugrau (Überschriften, Hero, Footer) |
| `--on-brand` | `#0d2130` | Text auf Cyan (Kontrast) |
| `--f-display` | Exo 2 | technische, kantige Headline – nah am Logo-Schriftzug |
| `--f-body` | Archivo | sachliche Grotesk – nah an „MASCHINENBAU GMBH“ |

Die Schraffur aus dem Logo (`\\\\\\`) taucht als Gestaltungselement auf (Hero-Unterkante,
Stellenkarten, Kicker). Das Logo wird **unverändert** eingesetzt – auf dunklem Grund auf
einer weißen Platte, weil es nur mit weißem Hintergrund vorliegt.

Schriften liegen lokal in `fonts/` – es wird **keine** Verbindung zu Google Fonts aufgebaut.

## Stellen pflegen

Beide Stellen stehen in **einer** Config (`var JOBS` im `<script>` am Seitenende). Sie speist:
Stellenkarten, Stellenauswahl im Formular, **die Screening-Fragen**, Hero-Text bei `?stelle=…`,
FAQ „Voraussetzungen“ und die JobPosting-Structured-Data.

Der Kartenblock **„Das setzen wir voraus“** wird aus den Pflichtfragen erzeugt, **„Schön,
aber kein Muss“** aus den optionalen – die Anzeige verlangt also exakt das, was das Formular prüft.

## Stellen-Deeplinks für die Anzeige

Die Seite textet dann den Hero auf die Stelle, wählt sie vor und **überspringt den
Auswahl-Schritt** (aus 6 Schritten werden 5):

- `…/?stelle=dreher` (auch `zerspanungsmechaniker`, `zerspaner`)
- `…/?stelle=rundschleifer` (auch `schleifer`)

## Vorfilterung (Screening)

Je Stelle **4 Fragen, eine pro Schritt**, davor die Stellenauswahl (entfällt beim Deeplink),
danach die Kontaktdaten. Kein Lebenslauf-Upload, keine Dateianhänge. Nur berufsbezogene
Fragen (AGG).

**Dreher / Zerspanungsmechaniker**

| # | Frage | Kategorie | Erfüllt bei |
|---|-------|-----------|-------------|
| 1 | Erfahrung im Drehen (konventionell, Einzelteile & Kleinserien) | **Pflicht** | mehrjährig oder erste Erfahrungen – *nicht*: nur CNC, keine |
| 2 | Umgang mit Lünetten (Teile 0,5–2 m) | **Pflicht** | regelmäßig oder einige Male |
| 3 | Messen mit Mikrometer | **Pflicht** | sicher oder schon öfter |
| 4 | Zyklenprogrammierung | optional | gute Kenntnisse oder Grundkenntnisse |

**Rundschleifer**

| # | Frage | Kategorie | Erfüllt bei |
|---|-------|-----------|-------------|
| 1 | Erfahrung im Rundschleifen zwischen Spitzen (Stahl, VA, verchromt) | **Pflicht** | mehrjährig oder erste Erfahrungen |
| 2 | Messen mit Mikrometer **und** Rachenlehre | **Pflicht** | beides sicher |
| 3 | Richten von Wellen | optional | sicher oder ein paar Mal |
| 4 | Nachzentrieren von Teilen | optional | sicher oder ein paar Mal |

Ergänzt (vom Kunden nicht explizit genannt, stellentypisch): Rundschleifer Frage 1
(Berufserfahrung im Rundschleifen). Beim Dreher ist „nur CNC“ ein K.-o., weil die Stelle
ausdrücklich konventionelles Drehen ist – soll das durchgehen, an der Option `ok:1` setzen.

**Logik**

- **Pflichtfrage nicht erfüllt** → Bewerbung endet sofort mit freundlichem Hinweis, keine
  weiteren Schritte, **kein Lead an die Lead Table**, keine anderen Stellen.
- **Optionale Frage nicht erfüllt** → Bewerber kommt normal weiter; der Wert wird mit
  `nicht erfüllt – …` übertragen, das Feld `nicht_erfuellt` nennt die betroffenen Kriterien.

## Mobile Laufruhe

Beim Schrittwechsel ändern sich ausschließlich Klassen und Texte – **kein** `window.scrollTo`,
**kein** `scrollIntoView`, **kein** `focus()`, kein Reload, kein Hash-Sprung. Der Formular-Inhalt
hat eine feste Mindesthöhe (`--form-min` 502 px / `--form-min-mobile` 488 px, gemessen am
höchsten Schritt), die auch mobil gilt – Fortschrittsbalken und Weiter-Button stehen fest.

Geprüft im Browser (Chromium, Touch-Emulation, echte Finger-Taps an Koordinaten), je Viewport
alle vier Wege (ohne Deeplink → Dreher / → Rundschleifer, beide Deeplinks), von der ersten
Frage bis zur Bestätigung:

| Viewport | scrollY | Kartenhöhe | Weiter-Button | Fortschritt | ganzer Schritt sichtbar |
|----------|---------|------------|---------------|-------------|-------------------------|
| 360 × 640 | konstant | 546 px | fix | fix | ja |
| 375 × 667 | konstant | 546 px | fix | fix | ja |
| 390 × 844 | konstant | 546 px | fix | fix | ja |
| 414 × 896 | konstant | 546 px | fix | fix | ja |

## Lead Table

Jede Stelle hat ihre eigene Kachel (URL beim jeweiligen `JOBS`-Eintrag unter `webhook`):

| Stelle | `tableID` |
|--------|-----------|
| Dreher / Zerspanungsmechaniker (m/w/d) | `6b06be35515fe859a526203b` |
| Rundschleifer (m/w/d) | `6b06be480d3b4be96c6387cc` |

Beide gehören zu `customerID 6b06be1dbf4b78e6870d3a0c`. Gesendet wird **nur** bei vollständiger,
qualifizierter Bewerbung. Payload – **jedes Feld genau einmal**, Vor- und Nachname getrennt,
**kein** kombiniertes `name`/`fullname`/`vollstaendiger_name`:

```json
{
  "vorname": "Max",
  "nachname": "Mustermann",
  "telefon": "0151 23456789",
  "email": "max@beispiel.de",
  "stelle": "Dreher / Zerspanungsmechaniker (m/w/d)",
  "erfahrung_drehen": "Mehrjährige Erfahrung im konventionellen Drehen von Einzelteilen und Kleinserien",
  "luenetten": "Arbeitet regelmäßig mit Lünetten, auch bei langen Teilen",
  "messen_mikrometer": "Misst sicher mit dem Mikrometer (Alltag)",
  "zyklenprogrammierung": "nicht erfüllt – Keine Kenntnisse in Zyklenprogrammierung",
  "nicht_erfuellt": "Zyklenprogrammierung",
  "datum": "25.11.2026",
  "datenschutz": "Ja (Einwilligung mit Absenden, Art. 6 Abs. 1 lit. a DSGVO)",
  "quelle": "Karriere-Landingpage Gebhardt Maschinenbau",
  "seite": "https://…"
}
```

Rundschleifer statt der vier Dreher-Felder: `erfahrung_rundschleifen`,
`messen_mikrometer_rachenlehre`, `wellen_richten`, `nachzentrieren`.

Die Kachel stellt kein abrufbares Feldschema bereit; die Feldnamen folgen der Konvention der
bestehenden Anbindungen derselben Agentur (`vorname`, `nachname`, `telefon`, `email` als
Standardfelder, alles Weitere als Zusatzfelder).

**Testeintrag (25.11.2026):** je Kachel eine Bewerbung über das echte Formular – beide mit
HTTP 200 angenommen, Lead-IDs `6b06c3460d3b4be96c6e68cb` (Dreher) und
`6b06c34d0d3b4be96c6e6f11` (Rundschleifer). Nachname „Testeintrag … (bitte löschen)“.

## Noch zu prüfen

- **Impressum:** Name der Geschäftsführung fehlt (`[bitte ergänzen]` in `impressum.html`).
- **Firmendaten** (Adresse Jahnstraße 5, Tel. 06103 49310, HRB 32074 AG Offenbach, „seit über
  35 Jahren“, Druckindustrie) stammen aus öffentlichen Verzeichnissen – bitte bestätigen.
- **Datenschutzerklärung** ist ein Mustertext für diese Seite – bitte vom Kunden freigeben lassen.
- Analytics-Snippet am Seitenende wie auf der Referenzseite (`data-website-id="gebhardt_maschinenbau_gmbh"`).
- Es gibt keine E-Mail-Adresse – Kontakt läuft über die Telefonnummer.
