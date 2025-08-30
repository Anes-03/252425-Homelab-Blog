# 252425 Homelab – Design System Guide

Ziel: Dieses Dokument definiert das exakte Design (Tokens, Komponenten, Verhalten), sodass du es in anderen Projekten 1:1 reproduzieren kannst – unabhängig vom Framework. Es ist bewusst technologieagnostisch (HTML/CSS/JS).

## 1) Design Tokens (Canonical Values)
- Farben (Dark – Default)
  - `--bg-color: #121212`
  - `--header-bg: #1f1f1f`
  - `--card-bg: #1f1f1f`
  - `--text-color: #e0e0e0`
  - `--button-bg: #2a2a2a`
  - `--button-hover: #3a3a3a`
  - `--border-color: rgba(255, 255, 255, 0.1)`
  - `--footer-bg: #1a1a1a`
  - `--title-gradient-start: #ffffff`
  - `--title-gradient-end: #a0a0a0`
- Farben (Light)
  - `--bg-color: #f5f5f5`
  - `--header-bg: #ffffff`
  - `--card-bg: #ffffff`
  - `--text-color: #333333`
  - `--button-bg: #f0f0f0`
  - `--button-hover: #e0e0e0`
  - `--border-color: rgba(0, 0, 0, 0.1)`
  - `--footer-bg: #e0e0e0`
  - `--title-gradient-start: #333333`
  - `--title-gradient-end: #666666`
- Typo & Spacing
  - Basis: `font-family: Arial, sans-serif`, Grundgröße `16px`, Zeilenhöhe ~1.4
  - Radii: Card `12px`, Button `6px`, Logo/Icons `8–10px` (rund)
  - Schatten:
    - Card: `0 4px 6px rgba(0,0,0,0.3)`
    - Card:hover: `0 8px 16px rgba(0,0,0,0.4)`
    - Navbar: komplexer Glasmorphism (siehe unten)
  - Border: `1px solid var(--border-color)`
  - Transitions: Standard `0.3s ease` (Hover/Theme/Modals), Modal‑Close `0.2s`
- Breakpoints
  - Kartenraster: 1 Spalte (<600px), 2 Spalten (≥600px), 3 Spalten (≥900px)
  - Navbar Switch (Burger): ≤768px mobiles Menü

## 2) Grundgerüst (Seitenrahmen)
- HTML
  - `<html lang="de" data-theme="dark|light">`
  - `<meta name="viewport" content="width=device-width, initial-scale=1.0">`
  - OG/Twitter‑Metatags für Sharing (Titel, Beschreibung, Bild)
- Body
  - `body { margin:0; padding:0; min-height:100vh; display:flex; flex-direction:column; background:var(--bg-color); color:var(--text-color); padding-top:70px; }`
  - `section` Container: max‑Breite `1200px`, Seitenabstand `1rem`

## 3) Komponenten (Markup + Regeln)
- Navbar (fixed, glasmorph)
  - Struktur:
    - `nav.navbar > .navbar-container`
    - Brand: `a.navbar-logo > img.logo-img + span.logo-text`
    - Menü: `#navbar-menu .navbar-nav > li.nav-item > a.nav-link`
    - Toggle: `#navbar-toggle` mit `span.hamburger-line`
  - Stil (Kernaussagen):
    - Fixiert, blur (backdrop-filter), sanfte Schatten, leicht transparenter Hintergrund (linear‑gradient)
    - `.nav-link` mit abgerundeten Kapseln, dezente Hover‑Luminanz, Unterstreichung via `::after`
    - Mobil: Vollbild‑Sidepanel, `left: -100%` → `left: 0` bei `.active`
- Footer
  - Struktur:
    - `footer > .footer-content > img.footer-logo + p + .social-media .social-icons a`
  - Stil: dunkler Hintergrund, Border‑Top, zentrale Ausrichtung, Icons mit Scale‑Hover
- Cards (Inhalte, Dienste, Projekte)
  - Struktur: `.cards-container` (Grid) → `.card` mit `h3`, `p`, `a`
  - Regeln: Card‑Hover (lift + shadow), Buttons als Block mit max‑Breite `200px`
- Buttons/Links in Cards
  - Basis: `display:block; padding:0.6rem 1rem; background:var(--button-bg); border:1px solid var(--border-color); border-radius:6px; transition:all .3s`
  - Hover: `background:var(--button-hover); transform:scale(1.05)`
