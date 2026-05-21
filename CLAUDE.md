# PROJECT-CONTEXT.md

> **Read this at the start of every session.** Five seconds. The discipline is what makes it work.
>
> Section 2 (Locked Decisions) is binding. Section 4 (Current Working State) is the scratchpad — edit it freely. Don't let Section 4 silently overwrite Section 2.

---

## 1. Project Identity

- **Project name:** Hometap Education Flow Prototype
- **One-line description:** A clickable, high-fidelity prototype demonstrating an education-first pre-qualification flow for Hometap's home equity investment product.
- **Primary goal:** Prove the hypothesis that inserting a personalized education step — showing how a Home Equity Investment (HEI) compares to a HELOC or Home Equity Loan for the user's specific goal — will increase conversion from site visit to pre-qual start.
- **Target user (of the prototype):** Hometap interviewers evaluating Ryan Peugh for Principal PM, Growth. The prototype must be immediately legible without explanation.
- **Target user (of the product being modeled):** Homeowners aged 35–65 exploring equity access options. Low financial literacy on HEIs specifically. Likely familiar with HELOCs from advertising. Middle-income, suburban/exurban.
- **Success looks like:** A reviewer clicks through all 4 screens, immediately understands the hypothesis, and sees how the education layer slots into Hometap's existing pre-qual funnel without disrupting it.
- **Out of scope:** Real backend, auth, API calls, actual pre-qualification submission, admin dashboard, mobile app, slide deck.

---

## 2. Locked Decisions

### Hypothesis and funnel context

The case study identified three funnel stages:
1. Site visit
2. Pre-qual start ← **biggest drop: ~70% of visitors never start**
3. Pre-qual completion → approval → settlement

This prototype intervenes **between stage 1 and 2** by inserting a 3-screen education wizard before the existing pre-qual form. Screen 4 (Pre-Qual Entry) is a mockup of Hometap's actual pre-qual form, pre-populated based on the user's chosen use case.

