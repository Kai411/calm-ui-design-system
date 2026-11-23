---
name: calm-ui-design-system
description: "Use when building or restyling a web app UI: first offers color scheme and font pairing choices in chat, then applies a calm token-based design system (type, motion, components, dark mode, mobile)."
---

# Calm UI design system

A general design system for product UIs (dashboards, trackers, internal tools, chat apps), distilled from a shipped app. Character: tinted paper neutrals, ink-dark text, one accent colour, dense and quiet. Motion is short, ease-out and never bouncy (Emil Kowalski style). Colour carries meaning (status, unread, danger), not decoration.

## Step 1. Let the user choose (always do this first)

Before writing any styles, offer the two choices below in the chat and wait for the answer. If a multiple-choice question tool is available (for example AskUserQuestion), use it, one question for colour and one for fonts; otherwise send one short message with two numbered lists. Mark one option as recommended based on what is being built. Always allow "custom": a brand hex colour (convert it to OKLCH and use its hue and chroma as the accent) or any Google Fonts pair.

If the project already has colours or fonts, ask whether to keep them before offering the lists. If the user says "you choose", pick the recommended ones and say which.

### Colour schemes

Show each with its approximate hex swatches (background / ink / accent, then dark background / dark accent).

| # | Name | Mood | Light bg / ink / accent | Dark bg / accent | Hs | Hi | c | Accent light (L C H) | Accent dark (L C H) |
|---|---|---|---|---|---|---|---|---|---|
| 1 | Warm paper | warm, editorial, friendly | #f8f7f4 / #1e1a15 / #e04f1a | #141210 / #ec6d3d | 85 | 70 | 0.004 | 62% 0.19 38 | 68% 0.17 40 |
| 2 | Cool slate | crisp, technical, trustworthy | #f4f7fb / #151b23 / #366bd3 | #101317 / #6496f4 | 250 | 255 | 0.006 | 55% 0.17 262 | 68% 0.15 262 |
| 3 | Forest | calm, natural, finance or health | #f5f8f5 / #161d17 / #187c49 | #101411 / #51b67a | 150 | 150 | 0.005 | 52% 0.12 155 | 70% 0.13 155 |
| 4 | Plum | creative, premium | #f9f6f9 / #1e181f / #9240a4 | #141215 / #c77dd8 | 320 | 320 | 0.005 | 52% 0.17 320 | 70% 0.15 320 |
| 5 | Ochre | earthy, crafted | #f9f7f3 / #211912 / #9a5f00 | #16120f / #e4a339 | 80 | 60 | 0.006 | 52% 0.13 70 | 76% 0.14 75 |
| 6 | Mono ink | minimal, content-first | #f7f7f7 / #1b1b1b / #1b1b1b | #131313 / #eeeeee | 0 | 0 | 0 | = ink | = ink |

Hs = hue of the surfaces, Hi = hue of the ink, c = neutral chroma.

### Font pairings (all on Google Fonts)

| # | UI / body | Mono | Feel |
|---|---|---|---|
| 1 | Hanken Grotesk 400/500/600/700 | JetBrains Mono 400/500 | warm, slightly geometric (the original) |
| 2 | Geist 400/500/600/700 | Geist Mono 400/500 | neutral, modern, technical |
| 3 | Instrument Sans 400/500/600/700 | IBM Plex Mono 400/500 | sharp, compact, good for dense tables |
| 4 | DM Sans 400/500/600/700 | DM Mono 400/500 | soft, round, friendly |
| 5 | Manrope 400/500/600/700 | Fira Code 400/500 | open, airy, good on phones |
| 6 | Fraunces 600 (h1/h2 only) + Inter Tight 400/500/600 | JetBrains Mono 400/500 | editorial headings, plain UI |

Load with one Google Fonts link using `display=swap`. If the chosen font has no 550/650 weights, round to 500/600.

## Step 2. Generate the tokens

Fill this template from the chosen row (Hs, Hi, c, accent). Status colours stay the same for every scheme because they carry meaning.

