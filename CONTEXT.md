# Jan Kohler Website — Context & Session Log

## Projekt-Übersicht

Statische HTML-Website für Jan Kohler (Executive Coaching).
Gebaut mit: Tailwind CSS (CDN), Newsreader + Manrope (Google Fonts), Material Symbols.
Kein Framework, kein Build-Prozess — alles in einer einzigen HTML-Datei.

---

## Dateien im Ordner

| Datei | Beschreibung |
|---|---|
| `index.html` | **Coming Soon Page** — animierter Hintergrund, Kontaktformular, Hidden-Login (3×-Klick auf Logo → Credentials `Jan` / `comingsoon`) |
| `site.html` | **Aktive Hauptseite** (vor Coming Soon war das `index.html`) — Hero, About, Insights, Blog, Contact |
| `index-old.html` | Option 1 — alte Version, wird nicht weiterentwickelt |
| `blog-strategy.html` | Blog-Artikel "The Playing Field of Leadership" (Platzhalter-Inhalt) |
| `blog-communication.html` | Blog-Artikel "The Art of Clear Signals" (Platzhalter) |
| `blog-performance.html` | Blog-Artikel "Split Seconds" (Platzhalter) |
| `blog-endurance.html` | Blog-Artikel "The Marathon of Transformation" (Platzhalter) |
| `insight-decision-making.html` | Insights-Artikel "Resilient Decision Making" (Platzhalter) |
| `insight-silence.html` | Insights-Artikel "The Silence Between Actions" (Platzhalter) |
| `insight-executive-presence.html` | Insights-Artikel "Cultivating Executive Presence" (Platzhalter) |
| `insight-habit-excellence.html` | Insights-Artikel "The Habit of Excellence" (Platzhalter) |
| `imprint.html` | Impressum (Gerüst — Platzhalter gelb markiert, zum Ausfüllen) |
| `privacy.html` | Datenschutz (Gerüst — Platzhalter gelb markiert, DSGVO-konform für Web3Forms, Google Fonts, Tailwind CDN) |
| `CNAME` | GitHub-Pages-Konfig: `jankohlercoaching.de` |
| `portrait.jpeg` | Hero-Bild |
| `about.jpeg` | About-Sektion, auch Autor-Avatar auf Artikelseiten |
| `insights.jpeg` | Insights-Feature-Bild (und Hero des Decision-Making-Artikels) |
| `CONTEXT.md` | Diese Datei |

---

## Sektionen in site.html

| ID | Sektion | Inhalt |
|---|---|---|
| `#home` | Hero | Mobile: Bild oben (gestapelt) + Text unten. Desktop: Vollbild-Portrait + Text links mit Gradient. |
| `#about` | About | Split-Layout: Text + Stats links, Bild rechts |
| `#insights` | Coaching | Bento-Grid mit Feature-Bild + 3 Cards (alle verlinkt auf Insights-Artikelseiten) |
| `#blog` | Blog | Mobile: Coverflow-Karussell mit Auto-Advance. Desktop: 4-spaltige Karten-Grid mit versetztem Offset. |
| `#contact` | Kontakt | Kontaktinfo links (`office@jankohlercoaching.de`, Munich / Remote), Web3Forms-Formular rechts |

---

## Technische Entscheidungen

### Hero-Layout (site.html)

**Desktop (≥768px):** Vollbild-Hintergrundbild (`absolute inset-0`), `object-position: 72% top` schiebt Gesicht nach rechts, Text auf `max-w-lg` links. Links→rechts-Gradient sichert Lesbarkeit.

**Mobile (<768px):** Stack-Layout — Bild oben in eigenem Container (`h-[42vh] min-h-[320px] max-h-[520px]`, `mt-[60px]` damit nicht von der fixen Nav überlagert), Text darunter im normalen Flow. Kein Overlay mehr → kein Konflikt mit Gesicht.

CSS-Steuerung über `.hero-img { object-position: 50% top; }` (Mobile) und `@media (min-width: 768px) { object-position: 72% top; }` (Desktop). Bild: `portrait.jpeg` (lokal), volle Auflösung, `grayscale`.

### Coming Soon Page (index.html)

Eigenständige Seite, ist die Default-Landing auf `jankohlercoaching.de`. Vollversion liegt unter `site.html` und wird über Hidden-Login erreicht.

