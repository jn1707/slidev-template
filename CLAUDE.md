# CLAUDE.md — Slidev presentation repo

Guidance for any agent working in this repository. Read this before touching anything.

---

## 1. Privacy — read this first

This working directory is a **local scratch area holding unpublished research talks**, but its
git remote is **public**. Only generic tooling is published. **No presentation is ever
published, including finished and already-public ones.**

**Rules, no exceptions:**

- **Never run `git add .`, `git add -A`, or `git commit -a`.** Stage files by explicit path only.
- **Never `git push` unless the user asks in that turn.** Approval to commit is not approval to push.
- The root `.gitignore` is **deny-by-default**: every top-level entry is ignored and only an
  explicit allowlist is tracked. **Never add a deck to that allowlist.** If the user asks to
  publish one, confirm it in that turn and review the whole directory first — draft plots,
  internal numbers, co-authors' photos and contact details hide in `public/images/`.
- **Nothing tracked may contain personal information**: no names, email addresses, affiliations
  written as fact, absolute home paths, or photos of people. Use placeholders in every tracked
  file. The user's real defaults live in `CLAUDE.local.md`, which is gitignored.
- Exported `*.pdf` / `*.pptx` are ignored everywhere. Keep it that way.

**Tracked (the allowlist):** `template/`, `VerticalSlides/`, `shared-assets/`, `tools/`,
`Slidev Example Presentation/`, plus `README.md`, `CLAUDE.md`, `.gitignore`.

**Everything else is private**, including `slide-library/`, `Old Presentations/`, and every
`YYMMDD-…` deck directory.

## 2. Layout

```
Slidev/
├── CLAUDE.md                  # this file
├── README.md                  # human-facing setup + Slidev cookbook
├── template/                  # ← copy this to start a new deck
├── shared-assets/logos/       # institutional logos
├── tools/slide_library.py     # index + copy helper for the slide library
├── VerticalSlides/            # minimal demo of the VerticalSlides component
├── Slidev Example Presentation/  # upstream Slidev feature showcase
│
│   # ── private, gitignored ──────────────────────────────
├── CLAUDE.local.md            # the user's real name, venue defaults, style reference
├── slide-library/             # reusable slides, by topic, newest first (§8)
├── Old Presentations/         # archive — read for reference
└── <YYMMDD-Topic-Venue>/      # active decks
```

Deck directories are named `YYMMDD-Topic-Venue`. Dates are `YYMMDD`. Keep that pattern.

---

## 3. Starting a new deck

From the repo root:

```bash
cp -r template 260911-MyTopic-MyVenue
cd 260911-MyTopic-MyVenue
pnpm install          # resolves Slidev fresh — see §7
pnpm dev
```

The template already ships `public/logos/` and `components/VerticalSlides.vue`.
Do not symlink `public/` into `shared-assets/`; Vite refuses to serve through a symlink
pointing outside the project root and images silently fail to load. Copy instead.

---

## 4. Building a deck from source material

The intended workflow: the user drops a source document in the deck root and images in
`public/images/`, then asks for a deck. Follow this procedure.

1. **Read the inputs.** The source is a `.md`, `.tex`, or paper draft in the deck root.
   Run `ls public/images/` and treat that as the full inventory — never reference an image
   that is not on disk, and never invent a filename.
2. **Propose an outline first.** A short numbered list of slide titles and what each one
   carries. Get a yes before writing 500 lines of markdown.
3. **Write `slides.md`** following §5 exactly.
4. **Verify it compiles.** Run `pnpm dev` in the background and confirm the server starts
   without a Vue compile error. An unbalanced `<div>` in one slide breaks that slide only,
   so also skim the output for template errors.
5. **Report what you left out** and which images went unused.

Target density: one idea per slide, a headline that states the finding, and a visual or an
equation that proves it. The user presents to physicists — do not dumb down the math,
but do not put a derivation on a slide without an accompanying takeaway box.

---

## 5. House style

The house-style reference deck is named in `CLAUDE.local.md`. Read that deck's `slides.md`
before writing a new one and match it. It is a **private** deck: read it, never publish it,
and never copy its author details into a tracked file.

