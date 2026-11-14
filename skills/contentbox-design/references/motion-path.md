## Motion Path ('data-fx-path')

Move an element along a smooth curve through waypoints — scroll-driven like all fx: it rides the section's scene (pinned needs 'data-fx-length'; non-pinned uses 'start'/'end') and scrubs cleanly both directions. Use only when there is an OBJECT to travel a path (a plane, a dot, an arrow tracing a route) — typically a story-driven page.

| Attribute | Default | Meaning |
|---|---|---|
| 'data-fx-path="x,y; x,y @tx,ty; …"' | — | Waypoints (min 2), ';'-separated. Optional per-point tangent '@tx,ty'. |
| 'data-fx-path-times="0,…,100"' | even | Progress at each waypoint; one per point, ascending, first '0', last '100' (floats ok). |
| 'data-fx-path-curve="1"' | '1' | Auto-tangent tension for points without '@tx,ty'; '0' = straight polyline. |
| 'data-fx-path-dwell="0.78"' | '.78' | '0' constant pace · '1' full stop at each waypoint · default slows near rests but drifts (never stops). |
| 'data-fx-path-rotate' / '="90"' | off | Auto-rotate along the travel direction; value = offset for the art's resting facing (right '0', up '90', left '180', down '270'). |
| 'data-fx-path-step="1"' | '1' | Sampling density in % (1–10); raise only with many path elements. |

Rules:
- Coords are bare px offsets from the element's own resting position — no units, no '%'/'vw'/'vh'; decimals fine. House pattern: a zero-size origin '<div style="position:absolute;left:50%;top:50%;width:0;height:0">' with the element centred on it via negative half-size offsets, so waypoints read as viewport-centre offsets.
- The path OWNS 'x'/'y' (+'rotate' when '-rotate' is on): literal 'data-fx-N' values for those keys are ignored. All other properties merge — keyframe 'opacity', 'scale', colours, 'clip-path' on the same element freely.
- Tangent '@tx,ty' = velocity through the waypoint; magnitude near the segment length gives full bowed swoops. For an organic "cursive" feel author mostly-downward tangents ('@±60,300'-ish) so the object passes each rest gently descending; side-alternating waypoints with auto tangents hairpin.
- One section = one journey. A path is scoped to its section's scene; an object cannot cross section boundaries. Put the whole journey (including any landing/docking) in one — usually pinned — scene, with the destination element fading in near the end.
- Never another preset on the same element — wrapper carries the path, a child carries any split/marquee/own-spin keyframes.

Auto-rotating arrow that noses along the curve:
<div style="position:absolute;left:50%;top:50%;width:0;height:0">
    <div style="position:absolute;left:-17px;top:-12px;width:34px;height:24px"
         data-fx-path="-440,-80 @200,220; -150,120 @240,-140; 150,-110 @230,150; 440,90 @160,180"
         data-fx-path-rotate>
        <svg viewBox="0 0 34 24" width="34" height="24"><polygon points="0,4 34,12 0,20 7,12" fill="#d63a2f"/></svg>
    </div>
</div>

Path drives position; opacity/scale keyframes ride along:
<div style="position:absolute;left:-14px;top:-14px;width:28px;height:28px;border-radius:50%;background:#d63a2f"
     data-fx-path="-380,60; 0,60; 380,60" data-fx-path-dwell="0.95"
     data-fx-0="opacity:1; transform:scale(1)" data-fx-50="opacity:.3; transform:scale(1.5)"
     data-fx-100="opacity:1; transform:scale(1)"></div>

Canonical pattern — a persistent object while pages pass it. One pinned section; N full-viewport page layers each translate at scrollbar speed ('(k−1)·100%' → '−(N−1)·100%' over a '(N−1)·100'vh pin — layers are 100vh so '%' of own height = viewport heights); the object drifts a path whose waypoint times match the page-centred moments, resting opposite each page's text column:
<div class="is-section is-box is-section-100 type-poppins section-pin m-pin-off" data-fx-length="400" data-fx-scrub="0.25">
  <div class="is-overlay" style="background-color:#ffffff">
    <div class="is-overlay-content" style="position:absolute;inset:0;overflow:hidden">
      <!-- page k of 5 (k = 1..5): text column alternates left/right -->
      <div class="flex items-center" style="position:absolute;inset:0;z-index:10"
           data-fx-0="transform:translateY(0%)" data-fx-100="transform:translateY(-400%)">
        <div class="is-container is-content-1280 v3"><div class="row">
          <div class="column" style="width:46%;flex:0 0 auto"><!-- copy --></div><div class="column"></div>
        </div></div>
      </div>
      <!-- pages 2..5: identical, starting at 100/200/300/400% -->
      <div style="position:absolute;left:50%;top:50%;width:0;height:0;z-index:40">
        <div style="position:absolute;left:-32px;top:-23px;width:64px;height:46px"
             data-fx-path="250,-140 @-120,300; -260,20 @60,320; 240,120 @-60,300; -250,-60 @60,300; 255,60 @-40,280"
             data-fx-path-times="0,25,50,75,100" data-fx-path-rotate><!-- directional artwork --></div>
      </div>
    </div>
  </div>
</div>
