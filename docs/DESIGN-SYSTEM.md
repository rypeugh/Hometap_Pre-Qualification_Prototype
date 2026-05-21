# Design System

All values sourced directly from Hometap's live homepage HTML source code. Do not change hex values without explicit instruction.

## Palette

| Token | Hex | Source class | Usage |
|---|---|---|---|
| `--primary` | `#366CED` | `bg-primary100` | CTA buttons, links |
| `--primary-hover` | `#2453C9` | `bg-blue300` | Button hover state |
| `--primary-active` | `#1A3FA8` | `bg-blue500` | Button active/pressed |
| `--primary-light` | `#EEF2FF` | `bg-primary5` / `bg-blue5` | Nav background, card backgrounds |
| `--primary-mid` | `#D2DFFC` | `bg-primary15` | Subtle borders, dividers |
| `--purple` | `#6A5CF7` | `text-purple200` | Hero accent, H1/H2 color emphasis |
| `--purple-dark` | `#3D31C8` | `purple400` | Dark section backgrounds |
| `--purple-section` | `#2D2480` | `purple500` | Deep footer / dark sections |
| `--text` | `#434C5E` | `neutral500` | Primary body text (confirmed via SVG stroke) |
| `--text-sub` | `#6B7585` | `neutral400` | Secondary / muted text |
| `--text-light` | `#9CA5B0` | `neutral300` | Placeholder, disabled |
| `--border` | `#DDE3EC` | `neutral30` | Input borders, dividers |
| `--bg` | `#FFFFFF` | — | Page background |
| `--card-bg` | `#F5F8FF` | — | Card / selected state background |
| `--success` | `#16A34A` | — | Positive indicators (HEI wins) |
| `--success-bg` | `#F0FDF4` | — | Win-row background in comparison table |
| `--warn` | `#DC2626` | — | Negative indicators (HELOC drawbacks) |

## Typography

- **Heading font:** Playfair Display (Google Fonts), weights 400–700 — matches Hometap's `font-serif`
- **Body font:** DM Sans (Google Fonts), weights 400–700 — matches Hometap's primary sans
- **Base size:** 16px
- **Line height:** 1.6 body, 1.2 headings
- **Heading scale (desktop):** H1 ~40px / H2 28px / H3 20px
- **Heading scale (prototype):** Follow what's in `index.html` — do not change without alignment

## Spacing Scale

Use only these values: `4, 8, 12, 16, 24, 32, 48, 64, 96` px

## Border Radius

- Default (cards, inputs): `10px` (`--radius`)
- Small (badges, tags): `6px` (`--radius-sm`)
- Large (modal, hero cards): `16px` (`--radius-xl`)
- Buttons: `8px`

## Buttons

- Primary (filled): `background: var(--primary)`, white text, 600 weight, `padding: 14px 28px`, `border-radius: 8px`
- Secondary (outlined): `border: 2px solid var(--primary)`, primary-colored text
- Min height: 44px

## Shadows

- Cards: `0 2px 8px rgba(54,108,237,0.08), 0 1px 3px rgba(0,0,0,0.06)` (`--shadow-card`)
- Raised/hover: `0 4px 16px rgba(54,108,237,0.14), 0 1px 4px rgba(0,0,0,0.08)` (`--shadow-raised`)

## Layout

- Max content width: `700px` (single-column wizard layout; intentional — matches Hometap's form-style UI)
- Section vertical padding: `48px` desktop
- Mobile breakpoint: `640px`

## Icons

- Library: Font Awesome Free 6.7.2 via cdnjs CDN
- Hometap's live site uses FA Kit with custom icons (`fa-kit`, `fa-ht` prefix) — FA Free matches the visual language but custom icons are not replicable without their kit
- Icon classes are stored in the `useCases` JS data object, rendered as `<i class="fa-solid fa-..."></i>`
- Do not upgrade to FA Pro/Kit without explicit instruction

## Logo

- Stored locally at `prototype/hometap-logo.svg` — no CDN dependency
- Fallback: `hometap` wordmark text in `var(--text)` if image fails
