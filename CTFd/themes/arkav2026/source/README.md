# Original artwork

This folder keeps the supplied, unmodified SVG exports:

- `hero-originial.svg`
- `hero-originial-without-coming-soon.svg`
- `Archived.svg`

The spelling matches the supplied filenames. The original exports stay local
and are excluded from Git and the Docker build context. Each hero source is
about 82 MB; Archived.svg is about 2.5 MB.

Source: https://www.figma.com/design/5d9GpsB6HzsycGGwsMkSX6?node-id=672-4368

The theme uses the export without Coming Soon. From the repository root:

```bash
python3 CTFd/themes/arkav2026/tools/prepare_artwork.py \
  CTFd/themes/arkav2026/source/hero-originial-without-coming-soon.svg \
  --already-clean --crop-top 1376 --separate-banner
```

These exports include 1376 SVG units of texture above the intended Frame 8.
The helper excludes that extra space in memory and generates desktop/mobile
landscapes plus the original banner in `../static/img/` for the fullscreen hero.

Prepare the background for Challenges, Scoreboard, Users and Teams with:

```bash
python3 CTFd/themes/arkav2026/tools/prepare_artwork.py \
  CTFd/themes/arkav2026/source/Archived.svg \
  --already-clean --background --width 1920
```

Neither the original SVGs nor the Figma document are changed. See the theme
README for dimensions and quality options.
