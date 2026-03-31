# Alphabees Founder Diaries — Project Notes for Claude

## Projektübersicht
Website für die "Alphabees Founder Diaries" — ein Blog von zwei Gründern (Marcel & Lasse) die AI-Ventures dokumentieren. Nebenbei werden eigene Tools verkauft (Toolkit-Seite).

## Dateistruktur
```
alphabees-founder-diary/           # GitHub Repo Root
├── index.html                     # Landing Page (React-basiert) — früher alphabees-founder-diaries.html
├── diaries.html                   # Blog-Übersichtsseite (Vanilla JS)
├── toolkit.html                   # Produkt-Seite (AlphaLearn, AlphaCount)
├── imprint.html                   # Impressum
├── .nojekyll                      # Verhindert Jekyll-Blocking von _prefixed Dateien auf GitHub Pages
├── CLAUDE.md                      # Diese Datei
├── DESIGN.md                      # Design System Referenz
├── partials/                      # Shared HTML Partials (alle Seiten außer diaries.html)
│   ├── _header.html               # Nav — wird via fetch geladen (NICHT von diaries.html — dort inline)
│   ├── _footer.html               # Footer — wird via fetch geladen von allen Seiten
│   └── _sidebar.html             # Sidebar — wird via fetch geladen von _template-auto.html
├── diaries/                       # Einzelne Diary-Artikel
│   ├── index.json                 # Artikel-Index — wird von diaries.html dynamisch geladen
│   ├── _template.html             # Standalone Template (Nav + Sidebar inline, Footer via fetch)
│   └── _template-auto.html       # Automation Template (Nav + Sidebar + Footer alle via fetch)
├── assets/
│   ├── palm_scroll_animation/     # plant_left_front.webp, plant_right_front.webp
│   │                              # WICHTIG: Beide Bilder sind bereits korrekt orientiert —
│   │                              # plant_right_front.webp zeigt nach links (inward), KEIN scaleX nötig
│   ├── founder-diary-categories_small/  # Hero-Bilder für Blog-Karten (Retro Pixel Art)
│   │   ├── Design_System.webp
│   │   ├── Error_Mistakes_Fails.webp
│   │   ├── Featured_Generic.webp
│   │   ├── Finance.webp
│   │   ├── Founder_Life.webp
│   │   ├── Founder_Statement.webp
│   │   ├── infrastructure-server-operations.webp
│   │   ├── Marketing.webp
│   │   ├── Marketing_1.webp
│   │   ├── Operations.webp
│   │   ├── Sales.webp
│   │   ├── Tool_Testing_Technology_Lab.webp
│   │   └── Welcome.webp
│   ├── Alphabees_Founder_Diaries.webp   # Hintergrundbild Landing Page
│   ├── Marcel_Founder_Diaries.webp
│   ├── Lasse_Founder_Diaries.webp
│   └── Alphabees_founder_diaries_Logo_white.svg
└── fonts/
    ├── Space_Mono/                # SpaceMono-Regular.ttf, Bold.ttf, Italic.ttf, BoldItalic.ttf
    └── bigola-display-font/       # bigola-display-regular.otf, italic.otf, outline.otf
```

## Deployment
- **GitHub Repository**: `https://github.com/beemarcello/alphabees-founder-diary`
- **GitHub Pages URL**: `https://beemarcello.github.io/alphabees-founder-diary/`
- **`.nojekyll`** im Root: notwendig, damit GitHub Pages Dateien mit `_` Prefix ausliefert (sonst 404 auf Partials)
- Domain noch ausstehend — muss in Cloudflare verbunden werden

## Technischer Stack
- **Landing Page** (`index.html`): React (via CDN Babel), Tailwind CSS
- **Alle anderen Seiten**: Vanilla HTML/CSS/JS, keine externen Frameworks
- **Fonts**: Lokal eingebunden via `@font-face` auf allen Seiten — kein CDN (CDN wird auf Mobile blockiert)
- **Partials**: Werden via `fetch('partials/_*.html')` geladen (bzw. `fetch('../partials/_*.html')` aus `diaries/`)
- **Header auf diaries.html**: Direkt inlined (NICHT via Partial) — so funktioniert die Subscribe-Animation zuverlässig
- **Nav auf Seiten mit Partial-Header**: MutationObserver erkennt wenn Nav ins DOM geladen wurde, dann `--nav-height` setzen + Scroll-Handler + Subscribe-Animation + Hamburger-Wiring

