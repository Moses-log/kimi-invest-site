# Professional Polish Pass — Design Spec

## Goal
Make `index.html` read as a professionally designed fintech/trading product rather than an "AI-generated landing page," while preserving the site's core visual identity. This is a whole-site pass touching icons and ambient animation only — no layout, copy, color palette, or font changes.

## What stays untouched
- Color palette: `#0A0A0A` background, `#F5E642` accent, all existing CSS custom properties in `:root`
- Font: Inter
- All 13 sections, their layout, and all copy/content
- The Discord Community preview section's emoji (channel icons, chat message emoji) — these mimic real Discord UI and are authentic, not decorative
- Existing `✓` / `✕` / `✦` glyphs used as checkmarks and bullets (already monochrome, already styled with brand color via their containing elements — not a "vibe code" tell)
- All purposeful hover/click/transition animations: button hover states, FAQ accordion expand/collapse, Discord tab switching — normal UI feedback, out of scope

## 1. Icon system
Replace 16 colorful emoji used as structural icons (rendered in each emoji's own native color, since none of the containing elements set a `color` for them — a real inconsistency with the brand palette) with a single consistent SVG line-icon set: 2px stroke, `fill="none"`, `stroke="currentColor"`, no fill colors, so each icon inherits its container's CSS color.

| Location | Current emoji | Container | New treatment |
|---|---|---|---|
| `.benefit-icon` (9 instances, benefits section) | 📡🤖📊🎓👥🔒🚨🏛️🧑‍💼 | 46×46px box, `background: var(--adim)`, `border: 1px solid var(--border-y)` | SVG icon, ~22px, `color: var(--accent)` set on `.benefit-icon` |
| `.ss-icon` (4 instances, signal streams section) | 🤖🚨🏛️🧑‍💼 | standalone, `font-size: 28px` | SVG icon, ~26px, inherits existing `.ss-who.ai/.moses/.gabe`-style coloring already defined nearby, or `var(--accent)` if no existing color class applies |
| `.sig-icon` (4 instances, track record section) | 🟢🔥✂️🔴 | standalone, `font-size: 20px` | SVG icon, ~20px, colored via existing win/loss color classes (`var(--green)` for the win icon, `var(--red)` for the loss icon, `var(--accent)` for the other two) |

Icon-to-meaning mapping (chosen to match each emoji's original intent):
- 📡 (signal/broadcast) → broadcast/antenna icon
- 🤖 (AI bot) → bot/chip icon
- 📊 (portfolio/stats) → bar-chart icon
- 🎓 (education) → graduation-cap icon
- 👥 (community/members) → two-person icon
- 🔒 (security/privacy) → padlock icon
- 🚨 (alerts) → bell/alert icon
- 🏛️ (politician/government tracker) → building/institution icon
- 🧑‍💼 (human investor) → single-person icon
- 🟢 (win signal) → check-circle icon
- 🔥 (hot/active) → flame icon
- ✂️ (trim/exit position) → scissors icon
- 🔴 (loss/sell signal) → x-circle icon

Inline decorative emoji removed (not replaced with icons, since they're embedded in copy, not structural):
- `🔒 Early Access Pricing` (early access banner) → drop the emoji, keep the text
- `🔒 Early Access rate...` (free-tag area) → drop the emoji, keep the text
- `⚠️ Scammer Warning` (disclaimer section) → drop the emoji, keep the text, rely on existing `.scammer-warn-title` styling (already colored/weighted for emphasis)

## 2. Animation tone-down
All four ambient/looping animations get reduced intensity, not removal. Exact before/after values:

| Animation | Current | New |
|---|---|---|
| `#spotlight` mouse-follow radial gradient | `radial-gradient(700px circle ..., rgba(245,230,66,0.025), transparent 70%)` | `radial-gradient(500px circle ..., rgba(245,230,66,0.015), transparent 70%)` — smaller radius, lower opacity |
| `btn-glow` keyframe (trial-badge, free-tag, btn-large, btn-primary) | 3s cycle, glow box-shadow peak ~`0 0 24px var(--aglow)` (exact peak value read from keyframe definition at implementation time) | 4.5s cycle (slower), glow peak reduced ~40% |
| `fc-float` keyframe (hero floating cards) | vertical translateY amplitude (exact value read from keyframe at implementation time, appears to be in the 10-16px range) | amplitude reduced ~50%, cycle duration unchanged |
| `pulse-dot` keyframe | scale pulse (exact value read from keyframe at implementation time) | scale delta reduced ~50%, cycle duration increased slightly (e.g. 2s → 2.5s) |

Implementation note: the plan must read each keyframe's current exact values from the file before writing new ones, since this spec describes the direction (slower, smaller, subtler) rather than guessing exact numbers that may drift from what's actually in the CSS.

## 3. Mobile
Per the standing project rule, any icon container sizing changes get mirrored in the existing `@media (max-width: 600px)` block. Animation value changes (durations, amplitudes) apply globally via the base keyframe definitions, so no separate mobile override is needed for those unless a mobile-specific override of the same keyframe/property already exists (check at implementation time).

## Out of scope
- No layout, copy, color, or font changes
- No changes to the Discord Community preview section's emoji
- No changes to `✓`/`✕`/`✦` glyphs
- No changes to purposeful hover/click/transition animations
- No changes to `portfolio.html`, `stats.html`, `trades.html`
