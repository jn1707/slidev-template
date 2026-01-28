# Slidev Presentations

A collection of scientific presentations built with [Slidev](https://sli.dev/).

## Quick Start

### Prerequisites
- Node.js (v18 or higher)
- pnpm (recommended) or npm

### Starting a Presentation

```bash
# Navigate to a presentation folder
cd "Geneva presentation"

# Install dependencies (first time only)
pnpm install

# Start the dev server
pnpm dev
```

The presentation will open at `http://localhost:3030` (or similar).

### Creating a New Presentation

```bash
# Copy the template
cp -r template "My New Presentation"
cd "My New Presentation"

# Copy logos or any other items frequeuntly used
cp .-R ./shared-assets/logos ./public/logos

# Install dependencies
pnpm install

# Start editing slides.md
pnpm dev
```

## Exporting Presentations

### Export to PDF

```bash
# Build and export to PDF
pnpm export

# Or specify output name
pnpm export --output my-presentation.pdf
```

The PDF will be created in the `./slides-export.pdf` (or your specified name).

### Export to PowerPoint (PPTX)
This will export the slides as images. Later editing in PPTX is not possible.
```bash
# Export to PPTX
pnpm export --format pptx

# Or with custom name
pnpm export --format pptx --output my-presentation.pptx
```

**Note:** PPTX export requires playwright. Install it if needed:
```bash
pnpm add -D playwright-chromium
npx playwright install chromium
```

### Export Options

```bash
# Export with notes
pnpm export --with-clicks

# Export dark theme
pnpm export --dark

# Export specific slides (e.g., slides 1-10)
pnpm export --range 1-10
```

## Project Structure

```
Slidev/
├── README.md                   # This file
├── template/                   # Clean template for new presentations
│   ├── slides.md               # Your presentation content
│   ├── package.json            # Dependencies
│   ├── components/             # Custom Vue components
│   └── public/                 # Static assets (images, videos, etc.)
│       └── images/
├── shared-assets/              # Often reused assets
├── Slidev Example Presentation/# Example with many features
└── Old Presentations/           
```

## Managing Images and Assets

### Recommended Approach

**Option 1: Per-Presentation Assets (Current)**
- Keep images in `./public/images/` within each presentation folder
- Reference in slides: `./images/your-image.png`
- ✅ Self-contained presentations
- ✅ Easy to share individual presentations
- ❌ Duplicated assets across presentations

**Option 2: Shared Assets Library (Currently not working)**

Directly reference shared assets folder for reusable images:
```
Slidev/
├── shared-assets/
│   ├── logos/           # Institutional logos
│   ├── diagrams/        # Reusable scientific diagrams
│   ├── backgrounds/
│   └── common/          # Other frequently used images
```

#### Symlinking Images to a Presentation

**Option A: Symlink entire shared folders**
```bash
# In your presentation folder
cd "My New Presentation"

# Create symlink to shared logos
ln -s ../../shared-assets/logos public/logos

# Now reference in slides.md as:
# <img src="./logos/ku_logo.png" />
```

**Warning about symlinks outside the presentation folder:**

- Some dev servers (Vite / Slidev) restrict serving files that are reached via `public/` symlinks when the symlink target is located outside the current presentation folder. This can cause images not to load during `pnpm dev` even though the symlink exists.
- Safer, more portable option: copy the shared assets into the presentation `public/` folder so the files are inside the project root. Example:

```bash
# From the presentation root
cp -R ../../shared-assets/logos public/logos
```

This avoids dev-server restrictions and ensures exports (PDF/PPTX) include the expected assets.

#### Verifying Symlinks

```bash
# Check if symlinks are working
ls -la public/logos          # Shows symlinked folders
ls -la public/images/*.png   # Shows symlinked files

# Follow symlinks to see actual files
readlink public/logos/ku_logo.png
```

#### Benefits of Symlinking
- ✅ No duplicate files (saves disk space)
- ✅ Update once, reflects everywhere
- ✅ Easy to share presentations (symlinks convert to actual files when exported)
- ✅ Keep related presentations synchronized

## Reusing Slides Between Presentations

### Method 1: Copy-Paste Markdown
Simply copy slide blocks from one `slides.md` to another:
```markdown
---
# Your slide separator
---

# Slide Title
Content here
```

### Method 2: Slidev Includes (Experimental)
Create reusable slide snippets:
```markdown
<!-- In shared-slides/icecube-detector.md -->
---
# The IceCube Detector
...
---
```

```markdown
<!-- In your slides.md -->
@include: ../shared-slides/icecube-detector.md
```

### Method 3: Component Library
Extract complex slides to Vue components in `components/`:
```vue
<!-- components/IceCubeDetectorSlide.vue -->
<template>  
  <div>
    <h1>The IceCube Detector</h1>
    <!-- ... -->
  </div>
</template>
```

Use in slides:
```markdown
---
<IceCubeDetectorSlide />
---
```

## Version Control with GitHub

### Recommended Setup

```bash
# Initialize Git in Slidev root (if not already)
cd /path/to/Slidev
git init

# Create .gitignore
cat > .gitignore << EOF
# Dependencies
node_modules/
.pnpm-store/

# Build outputs
dist/
.slidev/
*.pdf
*.pptx

# OS files
.DS_Store
Thumbs.db

# Editor
.vscode/
.idea/

# Environment
.env
.env.local
EOF

# Add and commit
git add .
git commit -m "Initial commit: Slidev presentations"

# Push to GitHub
git remote add origin https://github.com/YOUR_USERNAME/slidev-presentations.git
git push -u origin main
```

### Hosting slides with GitHub Pages

You can host a built Slidev presentation as a static site on GitHub Pages. Two common options are shown below.

- **Project Pages (recommended for single presentations):** the site is served at `https://USERNAME.github.io/REPO/`.

  1. Create a repository on GitHub (or via `gh repo create`).
  2. Add the remote and push your `main` branch:

  ```bash
  git remote add origin git@github.com:USERNAME/REPO.git
  git push -u origin main
  ```

  3. Build the static site:

  ```bash
  pnpm build   # creates a `dist/` directory
  ```

  4. Deploy `dist/` to the `gh-pages` branch. Two simple ways:

  - Using the `gh-pages` helper:

    ```bash
    pnpm add -D gh-pages
    npx gh-pages -d dist -r git@github.com:USERNAME/REPO.git
    ```

  - Or with `git subtree`:

    ```bash
    pnpm build
    git add -f dist
    git commit -m "chore(build): add dist"
    git subtree push --prefix dist origin gh-pages
    ```

  After deployment, GitHub Pages will serve the site at `https://USERNAME.github.io/REPO/` (may take a minute to become available).

- **User/Organization Pages:** create a repository named `USERNAME.github.io` and push the built site to the `main` branch. The site will be served at `https://USERNAME.github.io/`.

Notes:
- If your slides are under a subpath (e.g. `https://jn1707.github.io/Talks/mcdata/`) set the Slidev `base` or build with the correct public base path so links resolve; check Slidev docs for `--base`/`--router-base` options or adjust the exported files accordingly.
- For reproducible deploys, prefer the `gh-pages` package or a CI workflow that builds and deploys `dist/` (GitHub Actions).

### Workflow

```bash
# Create new presentation
cp -r template "New Meeting 2026"
cd "New Meeting 2026"

# Work on slides
pnpm dev

# Commit your work
git add .
git commit -m "Add new meeting presentation"
git push
```

### Branching Strategy (Optional)

```bash
# Create branch for work-in-progress
git checkout -b wip/new-meeting-2026

# When ready to merge
git checkout main
git merge wip/new-meeting-2026
```

## Common Slidev Features

### Basic Slide Syntax

```markdown
---
layout: default
---

# Slide Title

Content goes here

- Bullet point
- Another point

---
transition: fade-out
---

# Next Slide
```

### Click Animations

```markdown
# Progressive Disclosure

<v-click>

First click reveals this

</v-click>

<v-click>

Second click reveals this

</v-click>

<div v-click="3">
  Third click
</div>
```

### Images

```markdown
# Simple Image
![Alt text](./images/my-image.png)

# Styled Image
<img src="./images/my-image.png" 
     style="max-width: 80%; border-radius: 12px;" />

# Draggable Image
<img v-drag="'position-name'" src="./images/my-image.png" />
```

### Videos

```markdown
<SlidevVideo autoplay controls>
  <source src="./videos/my-video.mp4" type="video/mp4" />
</SlidevVideo>
```

### Math (LaTeX)

```markdown
Inline: $E = mc^2$

Block:
$$
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
$$
```

### Two-Column Layout

```markdown
<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 2rem;">
  <div>
    Left column content
  </div>
  <div>
    Right column content
  </div>
</div>
```

## Useful Commands

```bash
# Development
pnpm dev              # Start dev server
pnpm build            # Build for production
pnpm export           # Export to PDF

# Dependencies
pnpm install          # Install dependencies
pnpm update           # Update dependencies

# Cleanup
rm -rf node_modules .slidev dist
pnpm install          # Fresh install
```

## Troubleshooting

### Port Already in Use
```bash
# Kill process on port 3173
lsof -ti:3173 | xargs kill -9

# Or use different port
pnpm dev --port 3174
```

### PDF Export Issues
```bash
# Install Playwright
pnpm add -D playwright-chromium
npx playwright install chromium
```

### Math Not Rendering
Ensure KaTeX is installed:
```bash
pnpm add -D @slidev/plugin-katex
```

### Images Not Loading
- Check file path: `./images/` (relative to slides.md)
- Ensure images are in `public/` folder
- Check file extensions (case-sensitive on some systems)

## Resources

- [Slidev Documentation](https://sli.dev/)
- [Slidev Themes](https://sli.dev/themes/gallery.html)
- [Vue 3 Documentation](https://vuejs.org/) (for custom components)
- [UnoCSS Documentation](https://unocss.dev/) (styling utilities)

## Tips & Best Practices

1. **Keep slides.md clean**: Use components for complex layouts
2. **Optimize images**: Compress before adding (use tools like ImageOptim)
3. **Version control**: Commit frequently with descriptive messages
4. **Backup exports**: Keep PDF versions in a separate folder
5. **Test exports early**: Don't wait until presentation day
6. **Use consistent naming**: `YYYY-MM-DD_Conference_Topic` format
7. **Document custom styles**: Add comments in slides.md
8. **Reusable components**: Extract common patterns to `components/`

## Contact & Support

For Slidev-specific issues: [GitHub Issues](https://github.com/slidevjs/slidev/issues)

For this template setup: Contact Janni