```css
:root {
  --font-sans: '<UI font>', ui-sans-serif, system-ui, sans-serif;
  --font-mono: '<Mono font>', ui-monospace, monospace;

  --bg:          oklch(97.6% c Hs);
  --surface:     oklch(99.4% c/2 Hs);     /* cards, inputs, modals */
  --surface-2:   oklch(95.4% c*1.25 Hs);  /* hover, tags, segmented track */
  --surface-3:   oklch(92.6% c*1.5 Hs);   /* active nav item */
  --line:        oklch(89.5% c*1.75 Hs);  /* card borders, dividers */
  --line-strong: oklch(82% c*2.25 Hs);    /* control borders */
  --ink:   oklch(22% c*3 Hi);
  --ink-2: oklch(43% c*3 Hi);
  --ink-3: oklch(54% c*2.5 Hi);           /* faint text, keep 4.5:1 on --bg */

  --accent:      oklch(<accent light>);
  --accent-ink:  oklch(99% 0.01 Ha);      /* text on accent */
  --accent-soft: oklch(94% 0.04 Ha);      /* selection, highlights */
  --focus: oklch(<accent light> / 0.35);
  --scrim: oklch(20% 0.01 Hi / 0.32);

  --st-todo:     oklch(60% 0.01 Hi);
  --st-progress: oklch(62% 0.14 245);
  --st-review:   oklch(70% 0.15 75);
  --st-done:     oklch(58% 0.13 155);
  --st-warn:     oklch(64% 0.17 45);
  --st-muted:    oklch(46% 0.05 265);
  --danger:      oklch(57% 0.2 27);
  --danger-soft: oklch(95% 0.03 27);

  --radius-s: 6px;  --radius: 10px;  --radius-l: 14px;
  --shadow-1: 0 1px 2px oklch(22% 0.02 Hi / 0.06);
  --shadow-2: 0 1px 2px oklch(22% 0.02 Hi / 0.05), 0 8px 24px -6px oklch(22% 0.02 Hi / 0.12);
  --shadow-3: 0 2px 4px oklch(22% 0.02 Hi / 0.06), 0 24px 48px -12px oklch(22% 0.02 Hi / 0.24);

  --control-h: 34px;  --control-h-sm: 28px;
  --ease-out:    cubic-bezier(0.23, 1, 0.32, 1);
  --ease-in-out: cubic-bezier(0.77, 0, 0.175, 1);
  --ease-drawer: cubic-bezier(0.32, 0.72, 0, 1);
  color-scheme: light;
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg: oklch(18.5% c*1.5 Hi);  --surface: oklch(21.5% c*1.5 Hi);
    --surface-2: oklch(25% c*1.75 Hi);  --surface-3: oklch(29% c*2 Hi);
    --line: oklch(30% c*2 Hi);  --line-strong: oklch(38% c*2.25 Hi);
    --ink: oklch(95% c*1.25 Hs);  --ink-2: oklch(78% c*2 Hs);  --ink-3: oklch(62% c*2 Hs);
    --accent: oklch(<accent dark>);  --accent-ink: oklch(18% 0.02 Ha);  --accent-soft: oklch(30% 0.06 Ha);
    --danger-soft: oklch(30% 0.07 27);
    --scrim: oklch(0% 0 0 / 0.6);
    --shadow-2: 0 1px 2px oklch(0% 0 0 / 0.3), 0 8px 24px -6px oklch(0% 0 0 / 0.5);
    --shadow-3: 0 2px 4px oklch(0% 0 0 / 0.3), 0 24px 48px -12px oklch(0% 0 0 / 0.6);
    color-scheme: dark;
  }
}

@media (pointer: coarse) { :root { --control-h: 40px; --control-h-sm: 36px; } }
```

Ha = accent hue. Write the computed numbers, not the formulas. Mono ink: accent = ink, accent-ink = bg, accent-soft = surface-3, in both modes. Custom brand colour: keep its hue, set light accent L 52 to 62% and dark accent L 68 to 76%, and check that `--accent-ink` on `--accent` reaches 4.5:1 (darken the light accent until it does). If the project uses another framework (Tailwind, CSS-in-JS), map these names into its theme config instead of raw CSS.

## Step 3. Apply the system

### Typography

| Use | Size / weight | Notes |
|---|---|---|
| Body | 14px / 400, line-height 1.5 | antialiased |
| h1 (page title) | 22px / 650, lh 1.25 | letter-spacing -0.015em, text-wrap: balance |
| h2 (section) | 16px / 600, lh 1.35 | -0.01em |
| h3 | 13px / 600 | |
| Eyebrow (above titles, sidebar groups) | 11.5px / 600 uppercase, 0.06em | --ink-3 |
| Buttons, nav, menu items | 13.5px / 550 | |
| Labels, meta, status | 12.5px / 550 | labels in --ink-2 |
| Small (hints, tags, times) | 11.5 to 12px | --ink-3 |
| Mono | 12px, -0.01em | ids, code |

