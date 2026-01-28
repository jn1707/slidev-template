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

## Linking to Shared Assets

If you want to use logos or images from the shared assets folder or another presentation:

```bash
# Symlink shared logos
ln -s ../../shared-assets/logos ./public/logos

# Symlink specific images from another presentation
ln -s "../../Geneva presentation/public/images/icecube_detector.jpg" ./public/images/

# Then reference in slides.md:
# <img src="./logos/ku_logo.png" />
# <img src="./images/icecube_detector.jpg" />
```

## Customization

1. Edit [slides.md](slides.md) for your content
2. Add images to `public/images/`
3. Add custom Vue components to `components/`
4. Adjust colors/styles in slide `<style>` blocks

## Structure

- `slides.md` - Main presentation file
- `public/images/` - Your presentation images
- `components/` - Custom Vue components
- `package.json` - Dependencies and scripts

## Notes

Add any specific notes about this presentation here.
