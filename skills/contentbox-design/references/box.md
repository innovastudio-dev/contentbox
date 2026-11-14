# Box Framework

Page structure and layout. Works with the Content.css Framework (typography, spacing, buttons, rows/columns).

## Architecture

.is-section (page section)
  └── .is-box (layout box)
      ├── .is-overlay (background layer)
      │   └── .is-overlay-bg (background image/color)
      └── .is-container (content area)
          └── .row > .column (content from Content.css)

Hierarchy is fixed: Section → Box → Overlay + Container → Row → Column. Never skip a layer or nest a section inside a section.

## Sections

Full-width divisions: '<div class="is-section type-system-ui">'. Always include ONE 'type-*' typography class.
Height: 'is-section-<n>' (vh) — 10,15,20,25,30,40,50,60,70,75,80,85,90,100, or 'is-section-auto'. e.g. 'is-section-100' = full-screen hero.

### Typography (pick a 'type-*' class by design context)

| Class | Headings / Body | Best For |
|-------|-----------------|----------|
| 'type-system-ui' | System UI | Default, corporate, apps, general |
| 'type-literary-journal' | Lora / Karla | Blogs, essays, publishing, magazines |
| 'type-luxury-fashion' | Cormorant Garamond / Montserrat | Fashion, jewelry, luxury, galleries |
| 'type-brutalist-refined' | Archivo Black / Libre Franklin | Architecture, studios, portfolios, agencies |
| 'type-mono-technical' | JetBrains Mono | Docs, dev tools, SaaS, code-heavy |
| 'type-space-grotesk' | Space Grotesk | Startups, fintech, crypto, modern tech |
| 'type-candy-pop' | Fredoka / Nunito | Kids, games, entertainment, fun brands |
| 'type-blush-bloom' | Cormorant Infant / Quicksand | Weddings, beauty, wellness, feminine |
| 'type-botanical-garden' | Playfair Display / Lora | Gardens, eco, organic, nature |
| 'type-city-guide' | DM Serif Display / DM Sans | City guides, restaurants, urban lifestyle |
| 'type-grand-tour' | Cormorant SC / Raleway | Luxury travel, heritage tourism, museums |
| 'type-cultural-heritage' | Oranienbaum / Source Sans 3 | Cultural institutions, history, academia |
| 'type-spicy-kitchen' | Sriracha / Arvo | Food blogs, restaurants, recipes, cafes |

## Boxes

A section holds one or more boxes on a 12-column grid ('is-box-<n>', n = 1-12, each 8.33%). Mobile < 970px: all boxes stack to 100%.
- Single box (full width): combine on the section — '<div class="is-section is-box type-system-ui is-section-100">'.
- Multi-box (split): the section holds separate boxes — 'is-box-4' (33%), 'is-box-5' (40/60), 'is-box-6' (50/50), 'is-box-7' (60/40), 'is-box-8' (66%), 'is-box-12' (full).

Each box = optional '.is-overlay' (background) + '.is-container' (content):
<div class="is-box is-box-6">
    <div class="is-overlay"><!-- background --></div>
    <div class="is-container"><!-- content --></div>
</div>

## Overlay (Backgrounds)

- Colour: '<div class="is-overlay" style="background-color: rgb(245,245,245);"></div>'
- Image: '<div class="is-overlay"><div class="is-overlay-bg" style="background-image:url('image.jpg'); background-position:50% 60%;"></div></div>'
- Dark overlay (for light text on an image): add '<div class="is-overlay-color opacity-30"></div>' inside '.is-overlay-bg' (opacity-30..-50).
- 'background-position' sets the focal point — 'horizontal% vertical%' (e.g. '50% 60%', '67% 60%').

## Light and dark

