# Code Block

For elements that need JavaScript, use a code block: <div data-cb-code>
(that attribute alone marks it — no class, no other bookkeeping). It sits inside a column,
beside other content, or inside '.is-overlay > .is-overlay-content'. To fill
that layer, give the block 'position:absolute;inset:0' — the overlay is
full-screen, the block is not. The code runs live while editing and
runs again on the saved page.

A simple block is complete as written — no wrapper, no ceremony:

<div data-cb-code>
    <h1 id="greet7x">Lorem ipsum</h1>
    <p>This is a code block.</p>
    <script>
        document.querySelector('#greet7x').innerHTML = '<b>Hello World..!</b>';
    </script>
</div>

Ids are safe: when a block is duplicated, the editor gives the copy fresh ids
and updates the references inside its script to match.

For blocks that do more, these rules keep them working through editing:

- The editor re-renders the block on edit, undo and duplicate, and each
  render runs the code again on FRESH elements. Anything that keeps running
  — a timer, an interval, a window/document listener, an animation loop —
  must stop by itself when its elements leave the page. Check isConnected:

    const display = document.querySelector('#clock9k');
    const iv = setInterval(() => {
        if (!display.isConnected) { clearInterval(iv); return; }
        display.textContent = new Date().toLocaleTimeString();
    }, 1000);

- Plain classic <script> tags only — never type="module". To load a library,
  use dynamic import() inside ONE async IIFE wrapping the whole script,
  closed with exactly one })(); at the end — never nest a second wrapper:

    <script>
        (async () => {
            const confetti = (await import('https://cdn.jsdelivr.net/npm/canvas-confetti@1.9.3/+esm')).default;
            // ... the rest of the block's code ...
        })();
    </script>

  A classic library that defines a global loads with a created <script>
  element (check for an existing one first); CSS files the same way with a
  <link rel="stylesheet">. If you read document.currentScript, do it on the
  first line — it is null after the first await.

- Self-contained only: styles inline or in a <style> tag inside the block;
  never touch document-level styles or elements outside the block.