The 4 screens in order:
1. **Use Case Selector** — "What would you like to do with your home equity?" (6 options matching Hometap's homepage nav labels)
2. **Comparison Table** — HEI vs. HELOC vs. Home Equity Loan, with rows dynamically highlighted for the selected use case
3. **Benefits Summary** — 3 benefit cards + a customer quote, personalized to use case
4. **Pre-Qual Entry** — mockup of Hometap's pre-qual form, with "reason for equity access" dropdown pre-selected

### Tech stack

- **Frontend:** Single-file `prototype/index.html` — all HTML, CSS, and JS in one file
- **Styling approach:** Plain CSS with CSS custom properties (variables) for all design tokens; no Tailwind, no framework
- **JavaScript:** Vanilla JS only; no build step, no npm, no modules
- **Backend / data:** None — all content is hardcoded in a JS data object keyed by use case
- **State management:** Single `selectedKey` variable; screen navigation via `goTo(n)` function
- **Hosting:** Open `prototype/index.html` directly in a browser; or via `npx serve prototype` on port 3333
- **Build step:** None

### Design system

Full token spec is in [`docs/DESIGN-SYSTEM.md`](docs/DESIGN-SYSTEM.md). All values sourced from Hometap's homepage HTML. Do not change hex values without explicit instruction.

**Nav pattern:** White background (`--bg`), logo left, "Log in" link right, no CTA button. Matches Hometap's mobile app pattern. Do not re-add a CTA to the nav.

### Use case options

These must exactly match Hometap's homepage navigation labels:

1. Pay off debt
2. Fund home improvements
3. Fund a life event
4. Fund an education
5. Fund your small business
6. Fund your retirement

Default selected: `debt` (first option)

### Comparison table columns

Always three: **Hometap Investment** | **HELOC** | **Home Equity Loan**

Win rows (highlighted green for HEI advantage) are defined per use case in the JS data object. Do not generalize them — each use case has specific rows that win.

### Conventions

- **File structure:** Flat. Everything is in `prototype/index.html`. Do not split into multiple files.
- **Naming:** `camelCase` for JS variables/functions; `kebab-case` for CSS classes; descriptive screen IDs (`screen-1`, `screen-2`, etc.)
- **JS data object:** All use-case-specific content lives in the `useCases` object at the top of the `<script>` block — comparison text, highlighted rows, benefit cards, quote. Keep this as the single source of truth.
- **No external libraries:** No jQuery, no chart libraries, no animation libraries. Vanilla only.
- **Accessibility floor:** Semantic HTML, WCAG AA contrast, `alt` on images, `aria-label` on interactive elements.

### UX and interaction principles

These are binding for every screen. Apply before writing or editing any copy or interaction pattern.

1. **Minimum copy.** Every word must earn its place. If a sentence can be cut without losing meaning, cut it. No eyebrows that restate the title, no subtitles that repeat the eyebrow.

2. **Minimalist design.** Prefer space and clarity over decoration. No extra visual layers, badges, or embellishments unless they communicate something the user needs to act on.

3. **Copy concise and scannable.** Use plain, direct language. Lead with the most important word. Avoid filler phrases like "Here's how," "Based on your goal," or "We'll show you."

4. **Single-select auto-advances.** Tapping a use case option on Screen 1 auto-navigates to Screen 2 after 280ms (enough for visual confirmation). No separate "Continue" button on selection screens. Do not add a confirm step.

5. **Progress indicator is a single full-width thin line** (`3px`, `--primary` fill, `--primary-mid` track). No step dots, no labels. Advances proportionally across 4 screens. Nav is logo left + Log in right — no CTA, no extra elements.

6. **CTAs always above the fold on mobile.** Reference device: iPhone 13 (390 × 844 CSS px). On screens 2, 3, and 4, the primary CTA uses `position: fixed; bottom: 0` on mobile via the `.screen-cta` wrapper class. Screen container gets `padding-bottom: 120px` on mobile to prevent content hiding behind the bar. Screen 1 has no CTA — selection auto-advances.

---

## 3. Operating Rules

### The anti-drift ritual

**Before any change that touches CSS or visual markup, re-read Section 2 Locked Decisions.** If a proposed change would use a color, font, spacing value, or radius not in the design system, surface the conflict explicitly: "This would use a color not in the locked palette — want to add it to Section 2, or find an alternative within the existing tokens?" Do not silently apply off-system values.

### Model routing

- **Sonnet by default** — styling tweaks, new use-case content, JS logic changes, bug fixes
- **Escalate to Opus** — only for: redesigning the information hierarchy across screens, major architectural changes to the JS state machine, or resolving conflicts that touch multiple locked decisions

### Skills to invoke

- **`ryan-peugh-context`** — load at session start; shapes framing around Ryan's PM background, Hometap audience, and interview context
- **`project-context-creator`** — invoke to update this file when locked decisions change
- **`pdf`** — invoke when working with the case study PDF or rendered page images

### Ask before acting

Always confirm before:

- Adding a new screen beyond the 4 defined
- Changing the use case option labels (they must match Hometap's homepage)
- Changing any hex color in Section 2
- Splitting `index.html` into multiple files
- Adding any external library or CDN dependency (approved: Google Fonts, Font Awesome Free 6.7.2 via cdnjs — nothing else without asking)
- Restructuring the `useCases` data object schema

For everything else (adding/editing use-case content, styling fixes within the locked system, bug fixes, copy edits), proceed without asking.

### End-of-session push

At the end of every session (when running the wrap ritual), commit any pending changes to `prototype/index.html`, `docs/`, and `CLAUDE.md`, then push to `origin main`. Use a descriptive commit message summarizing what changed. Do not push mid-session — only on wrap.

### Conflict resolution

Surface conflicts explicitly. Name the specific locked decision being violated and offer two paths: (a) update the lock deliberately, or (b) solve within the existing constraints.

---

## 4. Current Working State

### What I'm building right now

All 4 screens complete and validated on iPhone 13 viewport (390×844). Prototype is demo-ready.

### Known issues / things on fire

- None. Logo stored locally at `prototype/hometap-logo.svg` — no CDN dependency.

### Open questions / decisions pending

- Should the comparison table include a "Learn more" expandable row per option, or keep it flat?
- Are 6 use cases the right number, or should some be consolidated for demo clarity?
- Does the pre-qual screen (Screen 4) need more form fields to feel realistic?

### Recent changes worth remembering

- Icons: all emojis replaced with Font Awesome Free 6.7.2 (cdnjs). Icon classes stored in `useCases` data object, rendered as `<i class="fa-solid fa-..."></i>`.
- Nav: white background, logo left, Log in right, no CTA — matches Hometap mobile app.
- Progress: single full-width thin line only, no step dots or labels.
- Purple (`#6A5CF7`) is heading accent, NOT a CTA color. Blue (`#366CED`) for buttons/links.
- Screen 1 auto-advances to Screen 2 after 280ms on selection — no Continue button.
- CTAs on Screens 2, 3, 4 use `position: fixed; bottom: 0` on mobile via `.screen-cta` class.

### Snippets / reference URLs for this phase

- GitHub repo: `https://github.com/rypeugh/Hometap_Pre-Qualification_Prototype`
- Preview server: `npx serve prototype -p 3333` → open `http://localhost:3333`

---

## 5. How to Work With Me

- **Coding comfort:** Beginner — Ryan is a product leader, not a developer. Explain what code does in plain English when introducing new patterns. Don't assume familiarity with JS concepts beyond basic logic.
- **Communication style:** Direct and outcome-first. Lead with the result or decision, then the rationale. No hedging, no unnecessary preamble.
- **Pushback:** Welcome. If something won't look right or will conflict with the design system, say so. "This would look off because X — here's a better option" is the right call.
- **Pace:** Move fast. This is interview prep — time-sensitive. Skip long theoretical explanations unless asked.
- **Context:** Ryan is a senior PM (VP-level) with deep AI product experience. Frame suggestions in terms of product impact and user experience, not engineering elegance.
- **What I find annoying:** Generic SaaS aesthetics, AI hedging language, adding features that weren't asked for, code without a brief plain-English explanation of what it does.
- **What I appreciate:** Surfacing tradeoffs early, noticing when something looks off, flagging when a proposed change would conflict with the design system.

---

*Last updated: 2026-05-21 (session 3)*