Every box offers two looks, switched by the user (Box panel → Mode). Give each box its look with 'data-mode':
- 'data-mode="light"' — dark text on a light ground.
- 'data-mode="dark"' — light text on a photo, video or dark ground.
- Put it on the section when the section is a single box ('<div class="is-section is-box …" data-mode="light">'); in a section of several boxes put it on EVERY box, including a box that only shows an image.
- Colours: any colours the design calls for; neutral ones with the role classes, which follow the look (Content.css → Text Colors). A small area that keeps its look in both: Content.css → Fixed areas.
- Box ground: a white box needs nothing (dark look: #121212). A soft grey ground: 'bg-mode-soft' on the overlay ('<div class="is-overlay bg-mode-soft"></div>').
- Photo or video box with light text: 'data-mode="dark"', and add 'mode-shade' to the shade: '<div class="is-overlay-color opacity-30 mode-shade" style="background-color: #000000;"></div>'. The shade shows in the dark look and is hidden in the light one, where the photo shows as it is.
- The navigation bar follows the box's look on its own.

## Container (Content Area)

Holds content (always '.row > .column' inside); controls width and position.
- Width: 'is-content-<n>' px — 560,580 … 1800 (step 20). Common: 760 narrow text · 980 default · 1200 standard wide · 1400 extra wide · 1740 ultra. Narrow (760-900) for text-heavy, wide (1200-1400) for visual.
- Vertical position — class on the BOX: 'is-content-top' / (default middle) / 'is-content-bottom'.
- Horizontal position — class on the CONTAINER: 'is-content-left' / (default center) / 'is-content-right'. Only shows when the container is narrower than its box.
- Fine-tune: '--offset: x y' on the container (px; +x right, +y down; works in any position, use sparingly).
- Vertical padding: 'content-pt-<n>' / 'content-pb-<n>' (0,10,20 … 80).
- Mobile < 760px: containers center automatically.

'is-content-left/right' guidance: use on FULL-WIDTH heroes; in split sections skip it (content centers naturally) EXCEPT a tiny caption (≤ 'is-content-300').

## Examples

Hero — full-width, image background (light photo, dark text), content anchored bottom:
<div class="is-section is-box type-system-ui is-section-100 is-content-bottom" data-mode="light">
    <div class="is-overlay">
        <div class="is-overlay-bg" style="background-image:url('hero.jpg'); background-position:50% 60%;"></div>
    </div>
    <div class="is-container is-content-1400">
        <div class="row"><div class="column">
            <h1 class="size-72 leading-12">Hero Title</h1>
            <p class="size-18 leading-17">Subtitle</p>
        </div></div>
    </div>
</div>

Split 40/60 — text box | image box:
<div class="is-section type-system-ui is-section-80">
    <div class="is-box is-box-5" data-mode="light">
        <div class="is-overlay bg-mode-soft"></div>
        <div class="is-container is-content-760"><div class="row"><div class="column">
            <h2 class="size-42 leading-13">Text Content</h2>
            <p class="size-17 leading-17">Description…</p>
        </div></div></div>
    </div>
    <div class="is-box is-box-7" data-mode="light">
        <div class="is-overlay"><div class="is-overlay-bg" style="background-image:url('image.jpg'); background-position:67% 60%;"></div></div>
    </div>
</div>

Photo + shade + light text + CTA, a box designed dark (button classes come from Content.css):
<div class="is-section is-box type-system-ui is-section-60 is-content-bottom" data-mode="dark">
    <div class="is-overlay"><div class="is-overlay-bg" style="background-image:url('bg.jpg'); background-position:50% 40%;"><div class="is-overlay-color opacity-30 mode-shade" style="background-color: #000000;"></div></div></div>
    <div class="is-container is-content-1200 text-center"><div class="row"><div class="column">
        <h2 class="size-60 font-light leading-12 pb-6">Start your next chapter.</h2>
        <div><a href="#" role="button" class="transition-all inline-block whitespace-nowrap cursor-pointer no-underline border-2 border-solid mt-2 mb-1 font-normal tracking-normal rounded-full py-3 px-10 size-16 leading-16" style="color: var(--cb-mode-button, rgb(24,24,27)); border-color: var(--cb-mode-button, rgb(24,24,27)); --cb-hover-bg: var(--cb-mode-button, rgb(24,24,27)); --cb-hover-color: var(--cb-mode-button-on, rgb(250,250,250));" data-cb-hover="bg color">Visit Us Today</a></div>
    </div></div></div>
</div>

## Best Practices

- Always include a 'type-*' class on every section — default 'type-system-ui', or a contextual one from the table above.
- Keep the hierarchy Section → Box → Overlay + Container → Row → Column; never skip layers or nest sections.
- Backgrounds: 'background-position' sets the focal point; darken behind light text with 'is-overlay-color opacity-30'..'-50' plus 'mode-shade' (see Light and dark).
- Every box gets 'data-mode' (see Light and dark); no 'is-light-text' / 'is-dark-text' classes.
- Vertical position: heroes → 'is-content-bottom'; centred messages → default middle; headers → 'is-content-top'.
- Mobile is automatic — boxes stack (< 970px), containers center (< 760px); no mobile-specific code needed.
- Box Framework = structure (sections, backgrounds, container width/position). Content.css = content (typography, spacing, buttons, rows/columns). Use both together.
- Optional smooth zoom on an overlay background image:
  <style>.is-overlay-bg{transition:transform 1.5s cubic-bezier(0.25,0.46,0.45,0.94);} .is-box:hover .is-overlay-bg{transform:scale(1.03);}</style>
