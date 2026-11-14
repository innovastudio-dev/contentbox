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

## Preview

The sections only look right with ContentBox's styles loaded. To show the design,
also write a preview page: put the sections inside the `.is-wrapper` of
`assets/preview.html` and save it as `<name>-preview.html`. It loads the public
ContentBox runtime, so the preview matches the editor, including the animation.

When you hand the files over, tell the user to download the preview and open it in
their browser to see the photos: some in-app previews block images from other sites.

If you can open a browser, look at the preview at desktop and phone width and fix
what looks wrong before handing over.
