# Liquid Glass (vendored)

Material by **Sohum Suthar** - https://github.com/sohumsuthar/liquid-glass
MIT License, Copyright (c) 2026 Sohum Suthar. Full text in `LICENSE`.

Vendored rather than installed from npm because this site is a single static
`index.html` with React from a CDN, and there is no bundler to resolve the
package's JSX components.

## What is used

- `css/liquid-glass-core.css` - the 4-layer material
- `css/liquid-glass-effects.css` - supporting effects
- `filters.html` - the two inline SVG refraction filters (`#lg-refract`,
  `#lg-refract-sm`), lifted verbatim from the upstream demo. Each carries its
  displacement map as a base64 data URL, so no extra asset is needed. Pasted
  into `index.html` directly after `<body>`.

The React components and hooks are not used.

## Local retune

The library is calibrated against macOS Control Center, where the backdrop is
bright: `glass_L = 0.58 * backdrop_L + 34`. On this site's `#000000` ground that
lift puts a panel near `#b3b3b3`, and `--text-secondary` (`#a0a0a0`) becomes
unreadable on it.

`index.html` therefore overrides `--lg-brightness`, the tint alpha, and adds a
dark scrim for `.project-glass`, `.rank-card` and `.nav-float`. The rim,
specular highlight and refraction are untouched. Light mode reverts to the
library defaults.

Glass also needs something behind it to refract, so the aurora blobs moved from
inside the hero to a page-wide fixed layer (`.page-aurora`) and their opacity
was raised.
