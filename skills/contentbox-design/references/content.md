# Content.css Framework

## Grid System

All content lives in rows and columns — NEVER place content directly under a container.

<div class="row">
    <div class="column"><!-- content --></div>
    <div class="column"><!-- content --></div>
</div>

- Custom width: <div class="column" style="width: 52%; flex: 0 0 auto;"> — set ONE column's width and let the other(s) flex. Equal columns: just <div class="column"> each.
- Mobile (< 760px): columns auto-stack full-width.

### Column Gap
- NO gap by default — columns already have 1rem side padding. Add gap ONLY for extra breathing room, and ALWAYS via inline style — never gap-* utility classes.
- Galleries/grids: gap:20px max. Image + text (2 col): gap:30px max.
- Gap takes space, so column widths + gap must not exceed 100%. Give ONE column a width and let the other flex.
  CORRECT: width:55% + a flexing column + gap:20px.
  INCORRECT: width:38% + width:62% + gap:20px → overflow.

## Typography

- Size: 'size-<n>' (px). Available: 12,13,14,15,16,17,18,19,20,21,24,28,32,35,38,42,46,48,50,54,60,64,68,72,76,80,84,88,92,96,100 … up to 400. Anchors: labels 12-14, body 16-18, subheads 20-32, headings 42-60, hero/display 60-96.
- Weight: font-light font-normal font-medium font-semibold font-bold
- Tracking: named steps tracking-tighter tracking-tight tracking-normal tracking-wide tracking-wider tracking-widest, then a numeric scale 'tracking-<n>' where n is a multiple of 25 from 0 to 500 (n/1000 em, so 'tracking-150' = 0.15em). Uppercase labels read best at 150-250; display headings often want tracking-tight.
- Leading: leading-none leading-12 leading-13 leading-14 leading-15 leading-16 leading-17 leading-18 — body: leading-17/18, headings: leading-12/13
- Text utils: uppercase lowercase capitalize text-center text-left text-right — put alignment classes ON the text elements (h/p), NOT on a wrapper div. Keep column content flat.

## Spacing

- Padding: p-0 p-2 p-3 p-4 p-6 p-8 p-10 p-12 (values 0-12, 14, 16, 20). Always pair padding with 'box-border' on divs/cards. Directional: px-* py-* pt-* pb-* pl-* pr-*.
- Spacer (vertical space between rows): <div class="spacer height-<n>"></div>. Heights: 20,40,60,80,100,120,140,160,180,200,220,240,260,280,300.
- NEVER put padding on a row. No p-*, py-*, pt-* or pb-* on <div class="row">. Space BETWEEN rows is a spacer in its own row:
    <div class="row">
        <div class="column">
            <div class="spacer height-40"></div>
        </div>
    </div>
  A spacer is an element the user can click and resize in the editor. Padding on a row is invisible to them — there is no way to select the row and change it — so the layout becomes uneditable.
- Text elements (h1-h6, p) already carry default margins — don't fight them with padding. Use padding for design (cards, borders, backgrounds); use spacers for section breaks.

---

## Buttons

Portable — wrap each in a div inside a column. Utility classes + inline styles for colors. (Users edit button text/colors via the ContentBuilder UI, so keep styling in classes + inline colors.)

BASE (every button): 'transition-all inline-block whitespace-nowrap cursor-pointer no-underline border-2 border-solid mt-2 mb-1 font-normal tracking-normal'
Sizes: sm 'py-1 px-4 size-14 leading-13' · md 'py-2 px-6 size-15 leading-14' · lg 'py-2 px-7 size-16 leading-16'
Variants (add to BASE + a size):
- Filled: 'rounded-full border-transparent' + inline 'background-color'
- Outline: 'rounded-full' + inline 'border-color' set to the SAME colour as the text (never 'border-current': the text colour changes on hover, the border must not)
- Text: 'underline border-transparent' (no 'rounded-full', no horizontal padding)
- Square: same as filled but omit 'rounded-full'
Groups: wrap in 'space-x-1'/'space-x-2'; give every button matching horizontal padding.

Neutral buttons (black, white, grey) take their colours from the button roles, so they follow light/dark: write each colour as 'var(--cb-mode-button…, <light colour>)'.
- '--cb-mode-button' — text of an outline/text button, fill of a filled one
- '--cb-mode-button-on' — text on that fill
- '--cb-mode-button-hover' — hover fill of a filled button
- '--cb-mode-button-soft' / '--cb-mode-button-soft-hover' — fill of a soft grey button and its hover
Inside a box the look sets these values (a light button in a dark box, a dark one in a light box); the colour after the comma is used only outside a box with a look. Write the examples below as they are, in any box.
Brand-coloured buttons (orange, blue…) keep literal colours.

### Hover Colors
Per-button CSS custom properties read by stylesheet rules — do NOT use 'hover:bg-*/text-*/border-*' classes for button colors (fixed palette) or 'onmouseover'/'data-bg' (legacy).

