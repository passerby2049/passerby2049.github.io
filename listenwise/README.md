# ListenWise landing page

Static site. `index.html` + `styles.css` + `screenshots/` + `.nojekyll`. No
build step. All asset paths are relative so it works unchanged at any host
path.

## Preview locally

Do **not** `open index.html` directly — `file://` blocks some resources. Serve
over HTTP:

```bash
cd landing
python3 -m http.server 8000
```

Then open http://localhost:8000.

## Deploy to GitHub Pages

The main `ListenWise` repo is **private**, so Pages on it requires GitHub Pro.
Cleanest free path is a separate public repo.

### Option A — dedicated public repo (recommended)

```bash
# One-time setup
cd landing
git init
git add .
git commit -m "init landing"
gh repo create listenwise-landing --public --source=. --push
gh api -X POST repos/passerby2049/listenwise-landing/pages \
  -f 'source[branch]=main' -f 'source[path]=/'
```

Site is then at `https://passerby2049.github.io/listenwise-landing/`. Custom
domain via `CNAME` file + DNS.

### Option B — `docs/` on this repo (needs GitHub Pro since repo is private)

```bash
cp -R landing docs
git add docs && git commit -m "add landing for Pages"
# Then: Settings → Pages → Source = main / /docs
```

### Option C — Vercel / Cloudflare Pages / Netlify

All have free tiers that work fine on a **private** repo. Point them at
`landing/` as the site root, no build command needed.

## Download button

The `Download for macOS` buttons currently point at `#download` (scroll
anchor). When you have a real DMG URL, edit the two `href="#download"` and
`href="#"` attributes in `index.html`.

Host the DMG somewhere **public**:

- Public GitHub Releases on a separate public repo (e.g. `listenwise-releases`)
- Cloudflare R2 / Backblaze B2 / S3 with a public bucket
- Any CDN

⚠️ Do **not** link to releases on a private repo — unauthenticated visitors
get a 404.

## Editing

- Copy lives in `index.html`. Search for the section.
- Styling in `styles.css`. CSS custom properties at `:root` drive the scale —
  `--max-w`, `--fg`/`--fg-2`/`--fg-3`, `--accent`, `--fw-medium` etc.
- Screenshots in `screenshots/`. Capture with `screencapture -x -R x,y,w,h
  file.png` (omit the menu bar), downsize with
  `sips -s format png --resampleWidth 1920 file.png --out file.png`.

## Domain

`listenwise.app` is the short one. Alternatives: `listenwise.co`,
`listenwise.dev`.
