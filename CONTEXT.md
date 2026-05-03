# Jan Kohler Website — Context & Session Log

## Projekt-Übersicht

Statische HTML-Website für Jan Kohler (Executive Coaching).
Gebaut mit: Tailwind CSS (CDN), Newsreader + Manrope (Google Fonts), Material Symbols.
Kein Framework, kein Build-Prozess — alles in einer einzigen HTML-Datei.

---

## Dateien im Ordner

| Datei | Beschreibung |
|---|---|
| `index.html` | **Aktive Version** (ehemals `index2.html`, umbenannt am 2026-05-03) |
| `index-old.html` | Option 1 — Multi-Section-Site, wird nicht weiterentwickelt (ehemals `index.html`) |
| `portrait.jpeg` | Hero-Bild — Jan Kohler Portrait (weißes Hemd, Studio) |
| `about.jpeg` | About-Sektion — Person von hinten, braune Jacke "Easy Yoke" |
| `insights.jpeg` | Insights-Feature-Bild — Gruppenphoto "Coach Catalyst Skill Lab" |
| `CONTEXT.md` | Diese Datei — Session-Log & technische Dokumentation |

---

## Sektionen in index.html

| ID | Sektion | Inhalt |
|---|---|---|
| `#home` | Hero | Vollbild-Portrait, Text links, Gradient-Overlay |
| `#about` | Über mich | Split-Layout: Text + Stats links, Bild rechts |
| `#insights` | Coaching | Bento-Grid mit Feature-Bild + Cards |
| `#blog` | Blog | 4-spaltige Karten-Grid mit versetztem Offset |
| `#contact` | Kontakt | Kontaktinfo links, Formular rechts |

---

## Technische Entscheidungen

### Hero-Layout
- **Problem:** Vollbild-Hintergrundbild → Text überlagert das Gesicht bei schmalem Browser.
- **Lösung:** Bild bleibt Vollbild (`absolute inset-0`), aber mit `object-position: 72% top` nach rechts verschoben. Text ist auf `max-w-lg` begrenzt (linke Seite). Gradient von links (opak) nach rechts (transparent) stellt Lesbarkeit sicher.
- Bild: `portrait.jpeg` (lokal), volle Auflösung, `grayscale`.

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

### Editor (eingebaut in index.html)
- Floating Pencil-Button (unten rechts) → aktiviert Bearbeitungsmodus.
- Alle Textelemente werden `contenteditable`.
- Bilder bekommen Hover-Overlay mit Kamera-Icon → Modal mit URL-Input oder Datei-Upload (base64).
- "HTML exportieren" → lädt saubere HTML-Datei ohne Editor-Code herunter.
- "Beenden" → verlässt den Bearbeitungsmodus.

### Scroll Spy
- JS-basiert, kein Framework.
- Aktiver Navlink wird per `data-section` und `window.scrollY` ermittelt.
- `scroll-mt-28` auf allen Sektionen verhindert, dass die fixe Nav den Inhalt überdeckt.

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
- [ ] **Hinweis:** Sichtbare Email im Kontaktblock steht auf `jan@kohler-coaching.com` (Template-Default) — ggf. auf `office@jankohlercoaching.de` anpassen, wenn das öffentlich gezeigt werden soll
- [ ] Restliche Placeholder-Bilder (Blog-Cards, Insights-Cards) durch echte Fotos ersetzen
- [ ] Sprach-Audit: Seite konsequent auf Englisch vereinheitlichen (aktuell gemischt: Blog-Cards DE, Rest EN)
- [ ] Deutsche Sprachversion hinzufügen (DE/EN-Umschalter)
- [ ] Mobile-Optimierung prüfen (Hero auf kleinen Screens)
- [ ] `index-old.html` (ehemals Option 1) ggf. löschen oder weiterentwickeln

---

## Session-Log

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