| Aspect | Custom property | data-cb-hover word |
|--------|-----------------|--------------------|
| Background | '--cb-hover-bg' | 'bg' |
| Text | '--cb-hover-color' | 'color' |
| Border | '--cb-hover-border' | 'border' |

'data-cb-hover' MUST list exactly the aspects that set a matching '--cb-hover-*' property — a mismatch invalidates the property and drops it on hover. No hover effect → omit both the properties and 'data-cb-hover'.

Example (filled neutral, hover background):
<a href="#" role="button" class="transition-all inline-block whitespace-nowrap cursor-pointer no-underline border-2 border-solid mt-2 mb-1 font-normal tracking-normal rounded-full py-2 px-6 size-15 leading-14 border-transparent" style="color: var(--cb-mode-button-on, rgb(250,250,250)); background-color: var(--cb-mode-button, rgb(24,24,27)); --cb-hover-bg: var(--cb-mode-button-hover, rgb(63,63,70));" data-cb-hover="bg">Get Started</a>

Example (outline neutral, hover fills):
<a href="#" role="button" class="transition-all inline-block whitespace-nowrap cursor-pointer no-underline border-2 border-solid mt-2 mb-1 font-normal tracking-normal rounded-full py-2 px-7 size-16 leading-16" style="color: var(--cb-mode-button, rgb(24,24,27)); border-color: var(--cb-mode-button, rgb(24,24,27)); --cb-hover-bg: var(--cb-mode-button, rgb(24,24,27)); --cb-hover-color: var(--cb-mode-button-on, rgb(250,250,250));" data-cb-hover="bg color">Learn More</a>

Example (brand colour, literal):
<a href="#" role="button" class="transition-all inline-block whitespace-nowrap cursor-pointer no-underline border-2 border-solid mt-2 mb-1 font-normal tracking-normal rounded-full py-2 px-6 size-15 leading-14 border-transparent" style="color: #ffffff; background-color: #c45414; --cb-hover-bg: #a8460f;" data-cb-hover="bg">Book a Table</a>

### Text Colors

Every box has a light and a dark look (see the Box framework, "Light and dark"). Use any colours the design calls for. Neutral colours (greys, white, black) are written with the role classes, which work in any box and follow the look:

| Class | Use for | Light | Dark |
|-------|---------|-------|------|
| 'text-mode-subtle' | secondary text | #4b5563 | #c8c8c8 |
| 'text-mode-muted' | labels, captions, meta | #6b7280 | #a3a3a3 |
| 'bg-mode-surface' | cards | #ffffff | #121212 |
| 'bg-mode-soft' | soft panels, image placeholders | #f9fafb | #1e1e1e |
| 'border-mode-line' | hairlines, dividers | #e5e7eb | #3c3c3c |
| 'border-mode-line-strong' | strong rules | #333333 | #5a5a5a |
| 'bg-mode-line' | a line drawn as a thin block (1px divider, short dash) | #e5e7eb | #3c3c3c |
| 'bg-mode-line-strong' | the same, stronger | #333333 | #5a5a5a |

The border classes set only the colour; combine them with the border utilities: 'border-b border-mode-line', 'border border-mode-line'.
A decorative line or mark in the same colour as the text (usually content over a background image: a short bar beside a name, a rule above a headline, a dot) follows the text with 'bg-current'. Give a line its own element and set its strength with an opacity class: <div class="bg-current opacity-50" style="width: 40px; height: 1px;"></div>. Normal dividers and hairlines on a page ground use 'border-mode-line' / 'border-mode-line-strong'.
If a box has a soft background ('bg-mode-soft') with white cards on it ('bg-mode-surface'), the cards turn darker than the box in dark mode, which looks wrong. Swap them for the dark look with the 'dark:' classes:
<div class="is-overlay bg-mode-soft dark:bg-mode-surface"></div>
<div class="... bg-mode-surface dark:bg-mode-soft">card</div>

Main text (headings, body) needs no colour: #111 in the light look, #e6e6e6 in the dark one.

Any other colour (accents, brand colours, a coloured area and the text on it) is written as a hardcoded value, e.g. style="color: #c45414", and stays the same in both looks. Neutral colours (greys, white, black) use the role classes, so they follow the look.

### Fixed areas

A small area INSIDE a box (a brand-colour band, a coloured card, a dark panel that must stay dark) can keep its colours in both looks, together with everything on it: give that area its own 'data-mode="light"' (light area) or 'data-mode="dark"' (dark area). The nearest data-mode wins, so plain text and the role classes on the area take its look.

Editorial style: stick to the neutral roles; use color sparingly for accents.

---

## Animation

