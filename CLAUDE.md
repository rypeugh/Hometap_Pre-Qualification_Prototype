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
- **Success looks like:** A reviewer clicks through the full education and pre-qual flow, immediately understands the hypothesis, and sees how the education layer slots into Hometap's existing funnel without disrupting it.
- **Out of scope:** Real backend, auth, API calls, actual pre-qualification submission, admin dashboard, mobile app, slide deck.

---

## 2. Locked Decisions

### Hypothesis and funnel context

The case study identified three funnel stages:
1. Site visit
2. Pre-qual start ← **biggest drop: ~70% of visitors never start**
3. Pre-qual completion → approval → settlement

This prototype intervenes **between stage 1 and 2** with a 14-screen pre-qual flow. The flow is **14 screens + 1 exit screen**.

**Note on screen IDs:** HTML IDs (`screen-1`, `screen-2`, etc.) do not match visual step order — navigation is handled entirely via `goTo(n)` and the `pct` map. Do not assume ID = step number.

| Step | HTML ID | Screen |
|------|---------|--------|
| 1 | screen-1 | **Interstitial** — "Get up to $600k from your home equity. No monthly payments." ($600k in `--purple`) + 01/02/03 steps + Trustpilot proof + "See my offer →" CTA |
| 2 | screen-3 | **Amount** — "How much money are you looking for?" — auto-advances |
| 3 | screen-2 | **Use case** — "How do you plan to use the money?" — auto-advances |
| 4 | screen-4 | **Urgency** — "How soon do you need the money?" — auto-advances |
| 5 | screen-6 | **HEI Interstitial** — Laura quote + dark hero card |
| 6 | screen-7 | **Address** — "Where is your home located?" — mock autocomplete |
| 7 | screen-8 | **Property type** — "How would you describe it?" — "Investment property" → exit; auto-advances |
| 8 | screen-9 | **Debt** — "What is the estimated debt on your home?" |
| 9 | screen-10 | **Name** — First/Last stacked vertically |
| 10 | screen-11 | **Phone** + TCPA disclaimer |
| 11 | screen-15 | **2FA Verification** — 6-digit OTP, auto-advances on completion |
| 12 | screen-12 | **Email** — manual entry + Google/Apple social auth options |
| 13 | screen-13 | **Loading** — 4 items, auto-advances after ~3.8s |
| 14 | screen-14 | **Results** — dynamic amount + urgency-reactive callback copy |

**Exit screen (screen-exit):** Investment property ineligible — graceful dead-end with return home.

**pct map:** `{ 1:7, 3:15, 2:23, 4:31, 6:39, 7:46, 8:54, 9:62, 10:69, 11:77, 15:81, 12:85, 13:92, 14:100 }`

### Tech stack

- **Frontend:** Single-file `prototype/index.html` — all HTML, CSS, and JS in one file
- **Styling approach:** Plain CSS with CSS custom properties (variables) for all design tokens; no Tailwind, no framework
- **JavaScript:** Vanilla JS only; no build step, no npm, no modules
- **Backend / data:** None — all content is hardcoded in JS objects
- **State variables:** `selectedKey` (use case), `selectedAmount`, `selectedUrgency`, `userAddress`, `userDebt`, `userFirstName`, `userLastName`, `userPhone`, `userEmail`, `qualifiedAmount`. Navigation via `goTo(n)` + `goToExit()`.
- **Hosting:** Open `prototype/index.html` directly in a browser; or via `npx serve prototype` on port 3333
- **Build step:** None

### Design system

Full token spec is in [`docs/DESIGN-SYSTEM.md`](docs/DESIGN-SYSTEM.md). All values sourced from Hometap's homepage HTML. Do not change hex values without explicit instruction.

**Nav pattern:** White background (`--bg`), logo left, "Log in" link right, no CTA button. Matches Hometap's mobile app pattern. Do not re-add a CTA to the nav.

**Purple-highlight pattern:** Use `<span style="color:var(--purple)">` to call out key dollar amounts or figures in headlines (e.g. `$600k` on screen-1). Do not apply to body copy or labels.

**Social proof pattern:** Trustpilot proof card used as a compact footnote above the CTA on entry/landing screens. Layout (top to bottom): logo row (green star icon + "Trustpilot" wordmark in `#191919`), star blocks row (five 16px green square stars + rating text), short italic quote, reviewer name only. Card: `border: 1px solid var(--border)`, `border-radius: var(--radius)`, `padding: 10px 14px`. All text at footnote scale (11–13px). Do not expand this into a featured testimonial section.

**CTA pattern:** `.screen-cta` is `position: fixed; bottom: 0` on mobile. On desktop, it switches to `position: sticky; bottom: 0` inside the `.phone-inner` scroll container (required for the phone frame wrapper). Button inside is capped at `max-width: 580px; margin: 0 auto`. Mobile screen container has `padding-bottom: 120px` to prevent content hiding behind the fixed bar. Do not revert to in-flow CTAs.

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

4. **Single-select auto-advances.** Steps 2, 3, 4, and 7 (screen-3, screen-2, screen-4, screen-8) auto-advance on tap after 280ms (enough for visual confirmation). No separate "Continue" button on any selection screen. Do not add a confirm step.

5. **Progress indicator is a single full-width thin line** (`3px`, `--primary` fill, `--primary-mid` track). No step dots, no labels. Advances proportionally across all 14 screens via the `pct` map in JS. Nav is logo left + Log in right — no CTA, no extra elements.

6. **CTAs always above the fold.** `.screen-cta` is `position: fixed; bottom: 0` on mobile; `position: sticky; bottom: 0` on desktop (inside the phone frame). Mobile screen container has `padding-bottom: 120px` to prevent content hiding behind the bar.

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

- Adding a new screen beyond the 14 defined
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

14-screen pre-qual flow + exit screen, demo-ready end-to-end. Session 8 refreshed screen-1 copy: new value-prop headline ("Get up to $600k from your home equity. No monthly payments." with $600k in `--purple`), reworked 01/02/03 step labels/descriptions, and CTA updated to "See my offer →".

### Known issues / things on fire

- iPhone 13 viewport (390×844) not yet validated — scroll check needed on screen-6 (HEI dark hero), screen-14 (results), and screen-15 (2FA) before demo.
- Loading screen (screen-13) resets on browser back-nav — acceptable for prototype.

### Open questions / decisions pending

- Comparison table: expandable "Learn more" rows per option, or keep flat?

### Snippets / reference URLs

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

*Last updated: 2026-05-26 (session 8)*
