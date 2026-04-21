# WARP — Project Page

Static one-page site for _Weight Teleportation for Attack-Resilient Unlearning Protocols_ (ICLR 2026).

## Structure

```
warp_project_page/
├── index.html                # the whole page (self-contained)
├── README.md
└── assets/
    ├── favicon.svg
    ├── imperial.png          # Imperial College logo
    ├── dartmouth.png         # Dartmouth College logo
    ├── fig_problem.png       # Fig 1 – threat model
    ├── gradient_norm.png     # Fig 2 – privacy risk vs. gradient norm
    ├── fig_methodology.png   # Fig 3 – WARP 3-panel methodology
    ├── warp_objective.png    # LaTeX-rendered WARP objective equation
    └── reconstructions.jpg   # Fig 4 – reconstruction comparison
```

No build step, no framework. Fully self-contained except for Google Fonts (graceful system-font fallback).

---

## 🚀 Deploying to GitHub Pages

Pick **Option A** — it's the easiest.

### Option A — `docs/` folder on `main` (recommended)

Makes the page appear at `https://<user>.github.io/WARP_Unlearning/`.

```bash
# 1. From the root of the WARP_Unlearning repo
cd /path/to/WARP_Unlearning

# 2. Copy the project-page folder into a new docs/ directory
mkdir -p docs
cp -r /path/to/warp_project_page/* docs/

# 3. Commit and push
git add docs/
git commit -m "Add project page"
git push origin main
```

Then on github.com:

1. Go to the repo → **Settings** → **Pages** (left sidebar).
2. Under **Build and deployment**:
   - **Source**: `Deploy from a branch`
   - **Branch**: `main` · **Folder**: `/docs`
   - Click **Save**.
3. Wait ~1 min. URL appears at the top of the Pages settings when live.

### Option B — dedicated `gh-pages` branch

Keeps the page completely out of `main`.

```bash
cd /path/to/WARP_Unlearning

git checkout --orphan gh-pages
git rm -rf .                               # clears the working tree on this branch
cp -r /path/to/warp_project_page/* .

git add .
git commit -m "Project page"
git push origin gh-pages
git checkout main
```

Then → **Settings** → **Pages** → Branch `gh-pages`, folder `/ (root)`, **Save**.

### Option C — separate project-page repo

For a cleaner URL (e.g., `https://mammadmaheri7.github.io/warp/`):

```bash
# Create empty repo at github.com/<user>/warp first, then:
git clone git@github.com:<user>/warp.git
cd warp
cp -r /path/to/warp_project_page/* .
git add . && git commit -m "Initial page" && git push
# Enable Pages in Settings → Pages → main, / (root)
```

### Custom domain (optional)

Add a file named exactly `CNAME` (no extension) in the page root, containing your domain on one line:
```
warp.example.com
```
Then set a DNS `CNAME` record pointing at `<user>.github.io`. HTTPS auto-provisions in ~15 min.

---

## 🛠 Local preview

```bash
cd warp_project_page
python3 -m http.server 8000
# open http://localhost:8000
```

Opening `index.html` directly via `file://` also works.

---

## ✏️ Customize

### 1. Links in the hero

Search `class="btn"` in `index.html`. Replace placeholder `href="#"`:

| Button     | Replace with                                       |
|------------|----------------------------------------------------|
| Paper      | arXiv URL once the paper is posted                 |
| OpenReview | `https://openreview.net/forum?id=…`                |
| Code       | ✅ already set to `github.com/mammadmaheri7/WARP_Unlearning` |
| Poster     | Link to your poster PDF                            |
| Slides     | Google Slides / PDF                                |

Author links are placeholders (`href="#"`) — point them to each author's homepage.

### 2. Re-rendering the WARP objective equation

The equation is `assets/warp_objective.png`, rendered from LaTeX. To regenerate:

```latex
% eq.tex
\documentclass[preview,border=10pt]{standalone}
\usepackage[paperwidth=50cm,paperheight=6cm]{geometry}
\usepackage{amsmath,amssymb,mathtools}
\begin{document}
$\displaystyle g^{\star} \in \arg\min_{g \in \mathcal{G}}\;
\underbrace{\sum_{(x,y)\in \mathcal{D}_f}\bigl\|\nabla_{\theta}\ell(g\cdot\theta)\bigr\|_2^2}_{\text{shrink forget gradients}}
\;-\;\beta\;
\underbrace{\bigl\|g\cdot\theta-\theta\bigr\|_2^2}_{\text{disperse parameters}}
\quad\text{s.t.}\quad \ell_r(g\cdot\theta)\leq \ell_r(\theta)+\varepsilon$
\end{document}
```

```bash
pdflatex eq.tex
pdftoppm -png -r 300 eq.pdf warp_objective
mv warp_objective-1.png assets/warp_objective.png
```

### 3. BibTeX

Update the `@inproceedings` block at the bottom of `index.html` once ICLR 2026 proceedings are live (add `editor`, `pages`, `publisher`, etc.).

---

## 🎨 Design tokens

CSS variables at the top of `<style>`:

```css
--hot:       #C72124;   /* WARP red — highlighted letters, accents */
--ink:       #111217;   /* body text */
--imperial:  #0000CD;   /* Imperial blue */
--dartmouth: #00693E;   /* Dartmouth green */
--bg-alt:    #F7F7F9;   /* card background */
```

Fonts: **Inter** (body) + **Source Serif 4** (display / italic) + **JetBrains Mono** (code), loaded from Google Fonts with system-font fallback.

---

## License

Page template: reuse freely. Paper content (figures, text) follows the paper's license.