- Transition: transition-none transition transition-colors transition-opacity transition-shadow transition-transform transition-all
- Duration: duration-75 duration-100 duration-150 duration-200 duration-300 duration-400 duration-500 duration-600 duration-700 duration-800 duration-900 duration-1000 duration-1500
- Easing: ease-linear ease-in ease-out ease-in-out
- Hover transform: hover:scale-105 hover:-translate-y-1 hover:-translate-y-2 hover:translate-x-1 hover:translate-x-2
- Hover shadow: hover:shadow-sm hover:shadow hover:shadow-md hover:shadow-lg hover:shadow-xl hover:shadow-2xl
- Hover colours: for neutral colours use the '--cb-hover-bg' / '--cb-hover-color' / '--cb-hover-border' properties with 'data-cb-hover' (see Buttons) and role values, e.g. 'style="--cb-hover-bg: var(--cb-mode-soft, #f9fafb);" data-cb-hover="bg"' — they work on any element.
- Hover opacity: hover:opacity-70 hover:opacity-75 hover:opacity-80 hover:opacity-90 hover:opacity-95 hover:opacity-100
- Keyframes: spin ping pulse bounce

Put the classes on the element that should move. Common combos:
- Card lift: 'transition-transform duration-300 hover:-translate-y-2' (add 'hover:shadow-xl' for lift+shadow)
- Card zoom: 'transition-transform duration-300 ease-out hover:scale-105'
- List item shift: 'transition-transform duration-300 ease-out hover:translate-x-2'

Example (card lift):
<div class="p-10 box-border bg-mode-surface border border-mode-line transition-transform duration-300 hover:-translate-y-2"><!-- card content --></div>

## Images

Use this pattern for all images (galleries, featured, portfolio):
<div class="w-full overflow-hidden relative bg-mode-soft" style="aspect-ratio:16/9">
    <img src="..." alt="..." class="w-full h-full object-cover transition-transform duration-1500 hover:scale-105">
</div>
- 'overflow-hidden' contains the zoom · 'bg-mode-soft' is the load placeholder · 'object-cover' fills the frame · use 'duration-1500' for larger images.
- aspect-ratio: 1/1, 3/4, 2/3, 4/3, 3/2, 16/9, 21/9, 9/16 as needed.
- Add 'pb-6' to columns holding images so they space correctly when stacked on mobile.

## Common Patterns

Section label (follows the light/dark look on its own):
<div class="row"><div class="column"><p class="uppercase tracking-150 size-12 text-mode-muted">Section Label</p></div></div>

Card — use SPARINGLY. Text sitting directly on the page is the default and suits most content; a card is for something that is genuinely a distinct object, such as a pricing tier or one item in a set of three. A page of stacked cards reads as a template, not as a designed page.
<div class="p-12 box-border bg-mode-soft"><p class="size-18 leading-17 pb-4">Title</p><p class="size-14 text-mode-subtle">Secondary text</p></div>

A dark surface that stays dark in both looks is a fixed area. It is optional — many pages want none. Use one only where the design genuinely calls for contrast. The outline marks any row that already has one ('surface=dark'), so check before adding a second.
<div class="p-10 box-border" data-mode="dark" style="background: #18181b;"><p class="size-18 leading-17 pb-4">Title</p><p class="size-14 text-mode-subtle">Secondary text</p></div>

Rows and columns are layout only and never carry a background — a surface is always a div INSIDE the column. Inside a fixed area, plain text and the role classes take that area's look (light text in a dark area).

Numbered feature:
<div class="column"><p class="size-14 font-semibold tracking-150 text-mode-muted pb-4">01</p><h3 class="size-32 font-normal leading-13 pb-4">Feature Title</h3><p class="size-16 leading-17 text-mode-subtle">Feature description.</p></div>

Icons: use Bootstrap Icons (NOT emojis), wrapped in a div: <div class="text-center pb-6"><i class="bi bi-lightning size-42"></i></div>

## Best Practices

- Structure: always '.row' → '.column'; never place content directly under the container; wrap links in a '<p>'/'<div>'; keep layout FLAT — no rows/columns nested inside a column.
- Horizontal layout inside a column: use flexbox, NOT a nested grid — 'flex justify-between' / 'justify-center' / 'items-center' / 'gap-4'.
  INCORRECT: '<div class="column"><div class="row">…nested…</div></div>'
- Column spacing: rely on the default 1rem side padding; don't add px-*/pl-*/pr-* to columns (use inline gap for more). Add 'pb-6' to image columns for mobile stacking.
- Vertical alignment: to align content by column height use flexbox ('flex flex-col justify-center' / 'justify-end'); use spacers ONLY for deliberate editorial offset, never for responsive alignment.
- Text spacing: trust default margins on h/p; for bordered rows 'pb-2 pt-2' is enough; reserve generous padding (p-8…p-12) for design containers (cards, backgrounds); use spacers for major section breaks.
- Typography hierarchy: large headings size-48-96 'font-light leading-12'; medium size-28-42 'font-normal leading-13'; body size-16-18 'text-mode-subtle leading-17'; labels size-12-14 'uppercase tracking-150 text-mode-muted'.
- Minimalist: generous whitespace; neutral roles only; light heading weights; hierarchy through size and spacing, not color.