### Accent colour

Each deck picks **one accent hex** and uses it for every heading, rule, border, and the slide
number. A second colour appears only to contrast two ideas side by side.

Two palettes are in use across existing decks:

| Palette | Accent | Secondary | Card backgrounds |
|---|---|---|---|
| Blue | `#004aad` | `#9f1267` | `#f2f7ff` / `#fdf2f8` |
| Dark red | `#8B0000` | — | `#fff5f5` |

### Every content slide

```md
---
layout: default
transition: fade-out
---

# lowercase headline that states the finding

<div class="mt-4">

Content.

</div>

<div style="position: absolute; bottom: 1em; right: 1.5em; font-size: 1em; color: #004aad; opacity: 0.7;">
    <SlideCurrentNo />
</div>

<style>
h1 { font-weight: 900; color: #004aad; }
</style>
```

- **Headlines are lowercase and declarative.** "from the ratio to latent space geometry",
  "multiclass CWoLa works in practice" — not "Results" or "Method Overview". The headline is
  the sentence the audience should remember. Proper nouns and acronyms keep their capitals.
- `transition: fade-out` on every slide.
- The slide-number div and the `<style>` block are repeated per slide. Yes, it is verbose;
  it is also what the existing decks do. Keep it consistent rather than clever.

### Title slide

Logos are absolutely positioned with `v-drag`, and their coordinates live in the title
slide's frontmatter under `dragPos`. Use root-absolute `src` paths (`/logos/…`), which resolve
against `public/`.

```md
---
layout: default
class: text-left
transition: fade-out
dragPos:
  KU: 13,13,159,57
  EU: 188,12,219,52
  AIPHY: 865,2,118,83
---

<div class="mt-16">

# Talk Title

### <em>Full Paper Title</em> (<a href="https://arxiv.org/abs/…">arXiv:…</a>)

</div>

<div class="mt-10">

**Date:** DD Month YYYY

**Venue:** Conference or meeting name

**By:** <speaker> — <institution>

Co-authors: …

</div>

<img v-drag="'KU'" src="/logos/ku_logo.png"/>
<img v-drag="'EU'" src="/logos/EU.png"/>
<img v-drag="'AIPHY'" src="/logos/AIPHY.svg"/>
```

`dragPos` values are `x,y,width,height` in slide coordinates and are written by Slidev itself
when the user drags an element in dev mode. **Never hand-tune them** — if a logo is misplaced,
say so and let the user drag it.

### Two-column concept cards

The signature layout. Second card reveals on click.

```html
<div class="mt-4" style="display:grid; grid-template-columns: 1fr 1fr; gap: 1.5rem;">

<div style="border:2px solid #004aad; border-radius:12px; padding:1.3rem; background:linear-gradient(180deg, #f2f7ff 0%, #ffffff 100%);">
<h3 style="color:#004aad; margin-top:0;">First idea</h3>
<p class="text-base">…</p>
</div>

<div v-click style="border:2px solid #9f1267; border-radius:12px; padding:1.3rem; background:linear-gradient(180deg, #fdf2f8 0%, #ffffff 100%);">
<h3 style="color:#9f1267; margin-top:0;">Contrasting idea</h3>
<p class="text-base">…</p>
</div>

</div>
```

### Takeaway box

```html
<div v-click class="mt-8">
<div style="background: linear-gradient(90deg, #f2f7ff 0%, #f8f9fa 100%); border: 2px solid #004aad; border-radius: 12px; padding: 1.2rem 1.5rem;">
<b style="color:#004aad;">The one-sentence conclusion.</b>
Supporting clause.
<span class="text-sm opacity-70">(Metodiev, Nachman &amp; Thaler, <a href="https://arxiv.org/abs/1708.02949">arXiv:1708.02949</a>)</span>
</div>
</div>
```

### Figures

```html
<div style="display: flex; justify-content: center; align-items: center; margin-top: 1.5rem;">
  <img src="/images/simplex_recovery.png" alt="…"
       style="max-width: 92%; max-height: 380px; border-radius: 12px; box-shadow: 0 4px 24px rgba(0,74,173,0.12); object-fit: contain;" />
</div>
```

