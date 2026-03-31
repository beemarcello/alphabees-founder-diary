# Alphabees Founder Diaries — Design System

## Farben
| Rolle | Wert |
|---|---|
| Hintergrund | `#000000` |
| Akzent Cyan | `#0CE3EC` |
| Akzent Pink | `#F990E8` |
| Warm Beige (card-tag) | `#A89F91` |
| Status Live | `#4ade80` |
| Status Warning | `#fbbf24` |
| Status Bad | `#f87171` |

## Typografie

### Fonts
- **Bigola Display** — Headlines, Titel (lokal: `fonts/bigola-display-font/`)
- **Space Mono** — Alles andere, monospace (lokal: `fonts/Space_Mono/`)
  - Gewichte: Regular (400), Bold (700) — kein Semibold verfügbar
  - Für Lesbarkeit: höhere Opacity statt Bold verwenden
- **Fonts immer lokal via `@font-face`** — CDN-Fonts werden auf Mobile blockiert

### Schriftgrößen & Farben (diaries.html)
| Element | Größe | Farbe |
|---|---|---|
| Body | 14px | `rgba(245,245,245,0.90)` |
| Nav Links | 12px | `rgba(255,255,255,0.72)` |
| Nav Subscribe | 12px | `rgba(255,255,255,0.88)` |
| Page Header Eyebrow | 10px | `rgba(255,255,255,0.50)` |
| Page Header H1 | clamp(2.8rem, 6vw, 4.5rem) | `rgba(255,255,255,0.88)` |
| Page Header P | 14px | `rgba(255,255,255,0.62)` |
| Filter Buttons | 12px | `rgba(255,255,255,0.80)` |
| Card Tag | 9px | `#A89F91` |
| Card Date | 9px | `rgba(255,255,255,0.48)` |
| Card Title | 1.25rem | `rgba(255,255,255,0.92)` |
| Card Excerpt | 12px | `rgba(255,255,255,0.58)` |
| Author Name | 10px | `rgba(255,255,255,0.55)` |
| Card Read | 10px | `rgba(255,255,255,0.50)` |
| Section Divider | 10px | `rgba(255,255,255,0.45)` |
| Newsletter P | 12px | `rgba(255,255,255,0.58)` |
| Footer | 12px | `rgba(255,255,255,0.50)` |
| Search Input | 12px | `rgba(255,255,255,0.90)` |

## Abstände & Layout
- **Max Content Width**: 1200px
- **Seitenpadding**: 48px (Desktop), 24px (Mobile ≤900px)
- **Nav Padding**: 28px 48px
- **Blog Grid**: 3 Spalten, gap: 2px; erster Post full-width (featured)
- **Hero Wrapper Height**: 285vh

## Komponenten

### Karten
- Border: `1px solid rgba(255,255,255,0.05)`
- Background: `rgba(255,255,255,0.015)`
- Hover Border: `rgba(255,255,255,0.12)`
- Hover Background: `rgba(255,255,255,0.035)`
- Bild-Overlay: `linear-gradient(to bottom, transparent 40%, rgba(0,0,0,0.65) 100%)`
- Featured (erster Post): horizontal layout, Bild 52% Breite, Höhe 420px

### Buttons (allgemein)
- Border-Radius: 100px (Pill-Form)
- Active/Hover Akzent: `#F990E8`

### Filter-Bar Buttons
- Normal: `rgba(255,255,255,0.80)`, border `rgba(255,255,255,0.12)`
- Hover: `rgba(255,255,255,0.95)`, border `rgba(255,255,255,0.30)`
- Active: `#F990E8`, border `rgba(249,144,232,0.40)`, bg `rgba(249,144,232,0.06)`

### Nav
- Background: `rgba(0,0,0,0.85)` + `backdrop-filter: blur(12px)`
- Sticky, z-index: 50
- Hide on scroll down (ab 120px scrollY), show on scroll up
- `--nav-height` CSS-Variable wird per JS dynamisch gesetzt
- Subscribe Button Hover: weißer Hintergrund, schwarzer Text
- Subscribe Expand: Eingabefelder fahren via `max-width` Transition rein

### Mobile Nav (Hamburger)
- Breakpoint: `max-width: 768px` — desktop Nav-Links + Subscribe-Wrap verschwinden, Hamburger erscheint
- Hamburger-Button `#mobile-menu-btn`: 3 Striche (22px × 2px), `rgba(255,255,255,0.88)`
- Animation auf `.open`: Strich 1 → `translateY(7px) rotate(45deg)`, Strich 2 → `opacity 0 scaleX(0)`, Strich 3 → `translateY(-7px) rotate(-45deg)`
- Overlay `#mobile-nav-overlay`: `position: fixed; inset: 0; background: rgba(0,0,0,0.97); z-index: 49`
- Overlay Nav-Links: Space Mono, 20px, `rgba(255,255,255,0.72)` → `#fff` on hover/active
- Subscribe-Button im Overlay `.mob-sub-btn`: Pill, `rgba(255,255,255,0.10)` background, 13px
- Body `overflow: hidden` während Overlay offen