## Wichtige Implementierungsdetails

### Landing Page (index.html)
- React + Babel via CDN, Tailwind CSS
- Hintergrundbild: `assets/Alphabees_Founder_Diaries.webp` (lokales Asset — kein Unsplash)
- Fonts alle lokal via `@font-face`:
  ```css
  @font-face { font-family: 'Space Mono'; src: url('fonts/Space_Mono/SpaceMono-Regular.ttf') format('truetype'); font-weight: 400; }
  @font-face { font-family: 'Space Mono'; src: url('fonts/Space_Mono/SpaceMono-Bold.ttf') format('truetype'); font-weight: 700; }
  @font-face { font-family: 'Bigola Display'; src: url('fonts/bigola-display-font/bigola-display-regular.otf') format('opentype'); font-weight: 400; }
  ```
- Typewriter-Animation: Startdelay 400ms, Zeichengeschwindigkeit 30–55ms
- React Nav: Hamburger-State + Mobile Overlay (inline in Komponente, kein Partial)

### Hero-Animation (diaries.html)
- `#hero-wrapper`: height 285vh → sticky hält 185vh an
- **Phase 1** (0→vh scroll): Palmen fahren von -15% → -72%, H1 faded ein (0→1) + skaliert (0.78→1.0)
- **Hold** (vh→1.5vh): H1 bleibt auf 100% opacity stehen
- **Phase 2** (1.5vh→1.85vh scroll): H1 faded aus
- Nach Phase 2: normaler Seiten-Scroll, Palmen bleiben fix

### Palmenblätter (diaries.html)
- `position: fixed; z-index: 100; pointer-events: none`
- Nur Front-Layer aktiv: `plant_left_front.webp` + `plant_right_front.webp`
- CSS Filter: `hue-rotate(58deg) saturate(0.50) brightness(0.68) contrast(1.10)`
- `mix-blend-mode: screen` auf dem `<img>` (nicht auf Container — verhindert Naht-Artefakte)
- **KEIN `scaleX(-1)`** — beide Bilder sind bereits korrekt orientiert (rechtes Bild zeigt nach links)
- CSS Start: `#plant-left-front { left: 0; transform: translateX(-15%); }` / `#plant-right-front { right: 0; transform: translateX(15%); }`
- `object-position`: links `right center`, rechts `left center`
- JS transforms (kein scaleX):
  ```javascript
  plantLF.style.transform = `translateX(${xL}%)`;
  plantRF.style.transform = `translateX(${-xL}%)`;
  ```
- **Stacking Context**: `#hero-scene` hat `z-index: 45` (oberhalb Filter-Bar z-index 40, unterhalb Nav z-index 50) — damit liegen die Palmen auch über der sticky Filter-Bar

### Filter-Bar (diaries.html)
- `id="filter-bar"` — Anchor-Target für Back-Links aus Diary-Artikeln
- `position: sticky; top: var(--nav-height, 93px); z-index: 40`
- `--nav-height` wird per JS dynamisch aus der echten Nav-Höhe gesetzt (resize-safe)
- `max-width: 1200px` — gleiche Breite wie Blog-Content
- Hintergrund: `rgba(0,0,0,0.88)` mit `backdrop-filter: blur(14px)`
- Sticky-Detection via JS: `.is-sticky` Klasse wenn `getBoundingClientRect().top <= navH`
- Aufbau: Featured | Most Recent | All | Categories (Dropdown) | Searchbar

### Subscribe-Animation (Header in diaries.html)
- Nav ist direkt in diaries.html inlined (nicht via Partial)
- Beim Klick auf "subscribe": Eingabefelder fahren mit `max-width` Transition rein
- Name-Feld: `max-width 1100ms`, Email-Feld: `max-width 900ms` mit 180ms Delay
- Button-Text faded aus, Pfeil (→) erscheint nach 500ms