- Modals
  - Struktur: `div.modal > div.modal-content > span.close + [Inhalt]`
  - Verhalten: `.show` einblenden (flex), Close animiert (`.closing` → 200ms)
  - Look: Card‑ähnlich, mit stärkerem Shadow/Blur; Varianten möglich (z. B. MenuPy Gradient)
- Theme‑Switch
  - `#themeSwitch` fixed, rund (50px), Box‑Shadow; SVG‑Icon (Sonne/Mond) wechselt

## 4) Verhalten (JS‑Schnittstelle)
- Theme Management (identisch wiederverwendbar)
  - Toggle setzt `data-theme` auf `html` und speichert in `localStorage('theme')`
  - Systempräferenz via `matchMedia('(prefers-color-scheme: dark)')` berücksichtigen
- Modals
  - Öffnen: Attribut `data-modal` am Trigger → `#modalId` erhält `.show`
  - Schließen: Klick auf `.close` oder außerhalb, `closing` Animation 200ms
- Navbar
  - Aktive Seite: Pfadvergleich → `.nav-link.active`
  - Mobile Toggle: `.active` auf Menü/Toggle, schließen bei Außenklick und Linkklick

## 5) Reuse‑Checkliste (Minimal)
1) CSS‑Variablen definieren (Dark/Light), Body/Section‑Basen übernehmen.
2) Komponentenblöcke stylen:
   - Navbar: aus `navbar.css` (Stile 1:1) übernehmen.
   - Footer: aus `footer.css` (Stile 1:1) übernehmen.
   - Cards/Grid/Buttons: aus `index.html`/`faq.html` Inline‑Styles in globales Stylesheet extrahieren (Werte siehe Tokens/Regeln oben).
   - Modals/Theme‑Switch: Werte und Animationen wie definiert.
3) JS übernehmen:
   - Theme: `theme.js` (ohne Änderungen nutzbar, solange IDs `#themeSwitch`/`#themeIcon` existieren).
   - Modals: `modal.js` (setzt auf `.modal`, `.close` und `data-modal`).
   - Navbar‑Aktivzustand/Mobile: entweder `load-navbar.js` (wenn per Partial) oder eigenes Script gemäß Regeln.
4) Assets/Icons:
   - Font Awesome per CDN einbinden.
   - Favicons und Social‑Preview (OG/Twitter) setzen.

## 6) Beispiel‑Skeleton (HTML‑Ausschnitte)
- Theme Toggle
```html
<div class="theme-switch" id="themeSwitch" title="Theme wechseln">
  <svg id="themeIcon" viewBox="0 0 24 24"><path d="..."/></svg>
  </svg>
</div>
```
- Cards Grid
```html
<section>
  <h2>Titel</h2>
  <div class="cards-container">
    <div class="card">
      <h3>Karten‑Titel</h3>
      <p>Beschreibungstext</p>
      <a href="#">Aktion</a>
    </div>
  </div>
</section>
```
- Modal
```html
<div id="exampleModal" class="modal">
  <div class="modal-content">
    <span class="close">&times;</span>
    <h2>Modal Titel</h2>
    <p>Inhalt</p>
  </div>
</div>
```
- Modal‑Trigger
```html
<div class="card" data-modal="exampleModal">Klick öffnet Modal</div>
```

## 7) Konsistenzregeln (Do/Don’t)
- Do: Nutze ausschließlich die Tokens für Farben, Border, Radii, Shadows, Transitions.
- Do: Halte Breakpoints (600/768/900) für Grids/Navbar ein.
- Do: Max‑Breite 1200px für Inhaltscontainer, Buttons max 200px.
- Don’t: Abweichende Schatten/Radius/Transitions mischen; keine Inline‑Farben außerhalb der Tokens.

## 8) Optional: 1‑Datei‑Übernahme
Wenn du kein Refactoring möchtest, kopiere für ein neues Projekt diese Dateien direkt:
- CSS: `navbar.css`, `footer.css` (und extrahiere Card/Modal/Theme‑Switch‑Stile aus `index.html` in ein globales Stylesheet).
- JS: `theme.js`, `modal.js` (optional `load-navbar.js` für Partials).
- Stelle sicher, dass IDs/Klassen wie oben existieren.

Mit diesem Guide kannst du das Design pixelgenau replizieren. Bei Bedarf erstelle ich dir gern ein konsolidiertes `252425-design.css` + `252425-design.js` als „Drop‑in Kit“.

