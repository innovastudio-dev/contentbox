# Animation Framework (Motion / data-fx)

Scroll-driven, time-based, and pointer animations for ContentBox content, expressed as **'data-fx-*' attributes**. You build a normal section (box → overlay → container → row → column, per the structure and utility-class docs) and then **add 'data-fx-*' attributes** to animate it.

---

## Core Rules (read first)

1. **Animation lives in attributes, never in 'style'.** Declare motion as 'data-fx-*' keyframes/presets. Do not hand-write 'transform'/'opacity'/'clip-path' into a 'style' as animation. (Static, non-animated inline styles for geometry/color are still fine.)
2. **Progress, not pixels.** Keyframes are percentages of a scene: 'data-fx-0' … 'data-fx-100'. '0' = scene start, '100' = scene end. Resolution-independent.
3. **One system per element.** Never add 'data-fx-*' to an element that already carries other animation attributes or classes.
4. **No CSS 'position:sticky'.** It's broken inside ContentBox (ancestor 'overflow'). For anything pinned — scrollytelling, card stacks, horizontal galleries — use a **pinned section** ('section-pin').
5. **Animate transform/opacity/colors/compound freely; avoid animating layout.** Keyframing 'width'/'height'/'top'/'margin' reflows the page — only do it on absolutely-positioned elements.
6. **Integer percents only.** 'data-fx-33' is valid; 'data-fx-33.5' is not.
7. **Scroll-driven only.** **Content above the fold cannot animate** — a hero is already at progress '100' at page load, so it simply appears, fully rendered. That is expected; do not work around it.

---

## The two scene types

Every animation belongs to a **scene** = one '.is-section'. Two kinds:

### A. Non-pinned scene (in-view / entrance)

A normal section; its scene progresses **as the section arrives**. Progress '0' = its top reaches the bottom of the screen; progress '100' = it is **fully in view**. So 'data-fx-0' → 'data-fx-100' means exactly "animate in as I arrive", at any section height, including the last section on the page. Use for entrances and reveal-on-scroll.

(To keep animating the whole time the section is on screen — a drift or parallax-like *through*-motion rather than an entrance — set 'data-fx-end="bottom top"'.)


<div class="is-section is-box is-section-90 type-poppins">
    <div class="is-overlay" style="background-color:#f4f2ec"></div>
    <div class="is-container is-content-1100">
        <div class="row"><div class="column">
            <h2 class="margin-0" data-fx-0="opacity:0; transform:translateY(40px)"
                data-fx-100="opacity:1; transform:translateY(0px)" data-fx-ease="expoOut">Rises as you arrive.</h2>
        </div></div>
    </div>
</div>

### B. Pinned scene (scrub / scrollytelling)

Add **'section-pin'**. The section stays fixed on screen while the page scrolls through the pin length, and the scene scrubs '0'→'100' across that scroll. Use for content that transforms in place: card stacks, image sequences, horizontal galleries, 3D reveals.

- **'data-fx-length="320"'** — pin length in **vh** (how much scrolling the scene takes). Typical 240–500; longer = slower.


<div class="is-section is-box is-section-100 type-poppins section-pin" data-fx-length="320" data-fx-scrub="0.3">
    <div class="is-overlay" style="background-color:#ece9e1"></div>
    <div class="is-container is-content-900">
        <div class="row"><div class="column">
            <h2 class="margin-0" data-fx-0="opacity:0; transform:scale(.94); clip-path:inset(0% 100% 0% 0%)"
                data-fx-62="opacity:1; transform:scale(1); clip-path:inset(0% 0% 0% 0%)"
                data-fx-100="opacity:1; transform:scale(1)" data-fx-ease="expoOut">Scrubs into view.</h2>
        </div></div>
    </div>
</div>

### Section-level options (on the '.is-section')