The shadow colour is the accent at 12% alpha. Always write a real `alt` describing the physics.

### Math

KaTeX is built in. **Leave a blank line before and after a `$$` block** or it will not render.
Inside a bordered box that also holds math, blank lines create unwanted paragraph gaps — add a
deck-level `style.css` with `.math-note p { margin: 0 !important; line-height: 1.45; }` and give
the box `class="math-note"` (see `Old Presentations/260616-InelasticityRFM-IceCube/style.css`).

### Markdown inside HTML

Markdown is only parsed inside an HTML block if there is a **blank line after the opening tag
and before the closing tag**. This is the single most common failure mode in these decks:

```html
<div class="mt-4">

**This renders as bold.**

</div>
```

Corollary: **wrap that content in `<div>`, never `<p>`.** markdown-it emits its own `<p>` for
blank-line-separated text, so a literal `<p>` around it nests `<p>` inside `<p>` and Vue warns
about the stray `</p>` at build time. Use `<p>` only for a single line with no blank lines in it.

### Utilities

UnoCSS is available: `mt-4`, `mt-16`, `text-base`, `text-sm`, `opacity-70`, `text-center`.
Prefer these over inline `style` for spacing; use inline `style` for the colour-bearing
decoration above, matching what the existing decks do.

### Closing slide

`layout: center`, `class: text-center`. Title, name, institution, email, then a QR code next
to the arXiv ID, then "Questions?". Copy the block from the reference deck named in
`CLAUDE.local.md`; take the speaker details from there too, never from a tracked file.

---

## 6. VerticalSlides — the depth-on-demand pattern

`components/VerticalSlides.vue` lets one horizontal slide hold a stack of sub-slides. The
speaker moves right for the fast path and **down** (arrow keys) to go deeper if the audience
asks. Slidev's own slide index does not change, so the main flow stays short.

```md
# Architecture

<VerticalSlides :count="2" style="height: calc(100% - 2.8rem); margin-top: 0.3rem;">

<div class="w-full h-full" style="display: grid; grid-template-columns: 1.1fr 0.9fr; gap: 1rem;">
… sub-slide 1 …
</div>

<div class="w-full h-full" style="display: grid; grid-template-columns: 1.1fr 0.9fr; gap: 1rem;">
… sub-slide 2 …
</div>

</VerticalSlides>
```

Rules:

- `:count` **must equal** the number of direct children. The component sizes the track from it;
  a mismatch silently crops or leaves blank space.
- Always pass `style="height: calc(100% - 2.8rem); margin-top: 0.3rem;"` so the stack fits
  under the `h1` without overflowing.
- Each direct child should be a single element that fills its page (`class="w-full h-full"`).
- Put the slide-number div and `<style>` block **outside** the `<VerticalSlides>` wrapper.
- Add a small hint on a sub-slide that has more below it, e.g.
  `<div style="font-size: 0.78em; opacity: 0.7; text-align: right;">↓ why we moved away</div>`.

The canonical version of the component is `template/components/VerticalSlides.vue`. The copy in
the top-level `VerticalSlides/` demo deck is an older, simpler implementation — prefer the
template one.

Working reference with six real stacks: `Old Presentations/260616-InelasticityRFM-IceCube/slides.md` (private).

---

## 7. Slidev version policy

The template must stay **version-independent**, so a deck created today gets today's Slidev.

- `template/package.json` specifies `latest` for `@slidev/cli`, `@slidev/theme-default`
  and `shiki`. A fresh `pnpm install` in a newly copied deck therefore resolves the newest
  release.
- **`template/` must never contain a `pnpm-lock.yaml`.** It is gitignored there on purpose.
  A lockfile in the template would freeze every future deck to today's versions.
- Once a deck is installed, its own `pnpm-lock.yaml` pins it. Leave that file alone so the
  deck can still be re-exported months later. Deck lockfiles are gitignored along with the
  decks themselves.
- To move an existing deck forward: `pnpm run update` (`pnpm up --latest`), then `pnpm dev`
  and check the deck still renders before anything else.