### Newsletter CTA Box (einheitlich auf allen Seiten)
```css
.newsletter-inner {
    background: linear-gradient(135deg, rgba(12,227,236,0.08), rgba(249,144,232,0.05));
    border: 1px solid rgba(12,227,236,0.20);
    border-radius: 12px;
    padding: 48px 56px;
}
.newsletter-text h2 {
    font-family: 'Bigola Display', serif;
    font-size: clamp(1.6rem, 3vw, 2.4rem);
    line-height: 1.15;
}
```
Verwendet auf: `diaries.html`, `diary/_template.html`, `toolkit.html`

### Sidebar CTA (diary artikel)
```css
.sidebar-cta {
    background: linear-gradient(135deg, rgba(12,227,236,0.12), rgba(249,144,232,0.08));
    border: 1px solid rgba(12,227,236,0.20);
    border-radius: 8px;
    padding: 20px;
}
```

### Toolkit Produkt-Cards
- Live-Produkt: `background: linear-gradient(135deg, rgba(12,227,236,0.05) 0%, transparent 60%)`
- Coming Soon: `background: linear-gradient(135deg, rgba(249,144,232,0.05) 0%, transparent 60%)`
- Border: `1px solid rgba(255,255,255,0.06)`, Hover: `rgba(255,255,255,0.12)`
- Border-Radius: 12px
- Layout: 2-Spalten Grid `1fr 1fr`, gap 64px

### Early Bird Box (AlphaCount)
```css
.early-bird-box {
    background: linear-gradient(135deg, rgba(249,144,232,0.08), rgba(12,227,236,0.04));
    border: 1px solid rgba(249,144,232,0.22);
    border-radius: 10px;
}
```

### Status Badges (Toolkit)
- Live: `color: #4ade80`, bg `rgba(74,222,128,0.08)`, border `rgba(74,222,128,0.20)`
- In Development: `color: #fbbf24`, bg `rgba(251,191,36,0.08)`, border `rgba(251,191,36,0.20)`
- Puls-Animation auf Status-Dot: `opacity 2s ease-in-out infinite`

## Palmenblätter — CSS & Stacking

### CSS Filter
```css
filter: hue-rotate(58deg) saturate(0.50) brightness(0.68) contrast(1.10);
```
- `hue-rotate(58deg)`: verschiebt Grün (120°) → Cyan/Teal (178°), passend zu `#0CE3EC`
- `mix-blend-mode: screen` auf `<img>` (nicht auf Container)

### Positionierung & Transform
- **KEIN `scaleX(-1)`** — beide Bilder (`plant_left_front.webp`, `plant_right_front.webp`) sind bereits korrekt orientiert
- `plant_left_front.webp`: Blätter zeigen nach rechts (inward) ✓
- `plant_right_front.webp`: Blätter zeigen nach links (inward) ✓
- `object-position`: links `right center`, rechts `left center`
- CSS Startpositionen: `translateX(-15%)` links / `translateX(15%)` rechts
- JS transforms:
  ```javascript
  plantLF.style.transform = `translateX(${xL}%)`;
  plantRF.style.transform = `translateX(${-xL}%)`;   // kein scaleX!
  ```

### Z-Index Stacking (diaries.html)
| Element | z-index | Grund |
|---|---|---|
| Nav | 50 | Oberste Schicht |
| Mobile Nav Overlay | 49 | Direkt unter Nav |
| `#hero-scene` | 45 | Über Filter-Bar, unter Nav — damit Palmen über Filter-Bar gerendert werden |
| `.plant-layer` | 100 | Innerhalb des hero-scene Stacking Contexts |
| Filter-Bar | 40 | Sticky unter Hero-Palmen |
| Mobile Hamburger Button | 101 | Immer sichtbar |

> **Wichtig**: `#hero-scene` hat `position: sticky` und erzeugt einen Stacking Context. Die darin enthaltenen `.plant-layer` (z-index 100) konkurrieren auf der Ebene ihres Elternelements (hero-scene, z-index 45) — nicht global. Deshalb muss `#hero-scene` höher als der Filter-Bar (40) stehen.

## Scroll-Animation Werte
```javascript
const START = -15;   // Startposition (% translateX) — sichtbar im Frame
const REST  = -72;   // Endposition nach Phase 1 — am Rand, noch leicht sichtbar
// Phase 1: 0 → vh    — Palmen auf, H1 fade-in + scale
// Hold:    vh → 1.5vh — H1 bleibt auf 100%
// Phase 2: 1.5vh → 1.85vh — H1 fade-out
function easeOut(t) { return 1 - Math.pow(1 - t, 3); }
```