| Attribute | Scene | Meaning |
|---|---|---|
| 'data-fx-scrub="0.3"' | both | Smoothing seconds (lag catching up to the scrollbar). '0'/'true' = hard-locked. Default '0.25'. |
| 'data-fx-length="320"' | pinned | Pin length in **vh**. Required to give a pinned scene room. |
| 'data-fx-start="top 85%"' | non-pinned | Where progress '0' is. Default 'top bottom'. |
| 'data-fx-end="bottom 40%"' | non-pinned | Where progress '100' is. Default 'bottom bottom' — the moment the section is fully in view, which is where an entrance should finish. Use 'bottom top' for a *through*-motion that should keep moving until the section leaves. Also '+=600' / '+=100%'. |

> Advanced: 'data-fx-emit' on a section dispatches 'fx:progress' CustomEvents for custom JS/canvas hooks — not needed for normal content.

---

## Where animated content goes (two placement patterns)

Animation attributes attach to whichever element should move. There are **two valid places** to put animated elements, and choosing right is the most common structural mistake:

### 1. Content-path — width-constrained content that animates in place
Put 'data-fx-*' on elements inside 'is-container → row → column'. Use for headings, paragraphs, split text, section color washes — anything that sits within the normal content column. (Both scene-type examples above use this.)

### 2. Overlay-layer — full-bleed layers that fill the box
Put full-viewport animated layers inside **'is-overlay → is-overlay-content'**. Each layer is 'position:absolute;inset:0' (so it fills the section, ignoring the content max-width) and carries its own 'data-fx-*'. Use for card stacks, image sequences/crossfades, parallax backdrops, anything edge-to-edge.

Key structural facts for the overlay pattern:
- **Stack multiple layers** in one 'is-overlay-content', each 'position:absolute;inset:0' with an ascending 'z-index'; each animates independently.
- **Text in a layer: wrap a BLOCK of copy, leave a SINGLE label bare.** A layer holding several text elements that read as one piece of content (a heading with a paragraph, a caption group) gets its own 'is-container → row → column' inside that layer:

<!-- inside a layer: position:absolute;inset:0 + its own data-fx-* -->
<div class="is-container is-content-none v3" style="max-width:none;padding:0">
    <div class="row">
        <div class="column">
            <h3 class="margin-0">Casa Mármore</h3>
            <p class="size-14 margin-0">Identity, signage and print</p>
        </div>
    </div>
</div>

  Keep 'data-fx-*' where it already was: on the layer, or on the text element itself.
  If a wrapped block uses flex to spread its items apart, put the split in the container (one 'row' with two 'column's) instead of making the text elements direct flex children.
  A layer holding ONE short text element (a plate title, an eyebrow label, a counter digit, a number in a rolling strip, one face of a 3D drum) needs **no container**.
  A word deliberately duplicated to build a visual effect (stroke-over-fill, masked split type) is a graphic, not copy.
- **A normal content container can sit *above* the overlay**: give an 'is-container' sibling 'position:relative;z-index:N' (higher than the overlay layers) to float captions/labels over an animated backdrop.

Templates for both patterns are below.

---

## Percent keyframes (the core mechanism)

Put 'data-fx-N' on **any element** in the section (or the section itself). Each is a 'style'-like declaration list; the engine tweens between consecutive percentages.


<div data-fx-0="opacity:0; transform:translateY(70px) scale(.84); clip-path:inset(100% 0% 0% 0%); color:#b7b2a6"
     data-fx-46="opacity:1; transform:translateY(0px) scale(1);   clip-path:inset(0% 0% 0% 0%);  color:#17161a"
     data-fx-100="opacity:1; transform:scale(1.02); color:#2233d6; letter-spacing:12px"
     data-fx-ease="expoOut">TWEEN</div>

