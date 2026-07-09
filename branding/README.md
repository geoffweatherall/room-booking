# Branding

Brand assets for the room-booking project family (`room-booking-webapp`, and the future
`room-booking-android`). This is prep material — nothing outside this directory references
these files yet. See [palette.html](palette.html) for a human-viewable version of everything
below, including the logo rendered at size.

## The mark: Calendar Done

A calendar with a plain checkmark badged onto the header — no room imagery, no wordmark yet,
just booked-and-confirmed. Flat, rounded, Material-language shapes. Chosen from a wider
exploration (a door motif, a two-chairs-at-a-table room scene, and a bolder booked-slot-bar
variant were the other finalists) for being the cleanest at small sizes and the least fussy to
extend later.

## Files

| Path | Purpose |
|---|---|
| [resources/logo.svg](resources/logo.svg) | Master mark, transparent background, 240×240 viewBox. The source of truth vector - scales cleanly to any size or resolution. Colors are hardcoded hex (not CSS variables), since standalone SVG tooling won't resolve custom properties. |
| [resources/icon.svg](resources/icon.svg) | Same mark on a plain square Paper-colored canvas, no corner rounding baked in. Ready to export as PNGs at any resolution for favicons, app icons, etc. Platform-specific masking (Android adaptive icon foreground/background split, iOS's own corner rounding) should be derived from this or from `logo.svg` when that work happens, not re-baked into this file. |
| [tokens.css](tokens.css) | CSS custom properties for the UI token set below - light default, dark via `prefers-color-scheme` and via a `data-theme` attribute override. |
| [palette.html](palette.html) | Standalone, self-contained HTML page for visually reviewing the logo and full palette in a browser. |

## Logo colors

Fixed hex values used inside the mark itself - constant regardless of the surrounding app's
theme, the same way most app icons don't recolor when the OS switches theme.

| Name | Hex |
|---|---|
| Ink Violet | `#4338CA` |
| Signal Teal | `#0E8F82` |
| Sunbeam Amber | `#F59E0B` |
| Paper | `#FAF9F6` |

## UI tokens

For the app's own surfaces, text, and accents (see `tokens.css`). `--color-primary`,
`--color-secondary`, and `--color-accent` are the same three logo hues above, reused for the
app's own buttons/links/highlights; they shift to lighter variants in dark mode for contrast,
unlike the logo's fixed colors.

| Token | Light | Dark |
|---|---|---|
| `--color-bg` | `#FAF9F6` | `#17152A` |
| `--color-surface` | `#FFFFFF` | `#201D38` |
| `--color-surface-tile` | `#F4F1EA` | `#ECE8F7` |
| `--color-ink` | `#1E1B2E` | `#F1EFFA` |
| `--color-ink-soft` | `#58527A` | `#B6B0D8` |
| `--color-border` | `#E7E3F6` | `#34305A` |
| `--color-primary` | `#4338CA` | `#8B85F0` |
| `--color-secondary` | `#0E8F82` | `#2DD4BF` |
| `--color-accent` | `#F59E0B` | `#FBBF24` |

## Status

Not yet wired up. The webapp still uses MUI's default (unbranded) theme, and the Android app
doesn't exist as code yet - just an empty repo. Applying this to the webapp (replacing the MUI
default palette, adding the icon as favicon/header mark) is tracked as future work, not part of
this commit.
