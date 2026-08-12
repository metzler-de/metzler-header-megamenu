# Aufgabe: Teaser-Banner im Kategorie-Flyout

## Kontext

Jedes Kategorie-Flyout im Header bekommt rechts neben dem Kachelraster eine Teaser-Karte,
die ein Leitprodukt der Kategorie zeigt. Referenzumsetzung liegt vor:

- Live: https://metzler-de.github.io/metzler-header-megamenu/briefkasten-menu.html
- Quelle: https://github.com/metzler-de/metzler-header-megamenu → `briefkasten-menu.html`
- Kit-Komponente: `header/preview-megamenu.html` im Repository `metzler-ui-kit`

Die Referenz ist eine statische HTML-Seite. Zu tun ist die Übernahme in den Shop:
Markup als Template, Inhalte aus dem CMS, Bilder über die Medienverwaltung.

## Umfang

Drei Kategorien sind fertig ausgearbeitet: **Briefkästen**, **Paketboxen**, **Sprechanlagen**.
Die Karte ist pro Kategorie einmal vorhanden, nicht pro Unterkategorie.

## Aufbau der Karte

Von oben nach unten, alles innerhalb einer Karte mit `--radius-lg` und Hintergrund `--color-teal-50`:

1. **Bildfläche** — weiße Fläche mit `--radius-lg`, Innenabstand `0.375rem`, darin das Bild
   quadratisch (`aspect-ratio: 1 / 1`, `object-fit: cover`, `--radius`)
2. **Eyebrow** — Kategorie-Bezeichnung, Versalien, `--color-teal`
3. **Titel** — Produktname, zwei Zeilen möglich
4. **Merkmalzeile** — drei Stichworte, mit `·` getrennt, `--color-graphite-700`
5. **Button** — `.btn .btn-primary .btn-block`, volle Breite

## Verhalten

- Die **gesamte Karte** ist klickbar, nicht nur der Button. Umsetzung als Stretched Link:
  der Button-Anker bekommt ein `::after` mit `position: absolute; inset: 0`, die Karte
  `position: relative`. **Keine** verschachtelten `<a>`-Elemente — das wäre ungültiges HTML.
- **Hover und `:focus-within`**: Rahmen wechselt von `--color-graphite-200` auf `--color-teal`,
  dazu `--shadow-hover`. Sonst nichts — **keine Bewegung, kein `transform`, kein Bild-Zoom**.
  Das gilt identisch für die Kacheln im Raster.
- Der Rahmen ist bereits im Ruhezustand vorhanden und wechselt nur die Farbe. Würde er erst
  im Hover entstehen, springt das Layout um `0.0625rem` pro Seite.
- `prefers-reduced-motion: reduce` schaltet die Transition ab.
- Fokusring auf dem Button: `outline: 0.125rem solid var(--color-teal)`, `outline-offset: 0.125rem`.

## Bildvorgaben

| Vorgabe | Wert |
|---|---|
| Format | quadratisch, 1200 × 1200 px |
| Dateiformate | `.webp` ausliefern, `.jpg` als Rückfallebene vorhalten |
| Motivgröße | Produkt füllt 86 % der Bildhöhe |
| Position | exakt mittig, horizontal und vertikal |
| Szene | Produkt am Haus, weiße Putzwand, warmes Streiflicht von oben links |
| Perspektive | Eckansicht, Front links, geschlossene Seitenwange rechts, Kamera leicht unterhalb der Mitte |
| Korpusfarbe | Anthrazit RAL 7016, Zielwert RGB 71 / 79 / 89 |
| Ladeverhalten | `loading="lazy"`, `width` und `height` gesetzt |

Alle drei Banner nutzen dieselbe Szene, damit die Flyouts als Serie wirken. Beim Anlegen
weiterer Kategorien ist das die Vorlage.

## Inhalte

| Kategorie | Eyebrow | Titel | Merkmale | Button | Ziel |
|---|---|---|---|---|---|
| Briefkästen | Einfamilien Briefkasten | Metzler Briefkasten aus hochwertigem Stahl | Farbe · Lasergravur · Befestigung | Jetzt konfigurieren | `/briefkasten` |
| Paketboxen | Paketbox mit Briefkasten | Metzler Paketbox XL mit Briefkasten | Farbe · Lasergravur · Befestigung | Jetzt konfigurieren | `/paketboxen-mit-gravur` |
| Sprechanlagen | Video-Türsprechanlage | Metzler XDM10 mit austauschbarem Namensschild | Farbe · Namensschild · Klingeltaster | Jetzt konfigurieren | `/video-tuersprechanlagen` |

Eyebrow, Titel, Merkmale, Button-Text, Ziel-URL und Bild sollen **pflegbar** sein, nicht im
Template stehen. Zielbild: pro Kategorie ein Datensatz mit diesen sechs Feldern.

## Responsive

- Ab `48rem`: Flyout zweispaltig, `minmax(0, 1fr)` für das Kachelraster, `20rem` feste Breite
  für die Teaser-Karte, Abstand `2.5rem`. Kachelraster sechsspaltig.
- Unter `48rem`: einspaltig, Karte unter dem Raster, Kacheln zweispaltig, Flyout klappt per
  Klick auf statt per Hover.

## Technische Randbedingungen

- Alle Maße in `rem`, keine `px`.
- Farben, Radien und Schatten ausschließlich über die Tokens aus `metzler-tokens.css`.
  Keine eigenen Hex-Werte, keine eigenen Schattenwerte.
- Container `max-width: 100rem`, Außenabstand `4rem` ab Tablet, `1.5rem` mobil.
- Schrift immer `var(--font-family)`.

## Abnahmekriterien

- [ ] Karte in allen drei Flyouts sichtbar, Inhalte aus der Pflege, nicht hart im Template
- [ ] Gesamte Karte klickbar, Cursor auf der ganzen Fläche, genau ein `<a>` pro Karte
- [ ] Hover ändert nur Rahmenfarbe und Schatten, Karte bewegt sich nicht, Layout springt nicht
- [ ] Tastaturbedienung: Karte per Tab erreichbar, `:focus-within` zeigt denselben Zustand,
      Fokusring sichtbar
- [ ] Bilder 1200 × 1200, `.webp` ausgeliefert, `width`/`height` gesetzt, kein Layout-Shift
- [ ] Unter `48rem` liegt die Karte unter dem Raster und ist vollständig bedienbar
- [ ] `check_compliance` des Design-System-MCP meldet keine Verstöße
- [ ] Kein `px`-Wert und kein Hex-Farbwert im neuen CSS

## Offene Punkte für die Umsetzung

- Wo werden die Karten gepflegt? Vorschlag: eigener Inhaltstyp „Kategorie-Teaser" mit
  Zuordnung zur Kategorie, damit pro Kategorie genau eine Karte existiert.
- Sollen die Ziel-URLs auf Kategorien oder direkt auf den Konfigurator zeigen?
- Wer erzeugt die Banner für weitere Kategorien? Die Bildvorgaben oben sind die Vorlage.