- **Animierter Hintergrund:** 4 weichgezeichnete Gradient-Blobs in den Site-Farben mit `float-a/b/c/d`-Keyframes (14–22s), zusätzlich rotierender konischer Gradient (`rotate-bg` 55s) für Tiefe, 10 schwebende Staub-Partikel (`rise` 14–32s), feine Noise-Overlay. `prefers-reduced-motion` respektiert.
- **Kontaktformular:** Web3Forms-Endpoint (gleicher Access Key wie Hauptseite), aber **eigener Subject-Header** "Coming Soon — new inquiry via jankohlercoaching.de" damit unterscheidbar von Hauptformular-Submissions.
- **Hidden Login:** 3× Klick auf "Jan Kohler"-Logo → Modal → Credentials `Jan` / `comingsoon` → JS-Redirect auf `site.html`. ESC schließt Modal, Backdrop-Klick auch.
- **Sicherheits-Caveat:** Rein clientseitig. Wer DevTools öffnet sieht das Passwort im Quelltext, und `site.html` ist per direkter URL immer erreichbar. Reicht als Soft-Gate für Pre-Launch, nicht als echter Schutz. Für echten Schutz wäre Netlify Identity / Cloudflare Access nötig.

### Mobile Blog-Karussell (Coverflow)

Auf Mobile (<768px) ersetzt ein 3D-Coverflow die gestapelte Card-Liste:
- Zentrale Karte scharf und groß, Nachbarkarten 78% skaliert, geneigt (`perspective: 1000px` + `rotateY ±10°`), mit `blur(4px)`
- Prev/Next-Buttons (links/rechts), Dot-Indikator unten (active = 22px Kapsel statt 6px Punkt)
- Auto-Advance alle 5s, pausiert wenn Tab unsichtbar (`visibilitychange`)
- Touch-Swipe (>40px Threshold)
- Container `h-[500px] overflow-hidden`, jede Karte `width: 230px; left: 50%; margin-left: -115px` für Zentrierung
- Desktop-Grid (`hidden md:grid grid-cols-2 lg:grid-cols-4`) bleibt unverändert, nur durch `md:hidden`/`hidden md:grid` umgeschaltet
- JS-Logik im `<script>`-Block nach dem Scroll-Spy-Code

### Mobile Section-Spacing

Globaler Pass auf alle 4 Hauptsektionen in `site.html`: `py-32` → `py-16 md:py-32` (halbiert vertikalen Abstand auf Mobile). Section-Header-Margins: `mb-20` → `mb-12 md:mb-20`. Footer-Top: `mt-32` → `mt-16 md:mt-32`. Gibt der Mobile-Version deutlich engeren Rhythmus, Desktop unverändert.

### Bilder — lokal statt extern
- Ursprüngliche KI-generierte Bilder (`lh3.googleusercontent.com`) waren verpixelt und können ablaufen.
- Alle ersetzten Bilder liegen lokal im Ordner → beim Teilen als ZIP oder via GitHub.

### Kontaktformular
- Backend: **Web3Forms** (kostenlos, 250 Submissions/Monat)
- Endpoint: `https://api.web3forms.com/submit`
- Access Key im HTML hardcoded (public — Web3Forms prüft serverseitig gegen die Domain)
- Empfänger: `office@jankohlercoaching.de`
- Einreichung per AJAX (Fetch), Seite lädt nicht neu
- Bei Erfolg: Form wird durch "Thank you"-Block ersetzt (`#cf-success`)
- Bei Fehler: Rote Meldung unter dem Button (`#cf-error`)
- Spamschutz: Honeypot-Feld `botcheck` (für Menschen unsichtbar)
- HTML5-Validierung (required, type=email) manuell ausgelöst, `novalidate` am Form verhindert Browser-Popups

### Editor (eingebaut in site.html, FAB unten rechts)
- Floating Pencil-Button (unten rechts) → aktiviert Bearbeitungsmodus
- Alle Textelemente werden `contenteditable`
- Bilder bekommen Hover-Overlay mit Kamera-Icon → Modal mit URL-Input oder Datei-Upload (base64)
- "Export HTML" → lädt saubere HTML-Datei ohne Editor-Code herunter
- "Exit" → verlässt den Bearbeitungsmodus
- UI komplett auf Englisch (passt zum Englisch-Audit der Seite)
- **Offen:** für Produktion ggf. hinter Tastenkombi (z.B. `Cmd+Shift+E`) verstecken statt sichtbarer FAB