Three text colours only (ink, ink-2, ink-3). Aligned numbers use tabular-nums. Paragraphs use text-wrap: pretty.

### Layout and spacing

- Spacing values from 4/6/8/10/12/14/16/20/24/32. Card padding 16 to 20, row padding 11px 14px, control gaps 6 to 8, section gaps 24.
- Page: max-width 1120px, centred, padding 28px 32px 64px (phones 20px 16px 48px). Page head: title left, actions right, 24px below.
- App shell: grid `248px minmax(0, 1fr)`, sticky full-height sidebar on --bg with a 1px right border. Under 900px it becomes a 272px drawer (shadow-3 over a scrim) and a sticky top bar appears.
- Cards: --surface, 1px --line border, --radius, no shadow.

### Motion

- Entering and responding: --ease-out. Leaving and colour changes: plain ease. Sheets and drawers: --ease-drawer.
- Durations: hover/colour 100 to 160ms; menus 150ms in, 100ms out; modals 200 to 220ms in, 150ms out; toasts 260ms in, 160ms out; sheets 380ms in, 220ms out; drawer 320ms.
- Never animate from zero: modals from scale(0.96), menus from scale(0.97) translateY(-2px) with transform-origin at the trigger, toasts from translateY(12px) scale(0.98), new list items from translateY(6px), route changes from translateY(4px).
- Exits are faster than entrances and just fade.
- Press feedback: buttons scale(0.97) on :active, chips 0.94, icon buttons 0.9.
- Transitions name exact properties, never `all`.
- Hover styles only inside `@media (hover: hover) and (pointer: fine)`.
- prefers-reduced-motion: durations to 0.01ms and no press scale.
- No bounce, no overshoot, no decorative loops (spinners and a 1.4s linear skeleton shimmer only).

### Components

- **Button**: height --control-h, padding 0 14px, --radius-s, 1px --line-strong border, --surface fill, shadow-1, 13.5px/550, icon gap 6. Primary = ink fill with --bg text (the main action per screen). Accent = accent fill, used sparingly. Ghost = no border, fill or shadow, --ink-2, hover --surface-2. Danger = default button with danger text. Small = 28px, padding 0 10, 12.5px. Icon = square. Disabled = opacity 0.5. A button next to a field uses the same size.
- **Field**: label 12.5px/550 --ink-2 above with 6px gap. Input/select/textarea: height --control-h, padding 6px 10px, --radius-s, 1px --line-strong, --surface. Focus: accent border plus `0 0 0 3px var(--focus)`. Placeholder --ink-3. Selects draw their own chevron (appearance: none). Textarea min 88px.
- **Status**: 8px dot with a 3px halo of its own colour at 18% (`box-shadow: 0 0 0 3px color-mix(in oklch, var(--dot) 18%, transparent)`) beside 12.5px/550 text.
- **Tag**: 22px pill, padding 0 8px, --surface-2 fill, 1px --line, 12px --ink-2.
- **Badges**: count = accent pill, min 20px, 11.5px/650, --accent-ink. Mention/flag = 20px circle, accent 14% fill, accent 45% border, accent text.
- **Segmented control**: track --surface-2, padding 2, radius 8; options radius 6, 12.5px/550 --ink-2; selected = --surface, ink, shadow-1.
- **Tabs**: 13.5px/550 --ink-3, gap 20, 1px --line bottom border; current tab ink with a 2px rounded ink underline.
- **Sidebar link**: 32px tall, radius 7, 13.5px/500 --ink-2; hover --surface-2; current --surface-3 and ink. Optional 9px colour swatch (radius 3). Keyboard hints in a small bordered kbd, hidden on touch.
- **List row**: grid `auto minmax(0, 1fr) auto`, gap 12, padding 11px 14px, 1px --line between rows inside one card. Title 14px/550 with ellipsis (650 when unread), 12.5px meta line below; right side date, badge, avatars. Finished items turn --ink-3, never strikethrough. Hover --surface-2.
- **Chat**: rows, not bubbles. 30px avatar + body (grid `30px minmax(0, 1fr)`, gap 11); name 13px/600 + time 11.5px faint; text 14px/1.55. Same author within 5 minutes merges into one group. Each message is its own hover target (radius 8, hover --surface-2) with a floating tool bar (surface, radius 9, shadow-2) top right; on touch a tap opens a bottom action sheet instead. Day dividers: centred 11.5px/600 label between 1px lines; an "Unread" divider uses the accent. Reactions are 24px pills, yours in --accent-soft with an accent border. Images max 320 wide, 260 tall, radius 10, 1px border.
- **Composer**: one rounded bordered box: icon buttons left, text field, accent Send button right, faint hint line below on desktop.
- **Modal**: overlay grid on --scrim with padding 24 and `grid-template-columns: minmax(0, 1fr)`. Panel --surface, 1px --line, --radius-l, shadow-3, max-height calc(100dvh - 48px). Header 16px 16px 12px 20px (title + close icon); body 4px 20px 20px and scrolls; footer right-aligned (ghost Cancel, primary action) with a 1px top border. Side-sheet variant docks right with an 8px inset.
- **Menu**: min-width 190, padding 4, --radius, shadow-2, 6px below the trigger; items 7px 9px, radius 6, 13.5px, hover --surface-2.
- **Toast**: bottom centre, --radius, shadow-3, 13.5px/500, 7px tone dot, max-width min(440px, 90vw).
- **Avatar**: initials in a circle with a hue per person: light `oklch(90% 0.045 h)` fill / `oklch(38% 0.08 h)` text; dark `oklch(36% 0.06 h)` / `oklch(90% 0.05 h)`.
- **Empty state**: one --ink-3 sentence inside the card, padding 18px 16px, optional action button right. No illustrations.
- **Loading**: skeleton blocks with a --surface-2 to --surface-3 shimmer, --radius-s.
- **Coloured callout or pill**: `background: color-mix(in oklch, var(--tone) 10%, var(--surface)); border: 1px solid color-mix(in oklch, var(--tone) 45%, transparent); border-radius: var(--radius)`, small uppercase label in the tone mixed 80% with ink.

