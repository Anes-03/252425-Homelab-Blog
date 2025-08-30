# 252425 Homelab – Website‑Dokumentation

Diese Dokumentation beschreibt Aufbau, Design und Verhalten der Website, sodass neue Personen schnell verstehen, worum es geht und wie alles strukturiert ist. Sie basiert auf dem aktuellen Stand der Dateien im Projektverzeichnis.

## 1) Überblick
- Zweck: Vorstellung des persönlichen Homelab-Projekts, inklusive Dienste, Projekte und einer App (MenuPy), plus Kontakt/Support.
- Art: Statische Website (HTML/CSS/JS) ohne Build‑Schritt.
- Sprache: Deutsch (UI‑Texte), einzelne README in Englisch.
- Domain: `252425.xyz` (CNAME vorhanden), Social‑Sharing‑Metadaten gepflegt.

## 2) Informationsarchitektur
- Hauptseiten (im Root):
  - `index.html` (Startseite, Überblick über Dienste, Projekte, Anwendungen, Modals).
  - `faq.html` (Fragen & Antworten; ähnlicher Stil, Kartenlayout).
  - `support.html` (Supporthinweise; Karten und ASCII‑Header).
  - `kontakt.html` (Kontaktformular; Mailto‑Formular, ASCII‑Header).
- Partials:
  - `navbar.html` (Navigationsleiste, per JS eingebunden).
  - `footer.html` (Footer, per JS eingebunden).
- Assets: `banner.jpeg`, `logo.jpeg`, Favicons (`favicon.ico`, `apple-touch-icon.png`, `favicon-16x16.png`, `favicon-32x32.png`).
- CSS: `navbar.css`, `footer.css`, `form-styles.css` (seitenübergreifend + Inline‑Styles pro Seite).
- JS: `load-navbar.js`, `footer.js`, `theme.js`, `modal.js`, `auto-modal.js`.

## 3) Navigation
- Inhalt: Links zu „Home“, „FAQ“, „Support“, „Kontakt“.
- Darstellung: Fixierte, transparente/glasmorphe Navbar mit Blur und Schatten (siehe `navbar.css`).
- Verhalten:
  - Navbar/Logo: Link zur Startseite.
  - Aktive Seite wird per JS (Pfadvergleich) markiert (`.nav-link.active`).
  - Mobile Menü (Burger): Ein-/Ausklappen über `.active`‑Klasse; schließt bei Linkklick und bei Klick außerhalb.
- Einbindung: `load-navbar.js` lädt `navbar.html` zur Laufzeit in `#navbar-placeholder`.

## 4) Footer
- Inhalt: Logo, Copyright‑Zeile (Jahr), Social‑Icons für A‑Ultra (Web, YouTube, X, Instagram, TikTok).
- Darstellung: Dunkler Footer mit Border‑Top, zentral ausgerichtet (`footer.css`).
- Einbindung: `footer.js` lädt `footer.html` in `#footer-container`.

## 5) Startseite – Aufbau (`index.html`)
- Header/Hero:
  - Vollbreites Bannerbild (`banner.jpeg`), Titel „Willkommen in meinem 252425 Homelab“ und Einleitungstext.
- Abschnitt „Meine Homelabdienste“:
  - Kartenraster (1–3 Spalten je Breakpoint).
  - Dienste: 252425 Cloud (Nextcloud), 252425 AI (zwei Instanzen), Home Assistant, n8n, Uptime Kuma, Intranet (teilw. „Coming Soon“).
  - Externe Links öffnen in neuem Tab.
- Abschnitt „Mein Technologie‑Stack“:
  - Drei Karten mit `data-modal` öffnen Modals: Hardware, Software & Dienste, Netzwerk & Sicherheit.
- Abschnitt „Meine Projekte“:
  - Karten mit Links zu Projektseiten und GitHub (Hacking Escape Room, A‑Ultra Website, Homelab Repo, Dorian Gray Escape Room).