### Mobile Hamburger Nav
- Implementiert auf allen Seiten
- `_header.html` Partial enthält: Hamburger-Button `#mobile-menu-btn` + Fullscreen-Overlay `#mobile-nav-overlay` + JS-Wiring
- CSS: bei `max-width: 768px` werden `.nav-links` und `.nav-subscribe-wrap` ausgeblendet, Hamburger erscheint
- Hamburger-Animaton: 3 Striche → X (scaleX + rotate)
- Overlay: `position: fixed; inset: 0; background: rgba(0,0,0,0.97); z-index: 49`
- **Auf diaries.html**: Hamburger + Overlay direkt inline im HTML (da kein Partial-Header)
- **Auf toolkit.html / imprint.html**: Hamburger-Wiring MUSS im MutationObserver erfolgen (Nav lädt async)

### MutationObserver Pattern (für Seiten mit Partial-Header)
Wird auf `toolkit.html`, `imprint.html` und `_template-auto.html` verwendet:
```javascript
const headerObserver = new MutationObserver(function() {
    const nav = document.querySelector('#site-header nav');
    if (!nav) return;
    headerObserver.disconnect();
    function setNavHeight() {
        document.documentElement.style.setProperty('--nav-height', nav.getBoundingClientRect().height + 'px');
    }
    setNavHeight();
    window.addEventListener('resize', setNavHeight);
    let lastY = 0;
    window.addEventListener('scroll', function() {
        const y = window.scrollY;
        nav.classList.toggle('nav-hidden', y > lastY && y > 120);
        lastY = y;
    }, { passive: true });
    const btn = nav.querySelector('#nav-subscribe-btn');
    const wrap = nav.querySelector('#nav-subscribe-wrap');
    if (btn && wrap) btn.addEventListener('click', function() { wrap.classList.add('expanded'); });
    // Hamburger-Wiring (Nav kommt aus Partial — muss hier verdrahtet werden)
    const hamburger = document.getElementById('mobile-menu-btn');
    const overlay = document.getElementById('mobile-nav-overlay');
    if (hamburger && overlay) {
        hamburger.addEventListener('click', function() {
            const isOpen = hamburger.classList.toggle('open');
            overlay.classList.toggle('open', isOpen);
            document.body.style.overflow = isOpen ? 'hidden' : '';
        });
        overlay.querySelectorAll('a').forEach(function(a) {
            a.addEventListener('click', function() {
                hamburger.classList.remove('open');
                overlay.classList.remove('open');
                document.body.style.overflow = '';
            });
        });
    }
});
headerObserver.observe(document.getElementById('site-header'), { childList: true, subtree: true });
```

### Diary Artikel Template (diaries/_template.html)
- Standalone HTML, Nav direkt inlined (gleich wie diaries.html)
- Asset-Pfade mit `../` Prefix (eine Ebene höher)
- Zwei-Spalten Layout: Artikel (1fr) + Sidebar (260px sticky)
- Sidebar enthält: Autor, "Back to all diaries" → `../diaries.html#filter-bar`, Related Articles, Subscribe CTA
- Footer wird via `fetch('../partials/_footer.html')` geladen
- Kein Breadcrumb (bewusst weggelassen)
- Content-Blöcke: intro, h2, h3, blockquote, tool-card, steps, takeaways, highlight-box, outlook, tags, FAQ accordion

### Diary Artikel Template Auto (diaries/_template-auto.html)
- Alle Partials via fetch: `../partials/_header.html`, `../partials/_sidebar.html`, `../partials/_footer.html`
- Placeholder-Tokens: `<!-- TITLE -->`, `<!-- CATEGORY -->`, `<!-- DATE -->`, `<!-- READTIME -->`, `<!-- SUBTITLE -->`, `<!-- INTRO -->`, `<!-- HERO_IMAGE -->`, `<!-- META_DESCRIPTION -->`
- MutationObserver für Nav-Höhe (da Nav async geladen wird)
- Wird von der Make.com Automation befüllt — Python Script ersetzt alle Tokens und injiziert Artikel-HTML