### Hero Intro Text
| Element | Größe | Farbe |
|---|---|---|
| Eyebrow | 9px | `rgba(255,255,255,0.40)` |
| H1 | clamp(2.4rem, 5vw, 3.8rem) | `rgba(255,255,255,0.92)` |
| Subheadline P | 15px | `rgba(255,255,255,0.72)` |

## Diary Artikel Template — Content-Blöcke

### Intro Paragraph
```css
p.intro { font-size: 16px; color: rgba(255,255,255,0.72); border-left: 2px solid rgba(249,144,232,0.50); padding-left: 16px; }
```

### H2 (Bigola Display, Cyan-Randlinie)
```css
article h2 { font-family: 'Bigola Display'; font-size: 1.7rem; border-left: 2px solid #0CE3EC; padding-left: 16px; }
```

### Tool Card
- Dark card mit Tabelle: Zweck, Warum, Ergebnis
- Status-Klassen: `.status-ok` (#4ade80), `.status-warn` (#fbbf24), `.status-bad` (#f87171)

### Steps (nummeriert)
- Cyan-Zahlen in eckiger Box `rgba(12,227,236,0.10)`
- Border-bottom zwischen Steps

### Key Takeaways Box
- Rosa Label, runde Zahlen `rgba(249,144,232,0.12)`

### Highlight Box
- Cyan-getönter Hintergrund + Border

### Outlook Block
- Gestrichelter Border `rgba(255,255,255,0.12)`

### FAQ Accordion
- `<details>/<summary>` native HTML
- Offen: Cyan-Hintergrund + rotiertes `+`-Icon

### Sidebar
- 260px, `position: sticky; top: var(--nav-height)`
- Blöcke: Autor, Back-Link (`../diaries.html#filter-bar`), Related Articles, CTA
- CTA: Cyan→Pink Gradient Border

## Hero-Bilder (founder-diary-categories_small/)
Retro Pixel Art, 8-bit Style, Dark Background — Thema-Zuordnung:

| Dateiname | Thema |
|---|---|
| `Design_System.webp` | Design, UI/UX, Webdesign |
| `Error_Mistakes_Fails.webp` | Wins and Fails, Fehler, Learnings |
| `Featured_Generic.webp` | Strategy, Product, allgemein |
| `Finance.webp` | Finance, Kosten, Revenue |
| `Founder_Life.webp` | Founder Life, Alltag, Reflexion |
| `Founder_Statement.webp` | Founder Opinion, Meinungen, Analyse |
| `infrastructure-server-operations.webp` | Operations, Server, Infrastruktur |
| `Marketing.webp` | Marketing, Content, Social Media |
| `Operations.webp` | Operations, Prozesse, Workflows |
| `Sales.webp` | Sales, Deals, Kunden |
| `Tool_Testing_Technology_Lab.webp` | Tool Testing, AI Tools, Automation, Custom Tools |
| `Welcome.webp` | Intro, Statement (Welcome-Karte, hardcoded) |

Neue Bilder mit diesen Prompts generieren (Midjourney / DALL-E):
> Retro Pixel art, 8-bit style, dark background, [thematisches Element], cyan (#0CE3EC) and magenta (#F990E8) accent lights, tropical palm silhouette, clean minimal composition

## Animationen
- **Karten Pop-In**: `opacity + translateY(32px)`, 600ms `cubic-bezier(0.16, 1, 0.3, 1)`, Stagger 90ms
- **Filter Transitions**: `0.25s` all
- **Subscribe Expand**: Name 1100ms, Email 900ms + 180ms delay, `cubic-bezier(0.4, 0, 0.2, 1)`
- **Scroll Hint**: `scrollHintPulse` 2s ease-in-out infinite
- **Status Badge Pulse**: `opacity` 2s ease-in-out infinite (Toolkit)
- **Hamburger → X**: `transform 0.3s ease, opacity 0.3s ease`
- **Typewriter (index.html)**: Startdelay 400ms, Zeichengeschwindigkeit 30–55ms random

## Mobile Breakpoints
| Breakpoint | Änderungen |
|---|---|
| `≤ 900px` | Seitenpadding 48px → 24px, Blog-Grid 3-spaltig → 1-spaltig, Featured Card vertikal |
| `≤ 768px` | Desktop Nav verschwindet, Hamburger erscheint; Nav-Padding reduziert |
| `≤ 600px` | Toolkit Grid 2-spaltig → 1-spaltig, Early Bird Box padding reduziert |

Scroll-Animation auf Mobile (`≤ 900px`): Palmen-Slide deaktiviert → nur Fade-in (weniger Ablenkung auf kleinen Screens)