### Rules

1. No thick left accent borders on blocks, pills or cards (no `border-left: 3px solid`, no inset left shadow). Use a soft tint plus an even 1px border all round.
2. One accent. It means "act here" or "new for you" (unread, send, selection, focus). Status colours appear only as dots or tints.
3. The primary action is the ink button, not the accent.
4. Shadows only on floating things: shadow-1 on buttons, shadow-2 on menus, shadow-3 on modals, toasts, drawers.
5. Focus always visible: `:focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; }`. `::selection { background: var(--accent-soft); }`.
6. Grid columns use `minmax(0, 1fr)`, never bare `1fr`, so long text truncates instead of widening the page.
7. User text wraps: `overflow-wrap: anywhere` on p, li, dd, td; `body { overflow-x: clip }` as a safety net (clip keeps sticky headers working).
8. Desktop scrollbars thin and quiet (12px, 3px transparent border, rounded --line-strong thumb, no arrows); phones keep native overlay bars.

### Dark and light

- Follow the system with prefers-color-scheme unless the user asks for a toggle (then put the dark block under `[data-theme='dark']` as well).
- Every colour goes through tokens, so components need no dark overrides except their own hue maths (avatars).
- Surfaces get lighter as they rise in dark mode. The accent is lifted and its text flips dark; the primary button inverts by itself.

### Mobile

- Breakpoints: 900px (sidebar to drawer), 720px (page padding), 640px (rows drop secondary meta).
- Touch controls grow to 40/36px through the tokens.
- On `(pointer: coarse)` force every text input, textarea and select to 16px with !important so iOS never zooms on focus.
- Date and time inputs: `appearance: none; min-width: 0; max-width: 100%` so iOS doesn't push sheets sideways.
- Hover-only UI is either always shown on touch or replaced by a tap action sheet.

## Step 4. Check before finishing

- Render the main screens in light and dark, desktop and about 390px wide, and look at them.
- Check text contrast (ink-3 on bg, accent-ink on accent) reaches 4.5:1.
- Check no element is wider than the viewport at 320px.
- Tell the user which scheme and fonts were applied and where the tokens live, so they can switch later.