- Abschnitt „252425 Anwendungen“:
  - Karte „MenuPy“ mit eigenem, farbverlaufendem Card‑Design und Modal.
- Modals:
  - `#hardwareModal`, `#softwareModal`, `#netzwerkModal`, `#menupyModal` (Inhalte: Hardware‑Specs, Software‑Stack, Netzwerk/Sicherheit, MenuPy‑Infos).
- Easter‑Egg: `#easter-egg` – Textnote für KI‑Leser.
- Globale Bedienelemente: Schweben­der Theme‑Switch (unten rechts).

## 6) Design‑System
- Farbvariablen (Dark/Light): In `<style>` via CSS‑Custom‑Properties definiert (`:root` und `[data-theme="light"]`).
  - Dark (Default): `--bg-color: #121212`, `--card-bg: #1f1f1f`, `--text-color: #e0e0e0` usw.
  - Light: Helle Palettenpendants; automatische Umschaltung oder manuell via Theme‑Switch.
- Typografie: Systemnahe Sans‑Serif (`Arial, sans-serif`), Basisgröße `16px`, skalierende Überschriften via `clamp()`.
- Layout:
  - Responsive Grids: Karten in 1/2/3 Spalten (Breakpoints bei ~600px/900px).
  - Spacing: Karten mit Border‑Radius (12px), Schatten, zartem Rahmen (`--border-color`).
- Komponenten:
  - Navbar: Glasmorphism‑Effekt (Gradient, Blur, Schatten), aktive/hover Zustände mit dezenten Übergängen.
  - Cards: Einheitliche Abstände, Buttons mit Hover‑Scale, max. Buttonbreite 200px, Fokus auf Lesbarkeit.
  - Modals: Zentriert, Glass‑Look (Box‑Shadows, Blur), Ein‑/Ausblend‑Animationen (`zoomIn`, `modalFadeOut`).
  - Theme‑Switch: Runde Schaltfläche, sanfte Hover‑Animation, Icon‑Rotation.
  - Social‑Icons: Font‑Awesome per CDN, Hover‑Scale und Farbwechsel.

## 7) Interaktionen & Verhalten
- Theme:
  - `theme.js` setzt/liest `localStorage('theme')`, berücksichtigt Systempräferenz (`prefers-color-scheme`), toggelt `data-theme` am `html`‑Element, ersetzt Inline‑SVG‑Icon.
- Navbar/Partials:
  - `load-navbar.js` lädt die Navbar, setzt aktive Links, steuert Mobile‑Menü und schließt es bei Außenklick.
  - `footer.js` lädt den Footer in alle Seiten.
- Modals:
  - `modal.js` öffnet Modals über `data-modal` auf Karten und schließt via Close‑Icon, Außenclick oder Animation.
  - `auto-modal.js` zeigt beim Seitenladen das `#menupyModal` automatisch an (Startseite).
- Scrolling:
  - Global: `html { scroll-behavior: smooth; }`.
  - Ankerlinks: In `navbar.js` zusätzlich Smooth‑Scroll für `href^="#"` implementiert.

## 8) Inhalte & Links (Kurzüberblick)
- Dienste (Startseite): Nextcloud Cloud, KI‑Dienste (aktuell), Home Assistant, n8n, Uptime Kuma, Intranet.
- Technologie‑Stack (Modals):
  - Hardware: PC (RTX 4060 Ti + Ryzen 5 5600), iMac M1, Raspberry Pi 5 (4 GB, 2 TB).
  - Software: Docker, Nextcloud, lokale KI‑Modelle, GitHub Pages, Cloudflare Tunnel, Tailscale, Moonlight/Sunshine; Geplant: Home Assistant, n8n.
  - Netzwerk: Cloudflare Zero Trust, Tailscale VPN.
