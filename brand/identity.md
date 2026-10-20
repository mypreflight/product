# Brand identity

How MyPreflight looks.

Last reviewed: 2026-10-20

Values below were read from the mypreflight.io CSS and the app's stylesheets on 2026-10-20. UI rules built on them are in `../frontend/design-rules.md`.

## Character

- **Brand type:** a professional, faceless product brand. No founder's face.
- **Style:** minimal yet professional. Rich, thought-through minimal; never the kind of minimal that looks like there's nothing to say.
- **Three words:** precise, professional, dependable.
- **No airline logos.**

## Name

| Where | Form |
| --- | --- |
| Prose, titles, alt text, notifications | **MyPreflight** (one word, capital M and P) |
| Logotype beside the mark | **mypreflight**, all lowercase: `my` in Noto Sans 400, `preflight` in Noto Sans 700 |

The two forms are not interchangeable. Never bold the whole logotype.

## Logo

| File | What it is |
| --- | --- |
| `assets/mypreflight-mark.svg` | The mark, vector, indigo `#6875F5` |
| `assets/mypreflight-icon-512.png` | App icon, 512 × 512 |
| `assets/mypreflight-icon-180.png` | Touch icon, 180 × 180 |
| `assets/mypreflight-social-card.png` | Social card, 1200 × 630: mark, logotype, headline and a live-map screenshot |

All four were downloaded from mypreflight.io. The wordmark is live text in Noto Sans, not an image: set it with the weights above.

The name, the logo and everything in `assets/` are not covered by this repo's CC BY 4.0 licence. All rights reserved.

## Colour

| Role | Token | Value |
| --- | --- | --- |
| Primary accent | `indigo-500` | `#6875f5` |
| Accent hover, pressed | `indigo-600` | `#5850ec` |
| Accent tint | `indigo-50` | `#f0f5ff` |
| Brand background (splash, icons) | `gray-50` | `#f9fafb` |
| App shell background (light) | `gray-100` | `#f3f4f6` |
| Dark background | `gray-950` | Tailwind default |
| Body text (light) | `gray-800` | `#1f2937` |
| Muted text | `gray-500` | `#6b7280` |

- The indigo ramp is Flowbite's, inherited by the app from `flowbite-react` and copied by the website. Stock Tailwind indigo is a different, visibly wrong hue next to the mark.
- Semantic colours (success, warning, info, danger) are for status only and always come with a word or icon.
- The website body is white in light mode.

## Type

| Use | Font | Weights |
| --- | --- | --- |
| Everything: headings, labels, body, controls | **Noto Sans** | 400 and 700 (the app also uses 600 for titles) |
| Figures people compare or sum: weights, fuel, times | **Roboto Mono**, tabular figures | 500 |

The website self-hosts Noto Sans (variable, Latin and Latin Extended, so Polish diacritics are covered) and ships its OFL licence beside it. The app loads Noto Sans and Roboto Mono from Google Fonts.

## Imagery

- Real product screenshots only: component crops from the running app, never mock-ups or invented numbers. See `screenshots.md`.
- Postcards are the one AI-generated artwork: flat-vector travel posters, one per city.
