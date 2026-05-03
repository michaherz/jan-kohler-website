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

- **Live-URL:** https://michaherz.github.io/jan-kohler-website/
- **Repo:** https://github.com/michaherz/jan-kohler-website (public)
- **Aktiviert:** 2026-05-03
- **Deploy:** automatisch bei Push auf `main`-Branch, Root-Verzeichnis
- **Build-Type:** legacy (Jekyll-default, aber ohne Jekyll-Konfig → reines Static-HTML)
- **HTTPS:** enforced

### Lokaler Workflow

```bash
cd ~/Desktop/jan-kohler-website
# Änderungen machen
git add .
git commit -m "beschreibung der änderung"
git push
# → nach ~1 min live
```

### Gitignore
- `.DS_Store`
- `Website/` (alter Archiv-Ordner)

---

## Offene Punkte / Ideen

- [x] ~~GitHub Pages live stellen~~ — erledigt 2026-05-03
- [ ] Restliche Placeholder-Bilder (Blog-Cards, Insights-Cards) durch echte Fotos ersetzen
- [ ] Kontaktformular mit Backend verbinden (z.B. Formspree — kostenlos, kein Server nötig)
- [ ] Mobile-Optimierung prüfen (Hero auf kleinen Screens)
- [ ] `index-old.html` (ehemals Option 1) ggf. löschen oder weiterentwickeln

---

## Session-Log

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
