# Metzler Header — Mega-Menü

Header im Metzler Design System mit Kategorie-Flyouts für **Briefkästen** und **Sprechanlagen**.

Alles liegt in einer einzigen Datei: [`briefkasten-menu.html`](briefkasten-menu.html) — Tokens inline, keine Abhängigkeiten, keine Build-Schritte.

## Ansehen

Die Datei direkt im Browser öffnen. Für die Sticky-Header-Logik braucht es einen HTTP-Aufruf:

```bash
python3 -m http.server 8788
```

Dann `http://localhost:8788/briefkasten-menu.html` aufrufen und mit der Maus über „Briefkästen" oder „Sprechanlagen" fahren.

## Aufbau

Der Header besteht aus drei Reihen: Trust-Bar in Teal, Logo mit Suche und Icons, darunter die Kategorie-Navigation. Er startet nicht sticky; die Klasse `is-sticky` kommt per JS beim ersten Scrollen.

Das Mega-Menü öffnet per `:hover` und `:focus-within`, Escape schließt es, unter 48rem klappt es per Klick auf. Links ein sechsspaltiges Kachelraster der Unterkategorien, rechts eine 20rem breite Teaser-Karte, die über einen Stretched Link vollflächig klickbar ist.

Hover-Zustand von Kacheln und Teaser-Karte: Rahmen wird Teal, `--shadow-hover` kommt dazu. Keine Bewegung, kein Versatz.

## Bilder

| Ordner | Inhalt |
|---|---|
| `briefkaesten-subkategorien/` | 11 Kacheln + Wand-Banner für den Einfamilien Briefkasten |
| `sprechanlagen-subkategorien/` | 8 Kacheln + Wand-Banner für die XDM10 Video-Türsprechanlage |

Alle Kacheln sind 1200 × 1200 px, das Motiv auf 84 % der Kantenlänge skaliert und zentriert, Hintergrund reinweiß. Die Anthrazit-Korpusse sind auf eine gemeinsame Farbe abgeglichen: **RGB 71 / 79 / 89** (RAL 7016). Jedes Motiv liegt als `.jpg` und `.webp` vor; im Markup wird `.webp` referenziert.

Die beiden Banner zeigen das Produkt an einer weißen Putzwand, aus der Ecke fotografiert, mit warmem Streiflicht von oben links — gleiche Szene, gleicher Winkel, gleiche Zentrierung.

## Design System

Farben, Radien, Schatten und Typografie stammen aus dem Metzler Design System. Regeln, die hier gelten: alle Maße in rem, Farben ausschließlich über Tokens, Container mit `max-width: 100rem`, kein horizontales Padding am äußeren `<header>`, Karten mit `--radius-lg`.

Die Komponente ist zusätzlich im UI Kit hinterlegt: `header/preview-megamenu.html` im Repository `metzler-ui-kit`, eingebunden im Abschnitt **Header**.
