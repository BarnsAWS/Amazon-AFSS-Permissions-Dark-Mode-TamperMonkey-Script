# About This Repository

## Purpose

This repository delivers a Tampermonkey userscript that re-themes the Manage Permissions / Posix Group browser at `prod.permissions.afss-security.aws.dev` using the AWS Cloudscape "Polaris Dark Mode" token palette.

## Design Principles

- **Cloudscape palette as ground truth.** Surfaces, text tiers, links, borders, focus rings, and primary actions all use Cloudscape v3.3 tokens (`#161d26` body, `#1b232d` cards, `#0a0f15` inset controls, `#42b4ff` link/accent). No hand-picked hex values. See `STANDARDS.md`.
- **Two-layer architecture.** CSS injected at `document-start` for an immediate dark paint; JS surface-enforcement passes as the safety net for inline styles, runtime injections, framework gradients, and shadow roots.
- **v3.3 sub-component coverage.** Card-internal `header` / `[class*="title"]` / `[class*="body"]` / `[class*="content"]` / `[class*="footer"]` drop to transparent so the parent card's `--bg-secondary` shows through.
- **v3.3 background-image stripping.** Every dark `background-color` rule pairs with `background-image: none !important`. The JS `isLightSurface()` helper scans gradient stops and strips any with luminance > 200.
- **v3.3 tight-loop style observer.** A dedicated `MutationObserver` on `attributeFilter: ['style']`, batched via `requestAnimationFrame`, catches framework writes within ~16 ms.
- **Hands off media + chart content.** Images, video, canvas, picture, and SVG content are excluded from color overrides.
- **Hands off the cascade.** No `filter: invert(1)`. Brand colors (orange Manage Permissions wordmark, semantic alerts) are preserved.

## Script Architecture

`afss_permissions_dark_mode.user.js` runs in seven phases (see `STANDARDS.md` for the full reference): framework hook → CSS injection at `document-start` → JS `enforceDarkSurfaces()` pass → top-bar detection → inline control + selected row passes → tight-loop style observer → main mutation observer + load-time safety net.

## Why This Repository Exists

The Manage Permissions / Posix Group browser is one of the more text-heavy permission-management UIs in regular use. The default light theme has long unfiltered member lists on bright cream rows that fatigue the eyes. This script delivers a single, predictable, Cloudscape-aligned dark experience for the full Manage Permissions surface.

## Maintenance Notes

- Keep `afss_permissions_dark_mode.user.js` at the repository root so the GitHub raw URL is the canonical Tampermonkey install link.
- Cloudscape tokens are the source of truth. If Cloudscape revises the dark palette, update the constants block at the top of the userscript and `STANDARDS.md`.

## Source References

- `STANDARDS.md` — the Cloudscape-aligned dark-mode standard this script implements.
- `SANITIZATION.md` — sanitization log for this repository.
- AWS Cloudscape Design System: <https://cloudscape.design>

## License

MIT (see `LICENSE`).
