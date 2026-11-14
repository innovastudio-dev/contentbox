# Building Games

Use this for any playable game. First decide the dimension — it changes the
whole toolchain:

- 2D (doodle, flat, sprite, puzzle, casual, "simple game"): the Canvas 2D
  API in a code block. Do NOT use three.js for these — it is heavier, slower
  to write, and fights the flat aesthetic.
- 3D (depth, camera, "3D"): three.js — ALSO load the "3d" reference and
  follow its games section for rendering and assets.

Target scale either way: an arcade mini-game — one scene, one core mechanic,
polished. Refine with the user over turns.

## The skeleton every game shares

Everything lives in one code block (see the "code" reference for the block
contract). Inside it:

- LOOP: requestAnimationFrame with a clamped delta; stop yourself when the
  block is re-rendered:

    let last = performance.now();
    function frame(now) {
        if (!stage.isConnected) return;           // this render was replaced
        const dt = Math.min((now - last) / 1000, 0.05);
        last = now;
        update(dt); draw();
        requestAnimationFrame(frame);
    }
    requestAnimationFrame(frame);

- INPUT: one flags object; keyboard listeners attach on game start (the
  start click also gives focus), ignore keys while the user types elsewhere,
  and detach themselves via the isConnected check. Map pointerdown /
  pointermove on the stage to the same flags so touch works. Do not
  preventDefault on events you do not use — the page must keep scrolling.
- STATE: a small machine — ready / playing / over — with restart resetting
  state, never reloading. Show "click or tap to start".
- HUD: HTML positioned over the stage (score, lives, messages) — real text,
  updated via textContent, never drawn glyph-by-glyph into the canvas.
- SOUND: only if asked; short WebAudio oscillator beeps, no external files.
- ENTITIES: recycle, never allocate per frame — spawn from a pool, move
  off-screen items back in.
- SELF-CHECK: when a view_render tool is offered, screenshot after building
  and after big visual changes, judge it, fix, re-check once.

## 2D games (Canvas 2D)

Setup — crisp on every screen:

<div data-cb-code>
    <div class="game-stage" style="position:relative; width:100%; aspect-ratio:16/10; overflow:hidden; border-radius:8px; background:#fdfdf8;">
        <canvas style="position:absolute; inset:0; width:100%; height:100%;"></canvas>
        <!-- HUD elements here -->
    </div>
    <script>
        (async () => {
            const container = document.currentScript.parentElement;
            const stage = container.querySelector('.game-stage');
            const canvas = stage.querySelector('canvas');
            const ctx = canvas.getContext('2d');
            function fit() {
                const dpr = Math.min(devicePixelRatio, 2);
                canvas.width = stage.clientWidth * dpr;
                canvas.height = stage.clientHeight * dpr;
                ctx.setTransform(dpr, 0, 0, dpr, 0, 0);   // draw in CSS pixels
            }
            new ResizeObserver(fit).observe(stage);
            fit();
            // ... game ...
        })();
    </script>
</div>

Collision at this scale is rectangles and circle distances — no physics
library unless the game truly is physics.

ART DIRECTION — a 2D game must look intentional, not like debug rectangles:
pick a style (hand-drawn doodle, flat pastel, neon minimal) and carry it
through background, entities, HUD typography and even the game-over screen.

- DOODLE STYLE, pure code: draw with strokes, not fills — rounded lineJoin,
  slightly thick lines, and a small random jitter added to each vertex,
  re-rolled only a few times per second (a wobble timer, not every frame) so
  shapes shiver like a flipbook. Paper background (#fdfdf8, faint grid),
  handwritten-feel HUD (a casual system font, slight rotation).
- SPRITES from generate_image, when the style needs real drawings: request
  each sprite "on a plain solid white background, no shadow" in one
  consistent style. Do NOT request transparent backgrounds — image models
  usually fail at true transparency, delivering fake checkerboards or dirty
  edges. Then either:
  - design the game on the SAME background color, so the sprite's white
    simply disappears (the doodle-on-paper trick — zero code); or
  - key it out at load: draw to an offscreen canvas, getImageData, and set
    alpha 0 on pixels close to the corner color (tolerance ~28); cache the
    result and draw that.
- Parallax sells motion cheaply: two or three background layers scrolling at
  different speeds.

## 3D games

Load the "3d" reference and follow its games section. The short version:
the scene shell (environment light, real materials, fog) applies to games
exactly as to scenes — no flat-material arcade fallback — and the 2-3 hero
entities that carry the look (player, collectible, signature obstacle)
deserve generated models, loaded once and cloned per spawn.
