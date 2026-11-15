---
name: contentbox-design
description: Design web page sections as ContentBox HTML, the format the ContentBox editor opens and edits. Use when the user asks for a page, landing page, header, hero or section "for ContentBox", or for HTML they will paste into a ContentBox page. Covers layout, typography, scroll animation, code blocks, three.js 3D scenes and browser games.
---

# ContentBox design

You write page sections in ContentBox's own HTML format: `<div class="is-section">`
blocks styled with ContentBox's framework classes. The user pastes them into a
ContentBox page (or opens them in the editor) and keeps editing them there.

## Read the references first

Read the files below before writing any HTML. Load only the ones the request needs.

| File | Read it when |
|------|--------------|
| `references/design-core.md` | Always. It sets the output format and the quality bar. |
| `references/content.md` | Always. Grid (rows, columns), typography, spacing, buttons, colours, images. |
| `references/box.md` | Always. Sections, boxes, backgrounds, overlays, container width and position, `type-*` font sets. |
| `references/animation.md` | The page should move: scroll scenes, pinned scenes, split text, parallax, typewriter. |
| `references/motion-path.md` | An object should travel along a curve through the page. Read with `animation.md`. |
| `references/code.md` | The page needs its own JavaScript (a calculator, a chart, an interactive widget). |
| `references/3d.md` | A three.js scene, 3D object or WebGL visual. Read with `code.md`. |
| `references/games.md` | A playable browser game. Read with `code.md`; for a 3D game also `3d.md`. |

The references were written for ContentBox's built-in assistant, which sometimes
has image and 3D model generation tools. If you do not have a tool they mention,
follow their fallback: build objects from code and use photo URLs.

## Output

- Only `<div class="is-section ...">` blocks. No `<html>`, `<head>`, `<body>`, and no
  stylesheet links: ContentBox already loads its frameworks.
- Images: Unsplash URLs unless the user gives their own.
- Save the sections to a `.html` file (for example `coffee-roastery.html`) and tell the
  user to paste its content into their ContentBox page.

## Rules

1. Never set `font-family` on text; the section's `type-*` class sets the fonts.
2. Set font size, weight, line height and letter spacing with utility classes
   (`size-*`, `font-*`, `leading-*`, `tracking-*`), never in CSS.
3. For display text, use one size class per screen range
   (`size-120 md-size-84 sm-size-68 xs-size-50`), never `clamp()`.
4. Never put `pointer-events:none` on anything that holds editable text.
5. Never put text inside `data-cb-code` or draw it as SVG, because it cannot be edited.
6. Never put words in a script, because they cannot be edited. A script may only
   animate a number taken from the page text.
7. Never put a `<video>` inside `data-cb-code`. In the overlay, use the
   `video-background` plugin. In a column, use `<video>` with a `<source>` child.
8. Set the page background on `.is-overlay`, nowhere else.
9. Give text on a photo or video a backing that works on any image (panel, soft
   field behind the text), never relying on this image's brightness.
10. When you set a text colour on a parent (a card, a panel), add
    `p, li { color: inherit; }` under the section's own class in its style.
    Otherwise the page's own paragraph colour overrides it.

### For animated sections

11. Build a complex animation inside the overlay (`.is-overlay-content`). Put
    editable content in several small `.is-container`s inside the overlay, each
    covering only its own area (nav, headline, caption, card). Never one full-width
    container holding the whole design: its row tools fall off-screen and everything
    is hard to select.
12. Use a container only for main content the user reshapes (headline, text,
    buttons). Put small animation parts (counter, timecode, scroll hint, label)
    straight in `.is-overlay-content` with no container, so they cannot be broken or
    deleted. Style them with the design's own CSS.
13. Make every effect still work after the user swaps the photo, video and copy.
    Never tie an effect to one spot in the image.
14. Make the first frame a finished picture. Never start with a cover, fog or blinds
    hiding the image.
15. Respect `prefers-reduced-motion`. Give anything that moves by itself a pause
    button.

### For animated series

A series is several photos shown together in a formation that changes on scroll and
moves by itself, eg. in a clear formation in 3D space (depth from size and overlap).

16. Show only the photos or videos. No drawn props (stands, wires, books, boxes,
    furniture).
17. On scroll, move the items from one clean geometric formation to the next, for
    example a loose ring, then a diagonal cascade, then one item brought forward.
18. Show each item as itself in every state, with its own place, size, angle and
    depth. Never pack them close together into one long strip or sheet.
19. Make the items move by themselves while nobody scrolls, in the start and end
    states (a ring turns, a stair spins). A formation that only moves on scroll looks
    dead.
20. On hover over an item, hold the motion. The pause button stops it, never the
    scroll.
21. For video series: Play one video at a time, with the others holding a still
    frame. Two or more playing drops the page to 30fps.
22. When a caption changes with the motion, freeze it while the pointer or text
    cursor is in it, so it stays editable.
23. Never animate `filter: blur` on moving items, because it drops the frame rate.

## Preview

The sections only look right with ContentBox's styles loaded. To show the design,
also write a preview page: put the sections inside the `.is-wrapper` of
`assets/preview.html` and save it as `<name>-preview.html`. It loads the public
ContentBox runtime, so the preview matches the editor, including the animation.

When you hand the files over, tell the user to download the preview and open it in
their browser to see the photos: some in-app previews block images from other sites.

If you can open a browser, look at the preview at desktop and phone width and fix
what looks wrong before handing over.