### Dynamisches Diary-Listing (diaries.html + diaries/index.json)
- `diaries.html` lädt Artikel-Karten dynamisch via `fetch('diaries/index.json')`
- `diaries/index.json` ist die einzige Quelle für alle publizierten Artikel
- Neueste Artikel stehen zuerst im Array (werden per prepend eingefügt)
- Welcome-Karte bleibt hardcoded in diaries.html (statischer Intro-Post)
- IntersectionObserver läuft für hardcoded + dynamische Karten getrennt via `observeCards()`
- JSON-Struktur pro Eintrag: `href`, `tag`, `image`, `imageAlt`, `date`, `title`, `excerpt`, `author`, `authorImg`
- `index.json` wird automatisch vom Make.com Upload-Script beim Publish aktualisiert

### Toolkit-Seite (toolkit.html)
- Zwei Produkte: **AlphaLearn** (Live, grünes Badge) + **AlphaCount** (In Development, gelbes Badge)
- AlphaLearn: Feature-Liste mit Cyan-Dots, CTAs "Start for free" + "Learn more"
- AlphaCount: Feature-Liste mit Pink-Dots + Early Bird Box (Pink-Gradient) mit Waitlist-Formular
- "More tools coming" Teaser-Block
- Newsletter CTA Box (Cyan-Gradient, gleich wie diaries.html und _template.html)
- Header + Footer via Partial, MutationObserver Pattern (inkl. Hamburger-Wiring)

## Newsletter CTA Box (einheitliches Design auf allen Seiten)
```css
.newsletter-inner {
    background: linear-gradient(135deg, rgba(12,227,236,0.08), rgba(249,144,232,0.05));
    border: 1px solid rgba(12,227,236,0.20);
    border-radius: 12px;
}
.newsletter-text h2 { line-height: 1.15; }
```
Verwendet auf: `diaries.html`, `diaries/_template.html`, `toolkit.html`

## Ausstehende Aufgaben

### Newsletter-Datenbank einbinden (6 Stellen)
1. `diaries.html` — Header Nav Subscribe-Formular (Name + Email)
2. `diaries.html` — Newsletter-Block unten
3. `toolkit.html` — Newsletter-Block unten
4. `index.html` — Header Nav Subscribe (React-Komponente)
5. `index.html` — Newsletter-Block am Ende der Seite
6. `toolkit.html` — AlphaCount Early Bird Waitlist-Formular (separates Ziel)

### Domain & Deployment
- Domain noch ausstehend — muss in Cloudflare verbunden werden
- Kanonische URLs im Python Upload-Script müssen nach Domain-Setup ergänzt werden

### Automation Pipeline für Diary-Artikel
- Make.com Blueprint: `BloggerAgent_Alphabees.ai_FounderDiary`
- Flow: Notion → Aggregator → AI Agent → Python Upload → Google Sheets → Discord
- `_template-auto.html` wird von Google Drive geholt und im Python Script befüllt
- Python Script: `github_upload.py` in `/Automatisierungen/BloggerAgent_Alphabees.ai_FounderDiary/`
- Agent Prompt: `agent_prompt.txt` im gleichen Ordner
- Sidebar Related Articles noch hardcoded — später dynamisch via index.json

## Bekannte Eigenheiten
- `_header.html` Partial wird von `diaries.html` NICHT verwendet — dort inline wegen Subscribe-Animation
- Alle Nav-Links sagen "diary" (singular) — bewusst vereinheitlicht
- `backdrop-filter` auf Nav erzeugt eigenen Stacking Context — plant layers (z-index 100) liegen drüber, haben aber `pointer-events: none`
- `#hero-scene` hat `z-index: 45` statt 10 — nötig, damit Palmen über sticky Filter-Bar (z-index 40) gerendert werden
- Palmen-Bilder sind bereits korrekt orientiert — **NIE `scaleX(-1)` hinzufügen**, weder in CSS noch JS
- Wenn JS `style.transform` setzt, überschreibt es die CSS `transform` komplett — scaleX muss immer im JS-String enthalten sein wenn es gebraucht wird
- Space Mono hat kein Semibold — für bessere Lesbarkeit höhere Opacity statt Fontweight nutzen
- Fonts nie via CDN laden — auf Mobile werden externe Font-CDNs blockiert → immer lokale `@font-face`
- `infrastructure-server-operations.webp` — umbenannt von `Infrastructure — Server_Operations.webp` (Leerzeichen + Em-Dash verursachten URL-Probleme)
- GitHub Pages blockiert Dateien mit `_` Prefix ohne `.nojekyll` im Root