### Scroll Spy (site.html)
- JS-basiert, kein Framework
- Aktiver Navlink wird per `data-section` und `window.scrollY` ermittelt
- `scroll-mt-28` auf allen Sektionen verhindert, dass die fixe Nav den Inhalt überdeckt

---

## GitHub Pages

- **Live-URL (primär):** https://jankohlercoaching.de
- **Alternative:** `www.jankohlercoaching.de` → leitet automatisch auf apex weiter
- **Fallback:** https://michaherz.github.io/jan-kohler-website/
- **Repo:** https://github.com/michaherz/jan-kohler-website (public)
- **Pages aktiviert:** 2026-05-03
- **Custom Domain verbunden:** 2026-05-03
- **Deploy:** automatisch bei Push auf `main`-Branch, Root-Verzeichnis
- **Build-Type:** legacy (Jekyll-default, aber ohne Jekyll-Konfig → reines Static-HTML)
- **HTTPS:** enforced (Let's Encrypt, auto-renew durch GitHub)

### Ionos DNS-Konfiguration

Die Domain `jankohlercoaching.de` ist bei Ionos registriert, Nameserver bleiben Ionos. Nur die DNS-Records zeigen auf GitHub:

| Typ | Hostname | Wert | Zweck |
|---|---|---|---|
| A | `@` | `185.199.108.153` | GitHub Pages |
| A | `@` | `185.199.109.153` | GitHub Pages |
| A | `@` | `185.199.110.153` | GitHub Pages |
| A | `@` | `185.199.111.153` | GitHub Pages |
| CNAME | `www` | `michaherz.github.io` | GitHub Pages www-Subdomain |

**Mail-Records (SPF, DKIM, DMARC, MX zu mx00/mx01.ionos.de) bleiben unangetastet** — Emails über `office@jankohlercoaching.de` laufen weiter über Ionos Mail.

**Vorher entfernt:** Default-Site-Records (A @ 217.160.0.36 und AAAA @ 2001:...), Ionos "Default Site" Service wurde über "Verwendungsart anpassen" deaktiviert.

### Repo-Struktur

- `CNAME` (im Repo-Root) → enthält nur `jankohlercoaching.de`. Diese Datei ist die Voraussetzung für GitHub Pages mit Custom Domain. **Nicht manuell ändern** — GitHub aktualisiert sie automatisch, wenn die Domain in den Pages-Settings geändert wird.

### Lokaler Workflow

```bash
cd ~/Desktop/jan-kohler-website
# Änderungen machen
git add .
git commit -m "beschreibung der änderung"
git push
# → nach ~1 min live auf jankohlercoaching.de
```

### Gitignore
- `.DS_Store`
- `Website/` (alter Archiv-Ordner)

---

## Offene Punkte / Ideen

- [x] ~~GitHub Pages live stellen~~ — erledigt 2026-05-03
- [x] ~~Custom Domain (Ionos) verbinden~~ — erledigt 2026-05-03
- [x] ~~Kontaktformular mit Backend verbinden~~ — erledigt 2026-05-03 (Web3Forms, Empfänger `office@jankohlercoaching.de`)
- [x] ~~Sprach-Audit: Seite konsequent auf Englisch vereinheitlichen~~ — erledigt 2026-05-03 (Blog-Cards, Nav, Editor-UI, JS-Meldungen, Footer-Links, Contact-Info, Form-Subject)
- [x] ~~Office-Angabe auf "Munich / Remote" setzen~~ — erledigt 2026-05-03 (vorher "Studio: Berlin / London / Remote")
- [x] ~~Sichtbare Email auf `office@jankohlercoaching.de` setzen~~ — erledigt 2026-05-03 (vorher `jan@kohler-coaching.com`)
- [x] ~~Blog-Artikel-Seiten als Platzhalter~~ — erledigt 2026-05-03 (4 Seiten: Strategy, Communication, Performance, Endurance — realistische Platzhalter-Prose, Cards verlinkt)
- [x] ~~Insights-Artikel-Seiten als Platzhalter~~ — erledigt 2026-05-03 (4 Seiten: Decision Making, Silence, Executive Presence, Habit of Excellence — Cards verlinkt)
- [x] ~~Impressum-Gerüst~~ — erledigt 2026-05-03 (`imprint.html`, gelbe Platzhalter — Jan muss Name/Adresse/USt-ID ausfüllen)
- [x] ~~Datenschutz-Gerüst~~ — erledigt 2026-05-03 (`privacy.html`, DSGVO-konform für Web3Forms, Google Fonts, Tailwind CDN, GitHub Pages)
- [x] ~~Coming Soon Page mit Hidden-Login~~ — erledigt 2026-05-05 (`index.html` = Coming Soon, `site.html` = Full Site. Login: 3×-Klick auf Logo → Modal → User: `Jan`, Pass: `comingsoon` → Redirect auf site.html. Achtung: client-seitig, nur Soft-Gate)
- [x] ~~Mobile-Hero optimiert (Text/Bild-Konflikt gefixt)~~ — erledigt 2026-05-05 (Stack-Layout auf Mobile: Bild oben mit `mt-[60px]` unter der Nav, Text unten. `object-position: 50% top` zeigt Gesicht ganz)
- [x] ~~Mobile Section-Spacing reduziert~~ — erledigt 2026-05-05 (`py-32` → `py-16 md:py-32` auf allen Sektionen, gleiche Behandlung für `mb-20` und `mt-32`)
- [x] ~~Mobile Blog-Karussell (Coverflow 3D)~~ — erledigt 2026-05-05 (mit Auto-Advance, Touch-Swipe, Dots; Desktop-Grid unverändert)
- [x] ~~Animierter Coming-Soon-Hintergrund~~ — erledigt 2026-05-05 (Gradient-Mesh-Blobs + rotating conic + Partikel + Noise; respektiert `prefers-reduced-motion`)
- [ ] **Nach Inhaltsfinalisierung:** Impressum + Datenschutz von Anwalt oder eRecht24-Service prüfen lassen
- [ ] Newsletter-Strategie (Mailchimp/Brevo/eigene Email-Sammlung?) + "Private Reading List"-Card und Footer-Link aktivieren
- [ ] Google Fonts selbst hosten (optional, reduziert externen Datentransfer an Google — Datenschutz-Bonus)
- [ ] Restliche Placeholder-Bilder (4× Blog-Cards, 1× Insights-Discipline-Card) durch echte Fotos ersetzen
- [ ] Deutsche Sprachversion hinzufügen (DE/EN-Umschalter via `de.html` — Option A)
- [ ] Inline-Editor (FAB) entscheiden: sichtbar lassen oder hinter Tastenkombination verstecken
- [ ] Mobile-Optimierung weiter prüfen (Insights-Bento-Grid, Contact-Form-Spacing falls noch holprig)
- [ ] `index-old.html` (ehemals Option 1) ggf. löschen oder weiterentwickeln

---

## Session-Log

### Session 5 — 2026-05-05 (Coming Soon, Articles, Legal, Mobile-Polish)

Großer Content- und Mobile-Push. 13 neue Dateien, ~1.500 LOC, mehrere Commits.

**Content-Erweiterung:**
- 4 Blog-Artikel mit realistischer Platzhalter-Prose: `blog-strategy.html`, `blog-communication.html`, `blog-performance.html`, `blog-endurance.html` (je 400–700 Wörter, Hero-Bild, Meta-Zeile, Subheadings, Blockquote, Autor-Block, CTA, Footer)
- 4 Insights-Artikel analog: `insight-decision-making.html`, `insight-silence.html`, `insight-executive-presence.html`, `insight-habit-excellence.html`
- Alle 8 Cards in `site.html` von `<article>`/`<div>` auf `<a href="…">` umgestellt → echte Links statt nicht-funktionaler `cursor-pointer`-Hovers
- `imprint.html` (DSGVO/§5 TMG-konformes Gerüst, gelb markierte Platzhalter mit Warnhinweis-Block)
- `privacy.html` (DSGVO-konform, deckt Web3Forms / Google Fonts / Tailwind CDN / GitHub Pages ab)
- Footer-Links Imprint/Privacy in allen 11 HTML-Dateien angepasst

**Coming Soon Page:**
- `index.html` umbenannt zu `site.html` (`git mv`, behält History)
- Neue `index.html` = Coming Soon Landing
- Animierter Hintergrund (4 Blobs + rotating conic + 10 Partikel + Noise — siehe Technische Entscheidungen)
- Eigenes Web3Forms-Form mit Coming-Soon-Subject
- Hidden-Login: 3×-Klick auf Logo → Modal → `Jan` / `comingsoon` → Redirect auf `site.html`
- In allen Artikel- und Legal-Seiten via `sed`: alle `href="index.html` → `href="site.html` (nav, back-buttons, CTAs, footer)

**Englisch-Audit komplett:**
- Blog-Cards (Strategie/Kommunikation/Leistung/Ausdauer + Headlines/Bodies) → Englisch
- Nav-Items, Editor-UI (10 Strings), Footer-Links, JS-Meldungen, Form-Subjects → alles Englisch
- Sichtbare Email-Anzeige `jan@kohler-coaching.com` → `office@jankohlercoaching.de`
- Studio "Berlin / London / Remote" → Office "Munich / Remote"
- `lang="de"` im `<html>`-Tag → `lang="en"`

**Mobile Polish:**
- Hero-Image Stack-Layout statt Vollbild-Overlay (kein Konflikt mit Gesicht mehr)
- `mt-[60px]` auf Bild-Container damit nicht von der fixen Nav verdeckt
- `object-position` für Mobile Feintuning (50% top — siehe `site.html:103`)
- Alle Sektionen `py-32` → `py-16 md:py-32` (halbiert Abstand auf Mobile)
- Section-Header `mb-20` → `mb-12 md:mb-20`, Footer `mt-32` → `mt-16 md:mt-32`
- Nav: kompakteres Padding + kleineres "Book a session" auf Mobile
- **Mobile Blog-Karussell**: Coverflow-3D mit Auto-Advance, Touch-Swipe, Dots, Prev/Next-Buttons (siehe Technische Entscheidungen). Desktop-Grid bleibt unverändert.

**Hinweise/Gotchas:**
- Coming-Soon-Login-Modal verwendet `position: fixed; inset: 0` mit Backdrop-Blur — funktioniert in allen modernen Browsern
- Web3Forms-Access-Key ist hardcoded und public — Web3Forms verifiziert serverseitig gegen die registrierte Domain (`jankohlercoaching.de`)
- Tailwind-CDN-Approach (kein Build-Step) macht es einfach, aber jede Seite muss die `tailwind.config` und Custom-CSS dupliziert enthalten
- `index-old.html` (alte Option 1, vor 03.05. das `index.html`) liegt noch im Repo, wird nicht weiterentwickelt — Entscheidung über Löschen/Weiterentwickeln steht aus

### Session 4 — 2026-05-03 (Mobile Hero Feintuning)
- Hero-Portrait auf Mobile hatte Kopf oben abgeschnitten — kein Atemraum über dem Haaransatz
- Fix: `.hero-img { object-position: 50% 5% }` → `50% top` (`site.html:103`)
- Desktop-Regel (`md:` breakpoint) unverändert bei `72% top`
- Direkt committed + gepusht — live auf jankohlercoaching.de

### Session 3 — 2026-05-03 (später)
- Custom Domain `jankohlercoaching.de` (Ionos) mit GitHub Pages verbunden
- Ionos: "Default Site"-Service deaktiviert → Park-Records (A/AAAA @) freigegeben
- Ionos DNS: 4× A @ (GitHub-IPs 108/109/110/111) + 1× CNAME www → michaherz.github.io
- Mail-Records (SPF/DKIM/DMARC/MX/Autodiscover) blieben komplett unangetastet
- `CNAME`-Datei im Repo angelegt mit Inhalt `jankohlercoaching.de`
- Pages-Settings via `gh api PUT` auf Custom Domain gesetzt
- HTTPS (Let's Encrypt) wird automatisch ausgestellt → `https_enforced` aktiviert

### Session 2 — 2026-05-03
- `index.html` (Option 1) umbenannt zu `index-old.html`
- `index2.html` umbenannt zu `index.html` → damit ist die aktive Version die GitHub-Pages-Startseite
- Git-Repo initialisiert (`.gitignore` für `.DS_Store` und `Website/`)
- Repo auf GitHub angelegt: https://github.com/michaherz/jan-kohler-website
- GitHub Pages aktiviert → **Live:** https://michaherz.github.io/jan-kohler-website/

### Session 1 — 2026-04-13/14
- `index.html` (Option 1) aus 3 separaten HTML-Snippets zusammengeführt
- `index2.html` (Option 2) aus einem vierten Snippet erstellt
- Inline-Editor in `index2.html` eingebaut (Text + Bilder editierbar, HTML-Export)
- Hero-Layout in `index2.html` von Overlay auf Split umgebaut, dann zurück auf Vollbild mit seitlicher Positionierung
- `portrait.jpeg`, `about.jpeg`, `insights.jpeg` lokal eingebunden (ersetzt verpixelte CDN-Bilder)
- GitHub-Pages-Setup erklärt
