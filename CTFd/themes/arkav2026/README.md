# Arkav 2026

An independent CTFd++ participant theme based on the Arkavidia 11.0 identity.
The admin theme and backend are unchanged.

## Design reference

- Brand reference: https://www.figma.com/design/5d9GpsB6HzsycGGwsMkSX6?node-id=672-124
- Homepage illustration: https://www.figma.com/design/5d9GpsB6HzsycGGwsMkSX6?node-id=672-4368
- Pine `#0f2f2e`, jade `#2e5e54`, gold `#f5a623`, ochre `#d98e04`, cream
  `#fffce9`, sand `#fdd594`, coral `#ef4434`, blue `#2a799d`, indigo `#314ab3`.
- The original Marko One font is bundled in `static/fonts`, with its SIL Open
  Font License. Source: https://github.com/google/fonts/tree/main/ofl/markoone.
- The navbar emblem is a read-only PNG export of Figma node `672:5235`.

All Figma access for this theme is read-only. The theme keeps local copies of
assets and never changes the source Figma file.

## Homepage artwork preparation

The supplied originals are kept locally in `source/hero-originial.svg` and
`source/hero-originial-without-coming-soon.svg`. The theme uses the latter,
without modifying either original. The 81.7 MB source is rendered to four
prebuilt, compressed WebP assets:

| Asset | Dimensions | Size |
| --- | --- | --- |
| `static/img/hero-landscape.webp` | 1900 × 1652 | 435,216 bytes |
| `static/img/hero-landscape-mobile.webp` | 900 × 783 | 91,282 bytes |
| `static/img/hero-banner.webp` | 3840 × 928 | 949,292 bytes |
| `static/img/hero-banner-mobile.webp` | 1200 × 290 | 104,806 bytes |

The homepage is a full viewport scene, with navigation over the artwork.
The landscape uses `cover` to fill the viewport. The original Frame 6 banner
is exported separately with its transparency and glow, so its wordmark stays
fully visible even on portrait screens. The landscape uses `srcset` so the
browser can choose a suitable resolution. The banner has its own `srcset` for
high-density screens. The original Figma glow is retained; its text is rendered
from the SVG at higher resolution rather than upscaling the old WebP. Marko One
labels and action buttons are grouped underneath, near the center of the hero.
Short viewports can scroll rather than clipping the banner or action buttons.
The original SVGs are excluded from Git and the Docker build context. They
are not served by the theme or needed to deploy the prebuilt artwork.

To regenerate from the repository root:

```bash
python3 CTFd/themes/arkav2026/tools/prepare_artwork.py \
  CTFd/themes/arkav2026/source/hero-originial-without-coming-soon.svg \
  --already-clean --crop-top 1376 --separate-banner
```

The helper requires Pillow and system libraries librsvg, Cairo and GObject.
These are available on the current development host. `--already-clean` uses
the supplied version without Coming Soon directly. `--crop-top 1376` excludes
the texture extending above Frame 8 in these exports, retaining the intended
3105 × 2700 artwork. This cropping happens in memory only. The white canvas
preserves the texture's intended blending.

`--width` controls the landscape desktop width (default 1900); `--quality`
controls landscape WebP quality (default 90, with mobile capped at 88). The
landscape mobile width is capped at 900. The separate banner defaults to width
3840 and quality 96, with a 1200-pixel variant capped at quality 94. These can
be changed with `--banner-width` and `--banner-quality`. Add `--banner-only`
to the command above to regenerate just the banner assets. To use a new export
already clipped to Frame 8, omit `--crop-top`.

## Background on Challenges, Scoreboard, Users and Teams

The supplied `source/Archived.svg` is kept unchanged and prepared as:

| Asset | Dimensions | Size |
| --- | --- | --- |
| `static/img/archived-background.webp` | 1920 × 1080 | 51,608 bytes |
| `static/img/archived-background-mobile.webp` | 900 × 506 | 17,854 bytes |

```bash
python3 CTFd/themes/arkav2026/tools/prepare_artwork.py \
  CTFd/themes/arkav2026/source/Archived.svg \
  --already-clean --background --width 1920
```

These backgrounds apply to the `challenges`, `scoreboard`, `users` and `teams`
blueprints, including public/private user and team pages. The background fills
the viewport and remains fixed as content scrolls. Paper panels behind text,
tables and forms preserve readability in light and dark modes.

## Component styling

The Figma ribbon inspires the shared paper/gold borders, curved corners and
Marko One labels. All Bootstrap button variants, navbar links, tabs, pagination,
dropdowns, close buttons, hints, forms and challenge cards use the theme's CSS
tokens, including hover, active, focus and disabled states. Colors can be edited
in the `.arkav-theme` variables at the top of `static/css/arkav2026.css`.

The navbar brand has no mouse-hover treatment. Navigation uses subtle cream
borders without a gold outline. The hamburger is shown below the expanded
navigation breakpoint (1200 CSS pixels), and hidden above it. Table headers
stay on one line while long values in table cells can wrap.

`static/img/favicon.ico` (16/32/48/64 pixels) and `favicon-32.png` are prepared
from the supplied `source/logo.png`, retaining its aspect ratio on a transparent
square. The participant theme uses these directly instead of CTFd's default icon.

`ncw2026` informed the extent of the theming: identity across controls, panels
and navigation. Its Persona artwork, fonts, motion and components are not used.

## Enable

Select **arkav2026** in **Admin > Config > Theme** and
click **Update**. Keep `THEME_FALLBACK` enabled. The theme uses the core theme's
compiled CSS/JavaScript and functional templates, including AI Source fields,
solver uploads, team forms, scoreboard, and account pages. An uploaded event
logo still takes precedence over the default navbar mark.

The homepage uses the selected illustration and login/challenge navigation
instead of the default index page content. Other custom Pages keep their
configured content. Navbar links continue to follow CTFd++ permissions.

Plain theme CSS and prebuilt assets are served directly, so no Node build or
Docker image rebuild is needed with the repository's source bind mount. Restart
CTFd after first adding the theme so its templates are discovered:

```bash
docker compose restart ctfd
```

Then select the theme and refresh the browser. Both light and dark modes use
the same Arkavidia palette.

The stylesheet uses `views.themes_beta`, as `ncw2026` does. CTFd's legacy
`views.themes` URL helper automatically inserts `.min` or `.dev` for CSS/JS,
which would point this prebuilt stylesheet at a nonexistent filename. Keep
the direct route when changing the stylesheet URL. After a template update,
restart CTFd and hard-refresh the browser to reload templates and CSS.

## Layout

```text
arkav2026/
├── templates/             Jinja shell, navbar, homepage and theme settings
├── static/css/            Scoped Arkavidia component styles
├── static/fonts/          Marko One and license
├── static/img/            Emblem, homepage layers and Archived background
├── source/                Original local artwork export
└── tools/                 Local artwork preparation helper
```

No unit tests, application runtime checks, or visual browser checks were run;
testing is left to the project owner as requested.
