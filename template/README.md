# Presentation Name

Brief description of the presentation.

## Details

- **Date**: DD Month YYYY
- **Event**: Conference/Meeting Name
- **Speaker**: Your Name
- **Institution**: Your Institution

## Quick Start

```bash
# Install dependencies
pnpm install

# Start development server
pnpm dev

# Export to PDF
pnpm export

# Export to PowerPoint
pnpm export --format pptx
```

## Bringing in images from elsewhere

Copy them in; do not symlink. Vite refuses to serve a `public/` symlink whose target sits
outside the project root, so the images load in neither `pnpm dev` nor an export.

```bash
cp -R ../shared-assets/logos ./public/logos
cp "../<other-deck>/public/images/detector.jpg" ./public/images/
```

Reference them with root-absolute paths, which resolve against `public/`:

```html
<img src="/logos/ku_logo.png" />
<img src="/images/detector.jpg" />
```

A referenced image that is not on disk is a hard build failure, not a warning.

## Customization

1. Edit [slides.md](slides.md) for your content
2. Add images to `public/images/`
3. Add custom Vue components to `components/`
4. Adjust colors/styles in slide `<style>` blocks

## Structure

- `slides.md` - Main presentation file
- `public/images/` - Your presentation images
- `public/logos/` - Institutional logos used on the title slide
- `components/` - Custom Vue components, including `VerticalSlides.vue`
- `package.json` - Dependencies and scripts
- `pnpm-workspace.yaml` - Lets Playwright install its browser, required for `pnpm export`

## Notes

Add any specific notes about this presentation here.