- Projekte: Hacking Escape Room (Website + Riddles + GitHub), A‑Ultra Website (Preview + GitHub), Homelab (GitHub), Dorian Gray Escape Room (GitHub + Rätsel‑Links).
- Anwendung: MenuPy (Website + GitHub) inkl. prominenter Karte und Modal.

## 9) Technischer Aufbau
- Statisches Hosting: Hinweis auf GitHub Pages (CNAME vorhanden). Zusätzlich `wrangler.toml` im Root deutet Cloudflare‑Einsatz an (z. B. Tunnel/ZT in Inhalten erwähnt).
- Abhängigkeiten:
  - Extern: Font Awesome CDN (`cdnjs.cloudflare.com`) für Icons.
  - Intern: Keine Build‑Tools; reine HTML/CSS/JS, Fetch‑basierte Partials.
- Dateien (Auszug):
  - HTML: `index.html`, `faq.html`, `support.html`, `kontakt.html`, `navbar.html`, `footer.html`.
  - CSS: `navbar.css`, `footer.css`, `form-styles.css` (+ Inline‑Styles pro Seite).
  - JS: `load-navbar.js`, `footer.js`, `theme.js`, `modal.js`, `auto-modal.js`, `navbar.js` (ergänzende Funktionen; teilweise von `load-navbar.js` abgedeckt).
  - Medien: `banner.jpeg`, `logo.jpeg`, App‑Icon „MenuPy“ PNGs, Favicons.

## 10) Pflege & Erweiterung
- Neue Seite hinzufügen:
  - HTML anlegen, in `navbar.html` verlinken. `load-navbar.js` markiert den aktiven Link automatisch.
  - `navbar.css`/`footer.css` einbinden; `#navbar-placeholder` und `#footer-container` belassen.
- Neue Karte:
  - In den jeweiligen Abschnitten (Startseite/FAQ/Support) `div.card` hinzufügen; Texte/Links anpassen.
  - Bei Bedarf `data-modal` setzen und passendes Modal definieren.
- Neue Modals:
  - Struktur wie bestehend: `div.modal > div.modal-content` mit `span.close` und Inhalt; IDs eindeutig vergeben.
- Theme:
  - Farben über CSS‑Variablen in `:root` und `[data-theme="light"]` zentral pflegen.
- Icons:
  - Font Awesome verwenden (`<i class="fas ..."></i>` bzw. `fab` für Brands).

## 11) SEO & Social Sharing
- Open‑Graph/Twitter‑Metatags pro Seite gepflegt (Titel, Beschreibung, Bild `banner.jpeg`).
- `CNAME` verwendet Custom‑Domain `252425.xyz`.

## 12) Barrierefreiheit (Kurz)
- Plus: Klare Kontraste im Dark‑Theme, skalierende Typografie (`clamp`), Fokus auf große Klickflächen.
- Hinweise:
  - Prüfen von Tastaturnavigation/Fokus‑Stil bei Modals und Mobile‑Menü.
  - `alt`‑Texte sind gesetzt; semantische Überschriftenstruktur beibehalten.

---

## Anhang A – Wichtige Selektoren/IDs
- Platzhalter: `#navbar-placeholder`, `#footer-container`.
- Theme: `#themeSwitch`, `#themeIcon`, `[data-theme]` am `html`‑Element.
- Grid/Container: `.cards-container`, `.card`.
- Modals: `#hardwareModal`, `#softwareModal`, `#netzwerkModal`, `#menupyModal`, `.modal`, `.modal-content`, `.close`.
- Navbar: `.navbar`, `#navbar-menu`, `#navbar-toggle`, `.nav-link.active`.

## Anhang B – Schnellstart für Entwickler
- Lokale Vorschau: Direkt `index.html` im Browser öffnen (kein Build nötig).
- Bearbeiten: Inhalte in den HTML‑Dateien, Layout in `navbar.css`/`footer.css`/Inline‑Styles, Verhalten in `*.js`.
- Deployment: Über statisches Hosting (z. B. GitHub Pages mit `CNAME`).
