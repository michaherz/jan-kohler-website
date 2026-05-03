# Jan Kohler Website — Context & Session Log

## Projekt-Übersicht

Statische HTML-Website für Jan Kohler (Executive Coaching).
Gebaut mit: Tailwind CSS (CDN), Newsreader + Manrope (Google Fonts), Material Symbols.
Kein Framework, kein Build-Prozess — alles in einer einzigen HTML-Datei.

---

## Dateien im Ordner

| Datei | Beschreibung |
|---|---|
| `index.html` | Option 1 — Multi-Section-Site (aus 3 Snippets zusammengeführt) |
| `index2.html` | Option 2 — Aktive Version, wird weiterentwickelt |
| `portrait.jpeg` | Hero-Bild — Jan Kohler Portrait (weißes Hemd, Studio) |
| `about.jpeg` | About-Sektion — Person von hinten, braune Jacke "Easy Yoke" |
| `insights.jpeg` | Insights-Feature-Bild — Gruppenphoto "Coach Catalyst Skill Lab" |
| `CONTEXT.md` | Diese Datei — Session-Log & technische Dokumentation |

---

## Sektionen in index2.html

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

### Editor (eingebaut in index2.html)
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

- Repository: noch anzulegen (Stand: Session 1)
- Geplante URL: `https://GITHUB-USERNAME.github.io/jan-kohler-website/`
- Setup-Schritte: siehe Chat-Log vom 2026-04-14

```bash
# Im Website-Ordner:
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/USERNAME/jan-kohler-website.git
git branch -M main
git push -u origin main
# Dann: GitHub → Settings → Pages → Branch: main / root → Save
```

---

## Offene Punkte / Ideen

- [ ] GitHub Pages live stellen
- [ ] Restliche Placeholder-Bilder (Blog-Cards, Insights-Cards) durch echte Fotos ersetzen
- [ ] Kontaktformular mit Backend verbinden (z.B. Formspree — kostenlos, kein Server nötig)
- [ ] Mobile-Optimierung prüfen (Hero auf kleinen Screens)
- [ ] Option 1 (`index.html`) ggf. verwerfen oder weiterentwickeln

---

## Session-Log

### Session 1 — 2026-04-13/14
- `index.html` (Option 1) aus 3 separaten HTML-Snippets zusammengeführt
- `index2.html` (Option 2) aus einem vierten Snippet erstellt
- Inline-Editor in `index2.html` eingebaut (Text + Bilder editierbar, HTML-Export)
- Hero-Layout in `index2.html` von Overlay auf Split umgebaut, dann zurück auf Vollbild mit seitlicher Positionierung
- `portrait.jpeg`, `about.jpeg`, `insights.jpeg` lokal eingebunden (ersetzt verpixelte CDN-Bilder)
- GitHub-Pages-Setup erklärt