- Numbers are **percent of scene progress** (integers 0–100). A property may appear in only some keyframes; each tweens across the frames where it appears.
- The **first keyframe of each property is its resting/initial state**, applied immediately (so 'data-fx-0="opacity:0"' starts hidden).
- **'data-fx-ease'** sets easing for that element (default 'linear').
- The **section element itself** may carry keyframes (e.g. animate its background color).
- **Transform shorthand** is parsed: 'translateX/Y/Z', 'translate(x,y)', 'scale', 'scale(x,y)', 'scaleX/Y', 'rotate', 'rotateX/Y', 'skewX/Y'. 'transform:none' resets. ('perspective' isn't a keyframe — set it as a static inline style on the parent for 3D.)
- **Bare Motion keys** also work: 'data-fx-0="y:40; scale:.9; rotate:-6"' ('x y z scale scaleX scaleY rotate rotateX rotateY skewX skewY'). '%' in 'x'/'y' is relative to the element's own size.

### What can be animated

| Category | Properties | Notes |
|---|---|---|
| Transform | 'translateX/Y/Z', 'scale', 'scaleX/Y', 'rotate', 'rotateX/Y', 'skewX/Y' | Composed into one transform; use static 'perspective' on the parent for 3D. |
| Opacity | 'opacity' | |
| Colors | 'color', 'background-color', 'border-color' | hex / rgb / rgba. |
| Numbers + units | 'letter-spacing', 'stroke-dashoffset', any numeric/length/% property | |
| Compound strings | 'clip-path', 'box-shadow', 'filter', 'background-position', multi-value 'border-radius', 'stroke-dasharray' | **Both keyframe values must share the same structure** (same token count), e.g. 'inset(100% 0% 0% 0%) → inset(0% 0% 0% 0%)'. Mismatched structures snap instead of tweening. |

---

## Easings ('data-fx-ease')

| Name | Feel |
|---|---|
| 'linear' | Constant — default; best for scrubbed scenes. |
| 'smooth' | Gentle smoothstep both ends. |
| 'easeIn' / 'easeOut' / 'easeInOut' | Slow start / finish / both (cubic). |
| 'quadIn' / 'quadOut' / 'quartOut' | Softer in / out / strong deceleration. |
| 'expoOut' | Fast start, long glide — great for entrances. |
| 'backIn' / 'backOut' | Anticipate / overshoot-and-settle. |
| 'bounceOut' | Bouncing landing. |
| 'elasticOut' | Springy, damped oscillation. |

---

## Scroll edge strings ('data-fx-start' / 'data-fx-end', non-pinned)

Format **'"triggerEdge viewportEdge"'** — the point is reached when *that edge of the section* meets *that edge of the screen*. Words: 'top'=0, 'center'=50%, 'bottom'=100%; or a '%' / raw px.

- 'top bottom' (start default): section top reaches viewport bottom (enters).
- 'top 85%': a little later, top reaches 85% down.
- 'bottom bottom' (end default): section bottom reaches the viewport bottom — i.e. it is fully in view. Where an entrance should finish.
- 'bottom top': section bottom leaves the top — keeps running the whole time the section is on screen. For through-motions (drift, parallax-like).
- '+=600' / '+=100%': end a fixed distance after start.

---

## Presets (single-attribute animations)

### 'data-fx-parallax' — depth drift
Element drifts as it scrolls. Value = travel; add '%' for element-relative, else px. Works in any section.

<div data-fx-parallax="-120" style="position:absolute;top:10vh;right:12vw;width:1px;height:120px;background:rgba(0,0,0,.16)"></div>

### 'data-fx-horizontal' — vertical scroll → horizontal track  *(pinned only)*
A flex row wider than the screen scrolls sideways as you scroll down. Cards are 'flex:0 0 auto'.

<div class="is-section is-box is-section-100 section-pin" data-fx-length="360" data-fx-scrub="0.3">
    <div class="is-overlay" style="background-color:#ece9e1"></div>
    <div class="is-container is-content-none" style="max-width:none">
        <div class="row"><div class="column" style="padding:0;overflow:visible">
            <div class="flex" data-fx-horizontal style="gap:24px;padding:0 6vw">
                <div style="flex:0 0 auto;width:min(78vw,560px);height:60vh">Card 1</div>
                <div style="flex:0 0 auto;width:min(78vw,560px);height:60vh">Card 2</div>
            </div>
        </div></div>
    </div>
</div>

### 'data-fx-split="words|chars"' — staggered text (auto-accessible)

| Modifier | Effect |
|---|---|
| 'data-fx-split="words"' / '"chars"' | Split unit. |
| 'data-fx-split-mask' | Each unit rises out of an overflow mask (clean reveal). |
| 'data-fx-split-stagger="0.08"' | Seconds between units (default '0.08' words / '0.018' chars). |
| 'data-fx-split-y="115%"' | Travel distance (default '115%' masked / '18px' plain). |
| 'data-fx-split-scrub' | Brighten units in reading order **as the scene scrubs** (put in a pinned section). |
| 'data-fx-split-from="0.12"' | Scrub mode: starting opacity of dim units. |


<h1 class="margin-0" data-fx-split="chars" data-fx-split-mask style="font-size:clamp(54px,12vw,172px)">Motion, measured.</h1>
Non-scrub plays each time the element re-enters the viewport.

### 'data-fx-typewriter' — scroll types the text
'data-fx-typewriter-until="0.72"' = progress at which typing completes (default '0.85').

<span data-fx-typewriter data-fx-typewriter-until="0.72" style="color:#2233d6">scrollScene({ scrub: 0.3 }) → 1.000 ✓</span>

### 'data-fx-draw' — SVG paths draw themselves
On an '<svg>' (all paths) or a single '<path>'.

<svg viewBox="0 0 120 84" preserveAspectRatio="none">
    <path data-fx-draw d="M10 74 L110 10" fill="none" stroke="#2233d6" stroke-width="2.5"/>
</svg>

### 'data-fx-marquee' — drifting band
Content should be a repeating strip. 'data-fx-marquee-speed="70"' (px/sec, default '90'); 'data-fx-marquee-lean' = skews with scroll velocity.

<div data-fx-marquee data-fx-marquee-lean data-fx-marquee-speed="70"
     style="display:inline-block;white-space:nowrap;font-size:clamp(40px,8vw,104px)"><span>Motion&nbsp;&nbsp;measured&nbsp;&nbsp;</span></div>

### Micro-interactions (pointer)
**'data-fx-tilt'** — 3D tilt toward the cursor. 'data-fx-tilt-max="16"' (degrees, default '14').
**'data-fx-magnetic'** — leans toward the pointer, snaps back. 'data-fx-magnetic-strength="0.4"' (default '0.35').

<a href="#" role="button" data-fx-magnetic data-fx-magnetic-strength="0.4"
   class="transition-all inline-block no-underline border-2 border-solid rounded-full py-3 px-8 size-14 uppercase"
   style="color:#fff;background-color:#2233d6;border-color:#2233d6">Try the pull →</a>

---

## Section templates

Copy-and-fill skeletons. Entrance and basic-pinned are shown under "The two scene types"; below are the fuller structural archetypes.

### Split-text hero (content-path, non-pinned)

<div class="is-section is-box is-section-100 type-poppins">
    <div class="is-overlay" style="background-color:#eef1f5"></div>
    <div class="is-container is-content-1200"><div class="row"><div class="column">
        <p class="size-13 tracking-150 uppercase margin-0" style="color:#5b6779">Field camera</p>
        <div class="spacer height-40"></div>
        <h1 class="leading-09 margin-0 font-extrabold" data-fx-split="chars" data-fx-split-mask
            style="font-size:clamp(50px,11vw,150px);color:#10182b">Made for the light you chase.</h1>
        <div class="spacer height-40"></div>
        <p class="size-19 leading-16 margin-0" data-fx-split="words" style="color:#5b6779;max-width:60ch">Weatherproof, full-frame, built for dawn starts.</p>
    </div></div></div>
</div>

### Pinned card stack — full-bleed layers, one 'is-container' per layer
Each card is a full-bleed layer in 'is-overlay-content'. The first card scales back as the next slides up over it. Add more layers by repeating the block with later percentages and higher 'z-index'.

<div class="is-section is-box section-pin is-section-100 type-poppins" data-fx-length="360" data-fx-scrub="0.3">
    <div class="is-overlay" style="background-color:#eef1f5">
        <div class="is-overlay-content" style="position:absolute;inset:0;overflow:hidden">

            <!-- Layer 1: scales back as layer 2 covers it -->
            <div class="flex items-center justify-center" style="position:absolute;inset:0;z-index:1">
                <div class="overflow-hidden" style="position:relative;width:min(680px,90vw);height:72vh;border-radius:24px"
                     data-fx-0="transform:scale(1)" data-fx-50="transform:scale(.9)" data-fx-100="transform:scale(.9)" data-fx-ease="linear">
                    <img class="w-full h-full object-cover" src="a.jpg" alt="" style="position:absolute;inset:0">
                    <div style="position:absolute;inset:0;background:linear-gradient(to top,rgba(6,10,20,.7),rgba(6,10,20,.15))"></div>
                    <div class="flex items-center justify-center" style="position:absolute;inset:0">
                        <div class="is-container is-content-none" style="padding:0"><div class="row"><div class="column" style="padding:0">
                            <h2 class="text-center margin-0" style="font-size:clamp(30px,6vw,56px);color:#fff">Dream it</h2>
                        </div></div></div>
                    </div>
                </div>
            </div>

            <!-- Layer 2: same block at z-index:2, entering later —
                 data-fx-0="transform:translateY(110%)" data-fx-50="transform:translateY(0%)" data-fx-ease="expoOut"
                 slides up over Layer 1 and holds. Add more layers by repeating this block with higher z-index and later percentages. -->

        </div>
    </div>
</div>

### Pinned image crossfade — animated backdrop + caption floating above
The image stack animates inside 'is-overlay-content'; a normal 'is-container' sits **above** it via 'z-index' for the caption.

<div class="is-section is-box section-pin is-section-100 type-poppins" data-fx-length="400" data-fx-scrub="0.2">
    <div class="is-overlay" style="background-color:#10182b">
        <div class="is-overlay-content" style="position:absolute;inset:0;overflow:hidden">
            <img class="w-full h-full object-cover" src="a.jpg" alt="" style="position:absolute;inset:0;z-index:1">
            <img class="w-full h-full object-cover" data-fx-10="opacity:0" data-fx-40="opacity:1" data-fx-ease="linear" src="b.jpg" alt="" style="position:absolute;inset:0;z-index:2">
            <img class="w-full h-full object-cover" data-fx-55="opacity:0" data-fx-85="opacity:1" data-fx-ease="linear" src="c.jpg" alt="" style="position:absolute;inset:0;z-index:3">
            <div style="position:absolute;inset:0;z-index:20;background:linear-gradient(to top,rgba(6,10,20,.55),transparent 45%)"></div>
        </div>
    </div>
    <!-- caption floats above the overlay -->
    <div class="is-container is-content-1380 is-content-bottom" style="position:relative;z-index:21"><div class="row"><div class="column">
        <p class="size-13 tracking-150 uppercase margin-0" style="color:rgba(255,255,255,.75)">A journey, frame by frame</p>
    </div></div></div>
</div>

---

## Gotchas

- **Pinned needs 'data-fx-length'** (240–500 vh). Without it a 'section-pin' scene has no scroll room.
- **'data-fx-horizontal' only works pinned.**
- **Keyframe percentages are integers 0–100** ('data-fx-33', not 'data-fx-33.5').
- **Match compound-string structure** across keyframes (same 'inset()' token count, same units) or it snaps.
- **3D:** set 'perspective' (static inline) on the parent, 'transform-style:preserve-3d' on the rotating element, 'backface-visibility:hidden' on faces.
- **Full-bleed animated layers → overlay** ('is-overlay > is-overlay-content'); **column content → the container path**. Don't put edge-to-edge absolute layers inside 'is-container' (its max-width will clip them).
- **A photo animates just as well as an '<img>'.** Fill its frame with class 'object-cover' (plus inline 'object-position' to shift the crop) and put the keyframes on the tag. Prefer this for content imagery, which then keeps its 'alt'; where an effect needs a background (a fill painted inside letterforms, a tiling pattern, layered gradients on one element), use one. A reveal is the frame clipping while the image scales inside it:

<div style="position:relative;overflow:hidden;height:min(60vh,540px)" data-fx-10="clip-path:inset(50% 0% 50% 0%)" data-fx-64="clip-path:inset(0% 0% 0% 0%)" data-fx-ease="expoOut">
    <img class="w-full h-full object-cover" src="…" alt="…" style="position:absolute;inset:0;object-position:54% 66%" data-fx-10="transform:scale(1.3)" data-fx-78="transform:scale(1)" data-fx-ease="expoOut">
</div>

- **One system per element** ('data-fx-*' or legacy 'data-t-*', never both).
- **Mobile:** add 'm-pin-off' / 'm-skrollrr-off' to a pinned section to disable pinning where the effect doesn't translate.

---

## Attribute cheat-sheet

**Section:** 'section-pin' · 'data-fx-length' (vh, pinned) · 'data-fx-scrub' · 'data-fx-start' / 'data-fx-end' (non-pinned)
**Keyframes (any element):** 'data-fx-0 … data-fx-100' · 'data-fx-ease'
**Presets:** 'data-fx-parallax' · 'data-fx-horizontal' (pinned) · 'data-fx-split' (+'-mask' '-stagger' '-y' '-scrub' '-from') · 'data-fx-typewriter' (+'-until') · 'data-fx-draw' · 'data-fx-marquee' (+'-speed' '-lean') · 'data-fx-tilt' (+'-max') · 'data-fx-magnetic' (+'-strength')
**Easings:** 'linear smooth easeIn easeOut easeInOut quadIn quadOut quartOut expoOut backIn backOut bounceOut elasticOut'

## Direction

Invent fresh mechanisms by composing the primitives — don't reproduce the templates as-is. A rich page uses 4–6 pinned scrub scenes, each a different mechanism; everything must scrub cleanly forwards AND backwards.

## Advanced constraints (in addition to Core Rules and Gotchas above)

- Inside keyframed transform values use only px, %, deg — never vw/vh (static inline styles may still use vw/vh). Do machinery timing math in px.
- Rolling digit/word strips: animate translateY in % of the element's OWN height, never em/px — font metrics drift and the strip lands off-digit.
- Never animate 'font-weight' (a static cut renders it stepped); for type mass animate '-webkit-text-stroke-width', 'letter-spacing', or 'filter'.
- clip-path applies in the element's LOCAL coords BEFORE transform — a transform drags the clipped hole along. For a clip window that must travel independently (magnifiers, masks): clip keyframes on a stationary wrapper, transform on a child inside.
- 3D cylinder/fold: clip with an 'overflow:hidden' wrapper placed OUTSIDE the perspective element (overflow flattens 3D on the element carrying it). Faces pushed out with translateZ are magnified — give the clip wrapper horizontal padding, or centre/inset the face text, or a word loses its first letter.
- Machinery that must make contact (presses, stampers, needles-on-dials): fixed-px stage, derive every rest/impact keyframe from the same numbers — no viewport units between actuator and target.
- A marker riding a drawn path (route arrows, tracing dots, a slider on a curve): sample the positions and dense mover keyframes (tangent-derived rotation) from the actual Bézier — hand-placed points cut straight chords off the curve.
- Centering an absolutely-positioned element that has a border: add 'box-sizing:border-box' (borders otherwise add outside the width and shift the visual centre).
- Never include the site's navigation shell (top bars, menus) — the CMS provides it.
- Images: use Unsplash unless the user specifies otherwise.

Before delivering, self-review against every rule above.
