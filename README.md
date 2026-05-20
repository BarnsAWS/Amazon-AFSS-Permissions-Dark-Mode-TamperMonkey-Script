# DEPRECATED — superseded by the Web Dark Mode Bundle

> ⚠️ **This standalone userscript is deprecated as of v2.0 of the bundle.**
>
> All sites previously covered by this repo (and 10 others) are now consolidated into a single auto-updating Tampermonkey userscript:
>
> **https://github.com/BarnsAWS/Web-Dark-Mode-Bundle**
>
> Direct install URL:
>
> `https://raw.githubusercontent.com/BarnsAWS/Web-Dark-Mode-Bundle/main/web_dark_mode_bundle.user.js`
>
> ## Migration
>
> 1. Open Tampermonkey dashboard.
> 2. **Disable or remove** this script (otherwise two scripts will fight on the same page).
> 3. Open the install URL above and click **Install**.
> 4. Tampermonkey will auto-update from `main` going forward.
>
> ## Why the bundle exists
>
> - One install, one auto-update across all covered sites.
> - Shared Cloudscape v3.3 engine — every site gets the same standard at the same time.
> - Adding a new site is a one-file change to `SITES` in the bundle.
>
> The original README/ABOUT and userscript are retained below this notice for archival / reference.
>
> ---

# AFSS Permissions Dark Mode TamperMonkey Script

A Tampermonkey/Violentmonkey userscript that applies an AWS Cloudscape-aligned dark theme to the `prod.permissions.afss-security.aws.dev` Manage Permissions / Posix Group browser UI.

## What This Repository Contains

- `afss_permissions_dark_mode.user.js` — the Tampermonkey userscript.
- `STANDARDS.md` — the Cloudscape-aligned dark-mode standard the script implements.
- `SANITIZATION.md` — what was scrubbed before this repo was made public.
- `LICENSE` — MIT.
- `README.md`, `ABOUT.md`.

## Behavior Coverage

The script follows the **Cloudscape Dark Mode Standard v3.3** (see `STANDARDS.md`):

- Forces a Cloudscape "Polaris Dark Mode" palette on the Manage Permissions shell — top header, the orange "Manage Permissions" wordmark, the breadcrumb (`Manage Permissions > Request Access`), the left-rail navigation (Place Request For / Manage&View Permissions / Ownership / Help Me! / Teams / Groups / Helpful Links), the group-detail card (group name + ownership details + group actions), the paginated member list, the search input, and the footer.
- Preserves the full Cloudscape text hierarchy and brightens stat labels (member counts, expiry dates) to emphasis text.
- Applies semantic alert tints (red error, blue info, green success badge) and Amazon Orange to primary action buttons.
- Forces dark surfaces on dropdowns, popovers, modals, menus, listboxes, tooltips, and any framework `awsui_*` surfaces.
- Runs the v3.3 `enforceDarkSurfaces()` JS pass which detects light surfaces via *both* `background-color` and `background-image`, strips white-stop gradients, and routes them by luminance bucket to `--bg-secondary` (`#1b232d`) or `--bg-tertiary` (`#232b37`).
- Includes the v3.3 sub-component coverage rules so card-internal `header` / `[class*="title"]` / `[class*="body"]` / `[class*="content"]` / `[class*="footer"]` drop to transparent and inherit the parent card's `--bg-secondary` surface.
- Attaches the v3.3 tight-loop `MutationObserver` on `style` attribute mutations, batched via `requestAnimationFrame`, so framework writes during interaction get corrected within ~16 ms.
- Includes the v3.1 inline-control rule so the Search Here input renders as an inset `#0a0f15` well with a `#656871` border (passes WCAG 1.4.11).
- Includes the v3.2 generalized in-content selected rule so the active page in the left rail and selected paginator pages get a `#001129` background with a 3 px `#42b4ff` left-rail; sidebar nav items get `#0a0f15` + 4 px rail.
- Pierces open shadow roots and re-applies the same stylesheet.
- Observes DOM mutations with a 250 ms main observer (class / ARIA / data-*) and a tight rAF observer (style only); runs delayed safety passes at 500 / 1500 / 3000 ms after `load`.

## Install on Chrome

1. Install Tampermonkey: <https://chromewebstore.google.com/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo>
2. Open the Tampermonkey dashboard and confirm it is enabled.
3. Open the raw userscript URL and let Tampermonkey prompt for install:
   `https://raw.githubusercontent.com/BarnsAWS/Amazon-AFSS-Permissions-Dark-Mode-TamperMonkey-Script/main/afss_permissions_dark_mode.user.js`
4. Click **Install**.
5. Visit the target page and refresh once.

## Install on Firefox

1. Install Tampermonkey: <https://addons.mozilla.org/en-US/firefox/addon/tampermonkey/>
2. Open the Tampermonkey dashboard.
3. Open the raw userscript URL above and click **Install**.
4. Visit the target page and refresh once.

## Verification Checklist

- [ ] Page body and main content render on `#161d26`.
- [ ] Top header renders dark; the Manage Permissions wordmark stays orange.
- [ ] Breadcrumb (`Manage Permissions > Request Access`) — the link is `#42b4ff`, the trailing crumb is `#8c8c94`.
- [ ] Left-rail panels (Place Request For, Manage&View Permissions, Ownership, Help Me!, Teams, Groups, Helpful Links) render on `#1b232d` cards with readable section titles.
- [ ] Search Here input is an inset `#0a0f15` well with a `#656871` border.
- [ ] Group-detail card (group name, Note, Ownership Details, Group Actions, Members section) is on `#1b232d` with `#424650` border and internal sub-headings dropped to transparent.
- [ ] Member-list pagination links are `#42b4ff`.
- [ ] Member-list zebra rows alternate `#1b232d` / `#232b37`.
- [ ] No card sub-component renders with a light surface.
- [ ] No element renders with a `background-image: linear-gradient(white, ...)`.
- [ ] No bright white flash on initial load.

## Troubleshooting

- **Bright sections after load** — hard refresh (Ctrl+F5) and confirm Tampermonkey is enabled.
- **Embedded iframe still light** — Tampermonkey does not inject into cross-origin iframes by default.

## Source References

- `STANDARDS.md` (in this repo) — the Cloudscape-aligned dark-mode standard.
- `SANITIZATION.md` (in this repo) — sanitization log.
- AWS Cloudscape Design System: <https://cloudscape.design>

## License

MIT (see `LICENSE`).
