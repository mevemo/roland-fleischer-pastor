# roland-fleischer-pastor.de

Statische Webseite für Roland Fleischer (Pastor & Historiker), gebaut mit [Zola](https://www.getzola.org).

---

## Was du brauchst

1. **Zola installieren** — der Static Site Generator selbst.
   - macOS: `brew install zola`
   - Windows: `winget install --id getzola.zola` oder über [Scoop/Chocolatey](https://www.getzola.org/documentation/getting-started/installation/)
   - Linux: über den Paketmanager oder Binary von [github.com/getzola/zola/releases](https://github.com/getzola/zola/releases)
   - Prüfen mit: `zola --version` (sollte ≥ 0.19 sein)

2. **Ein Texteditor** für Markdown — VS Code, Sublime, Obsidian, was du magst.

3. **Optional: Git** für Versionierung.

4. **Hosting** — siehe Abschnitt unten. Empfehlung: Cloudflare Pages oder Netlify, beide kostenlos und unschlagbar einfach für statische Seiten.

---

## Lokal entwickeln

Im Projektordner:

```bash
zola serve
```

Öffnet einen lokalen Server unter `http://127.0.0.1:1111` mit Live-Reload — Änderungen an Markdown, Templates oder SCSS werden sofort im Browser sichtbar.

Zum Beenden: `Ctrl+C`.

---

## Bauen für Produktion

```bash
zola build
```

Erzeugt den Ordner `public/` mit der fertigen statischen Seite. Genau dieser Ordner wird auf den Webserver geladen.

---

## Inhalt bearbeiten

Alle Inhalte liegen in `content/`:

- `_index.md` — die Startseite (das ist die Hauptseite, die ihr für Schwiegervater gebaut habt)
- `impressum.md` — bitte Adresse ergänzen!
- `datenschutz.md` — Standardtext, ggf. anpassen

Markdown ist dasselbe wie in eurem ursprünglichen WordPress-Export. Überschriften mit `##`, Links mit `[Text](URL)`, etc.

### Frontmatter

Oben in jeder Datei steht ein TOML-Block zwischen `+++`. Den nicht löschen — er steuert Titel, Template, etc.

---

## Design ändern

- **Farben & Schriftarten**: `sass/main.scss`, ganz oben im `:root`-Block. Alle Farben sind als CSS-Variablen definiert, einmal ändern reicht.
- **Layout der Startseite**: `templates/index.html`
- **Header/Footer**: `templates/base.html`

Beim nächsten `zola serve` oder `zola build` wird das SCSS automatisch zu CSS kompiliert.

---

## Bilder

Alles in `static/` wird 1:1 übernommen. Bilder gehören in `static/images/`.

Im Markdown referenziert: `![Beschreibung](/images/dateiname.png)`

---

## Deployment

### Option A: Cloudflare Pages (empfohlen — kostenlos, schnell, kein Bullshit)

1. Code in ein Git-Repo schieben (GitHub, GitLab, Codeberg)
2. Bei Cloudflare Pages → "Create project" → Repo verbinden
3. Build-Einstellungen:
   - **Build command**: `zola build`
   - **Build output directory**: `public`
   - **Environment variable**: `ZOLA_VERSION` = `0.19.2` (oder neuer)
4. Deploy. Bei jedem Git-Push baut sich die Seite automatisch neu.
5. Eigene Domain in den Cloudflare-Pages-Einstellungen verknüpfen.

### Option B: Netlify

Sehr ähnlich. `netlify.toml` im Projekt-Root anlegen:

```toml
[build]
publish = "public"
command = "zola build"

[build.environment]
ZOLA_VERSION = "0.19.2"
```

### Option C: Klassisches Webhosting per FTP

`zola build` lokal ausführen, dann den Inhalt von `public/` per FTP/SFTP auf den Webspace laden. Funktioniert auch bei jedem 0815-Hoster.

---

## Was noch zu erledigen ist

- [ ] **Impressum**: Adresse in `content/impressum.md` ergänzen — rechtlich Pflicht in Deutschland
- [ ] **Favicon**: Eine `favicon.png` (z.B. 512×512) in `static/images/` legen
- [ ] **Domain**: DNS auf den neuen Hoster zeigen lassen
- [ ] **Alte WordPress-PDFs**: Die Links zu `roland-fleischer-pastor.de/wp-content/uploads/...` zeigen aktuell noch auf die alte WordPress-Installation. Wenn die abgeschaltet wird, müssen die PDFs in `static/pdf/` gelegt und die Links in `content/_index.md` angepasst werden.

---

## Struktur im Überblick

```
zola-site/
├── config.toml              # Globale Konfiguration
├── content/
│   ├── _index.md            # Startseite
│   ├── impressum.md
│   └── datenschutz.md
├── templates/
│   ├── base.html            # Grundgerüst (Header, Footer)
│   ├── index.html           # Startseiten-Template
│   └── page.html            # Template für Unterseiten
├── sass/
│   └── main.scss            # Alle Stile
├── static/
│   └── images/
│       └── portrait-roland-fleischer.png
└── public/                  # Wird von `zola build` erzeugt — nicht eigenhändig editieren
```
