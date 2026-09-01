# Liquid Glass

Material by **Sohum Suthar** - https://github.com/sohumsuthar/liquid-glass
npm: `@sohumsuthar/liquid-glass@3.1.0` - MIT, (c) 2026 Sohum Suthar. See `LICENSE`.

## How this site consumes it

- **CSS: from npm via jsDelivr**, pinned to 3.1.0.
  `liquid-glass-core.css` and `liquid-glass-effects.css` are linked in
  `index.html`. No local copy - the published package is the single source.
- **`useLiquidGlassEffects` + `Spotlight`: inlined verbatim** into the Babel
  script in `index.html`, from `hooks/useLiquidGlassEffects.jsx`. This page runs
  React from a CDN through Babel standalone, so there is no bundler to resolve
  the package's ESM imports. The code is the author's, unmodified apart from
  dropping the `import` and `export` lines.
- **`filters.html`**: the two inline SVG refraction filters (`#lg-refract`,
  `#lg-refract-sm`) lifted from the upstream demo, each carrying its
  displacement map as a base64 data URL. Pasted into `index.html` after `<body>`.

The React components are not used.

## What the effects hook drives

`liquid-glass-effects.css` is inert without it. The hook supplies:

| Written by the hook | Drives |
|---|---|
| `--mx` / `--my`, `--lg-light-angle` | per-element cursor lens; specular rim rotates toward the pointer |
| `--cx` / `--cy`, `html.over-glass` | sitewide spotlight |
| `data-reveal="in"` | scroll reveal on every `.liquid-glass` |
| `html.scrolled`, `--sv` / `--svmag` | macro squash on scroll velocity |

Linking the CSS without mounting the hook leaves all of the above unset, so the
glass renders static - correct-looking but dead. Verified live after wiring:
`--mx=97.47px`, `--lg-light-angle=-88.7deg`, `html.class="over-glass scrolled"`,
`--sv=0.063`.

## Local retune

The library is calibrated against macOS Control Center, where the backdrop is
bright: `glass_L = 0.58 * backdrop_L + 34`. On a dark ground that lift puts a
panel near `#b3b3b3` and `--text-secondary` becomes unreadable, so `index.html`
overrides `--lg-brightness`, the tint alpha, and adds a dark scrim for
`.project-glass`, `.rank-card`, `.cert-card`, `.navpill`, `.searchpill` and
`.nav-float`. Rim, specular highlight and refraction are untouched. Light mode
reverts to library defaults.

**Any new glass element must be added to those selector lists**, or it renders
pale against everything else. That has caught us twice.

Glass also needs something behind it to refract, so the aurora blobs live in a
page-wide fixed layer (`.page-aurora`) over a procedural starfield and nebula.