- If Slidev has had a major bump and something breaks, fix the deck — do not pin the template
  backwards without asking.

---

## 8. Slide library

`slide-library/` collects slides worth reusing, organised by topic and newest-first, so the
user does not have to remember which old talk had the good version of a figure.

**It is gitignored and stays that way.** It holds the user's real slides; only the generic
helper in `tools/` is published. `tools/slide_library.py index` creates the directory on first
run if it does not exist.

### Reusing a slide

**Slidev's `src:` include only resolves paths inside the deck directory.** A `../` path
pointing at `slide-library/` is dropped silently — no warning, no error, the slide just
never appears. Verified against Slidev 52.19.1. So the slide must be copied in:

```bash
cd 260911-MyTopic-MyVenue
python3 ../tools/slide_library.py use <topic>/<slug>.md .
```

That copies the slide to `pages/<slug>.md`, copies its images into `public/`, and prints the
block to paste into `slides.md`:

```md
---
src: ./pages/<slug>.md
---
```

Afterwards, retune the accent colour in the pulled slide to match the new deck.

### Archiving a slide after a talk

When the user says "add slides 7 and 12 to the library":

1. Copy the slide blocks verbatim into `slide-library/<topic>/<YYMMDD>-<slug>.md`.
2. Add a `library:` key to the first slide's frontmatter (Slidev ignores unknown keys):

   ```yaml
   ---
   layout: default
   transition: fade-out
   library:
     title: simplex recovery — two routes
     topic: cwola
     created: 2026-08-27
     source: 260827-MyTopic-MyVenue
     tags: [cwola, simplex, method]
     assets: [images/simplex_recovery.png]
   ---
   ```

3. Copy every referenced image into `slide-library/_assets/<topic>/<slug>/` preserving the
   `images/…` path suffix.
4. Run `python3 tools/slide_library.py index` to regenerate `slide-library/INDEX.md`.

**Only archive slides the user named.** Do not decide on their behalf that a slide was good.

---

## 9. Export

```bash
pnpm export                      # PDF → slides-export.pdf
pnpm export --format pptx        # PPTX (images only, not editable)
pnpm export --range 1-10
```

Needs `playwright-chromium` to have actually installed its browser, which requires
`pnpm-workspace.yaml` to be present at install time (§10). If export still fails,
`npx playwright install chromium`.
Exports are gitignored. Rename the PDF to `<deck-name>.pdf` when handing it over.

---

## 10. Gotchas

- **Port in use:** `lsof -ti:3030 | xargs kill -9`, or `pnpm dev --port 3031`.
- **Images not loading:** they must be under `public/`, referenced as `/images/…` (root-absolute).
  Some older decks use `./images/…`; both work in dev, root-absolute is safer under `--base`.
- **An included slide is missing from the build:** `src:` pointed outside the deck
  directory. Slidev drops those silently. Copy the file in instead (§8).
- **Blank sub-slide in a stack:** `:count` does not match the number of children.
- **`UNRESOLVED_IMPORT: Could not resolve '/images/x.png'` and the build dies:** the file is
  not in `public/`. A missing image is a hard build failure, not a warning, and it breaks
  `pnpm export` too. Never reference an image before it exists on disk.
- **Math not rendering:** missing blank line around `$$`.
- **`Ignored build scripts: playwright-chromium` on install:** the deck is missing
  `pnpm-workspace.yaml`. pnpm 12 renamed this setting from `onlyBuiltDependencies` to
  `allowBuilds`; the old key is silently ignored. Copy the file from `template/`,
  then `rm -rf node_modules pnpm-lock.yaml && pnpm install`.
- **Vue warns about `</p>` at build time:** markdown wrapped in `<p>` instead of `<div>` (§5).
- **A slide renders as raw HTML source:** missing blank line after the opening tag.
- `.DS_Store` files are everywhere and gitignored. Ignore them.
- Each deck under `Old Presentations/` is its own npm project with its own `node_modules`.
  To open one, `cd` into that deck directory and run `pnpm install && pnpm dev` there.
