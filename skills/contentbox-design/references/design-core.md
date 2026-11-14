# Design Core

## Design Freedom Within Constraints

The frameworks provide utility classes for structure and typography. To create rich, varied designs:

1. **Use framework classes** for all structural elements (sections, boxes, containers, typography, spacing)
2. **Add inline styles** for colors, backgrounds, gradients, shadows, and transforms

## CRITICAL: Output Format

**DO NOT include:**
- '<!DOCTYPE>', '<html>', '<head>', '<body>' tags
- External CSS files or '<link>' tags
- The frameworks are already loaded in the system

**DO create:**
- '<div class="is-section">' blocks with complete content
- Only the HTML sections that will be inserted into an existing page

**Example structure:**

<!-- Section 1 -->
<div class="is-section is-box type-system-ui is-section-100" data-mode="light">
    <!-- Content -->
</div>

<!-- Section 2 -->
<div class="is-section is-box type-system-ui is-section-80" data-mode="light">
    <!-- Content -->
</div>

<!-- Section 3 -->
<div class="is-section is-box is-section-100 type-system-ui" data-mode="light">

    <div class="is-container leading-14 size-18 is-content-1100">

        <div class="row">
            <div class="column">

                <!-- GUIDANCE: Use this layout to show images at any aspect ratio with a hover scale effect -->
                <div class="w-full overflow-hidden relative bg-mode-soft" style="aspect-ratio:16/9">
                    <img src="https://images.unsplash.com/photo-1497366811353-6870744d04b2?w=1200&q=80&auto=format&fit=crop" alt="Featured Project" class="w-full h-full object-cover transition-transform duration-1500 hover:scale-105">
                </div>

            </div>
        </div>

        <div class="row">

            <div class="column">

                <div class="p-12 bg-mode-soft hover-lift">
                    <p class="size-18 leading-17 pb-8">The level of professionalism and expertise demonstrated throughout the project was outstanding. Highly recommend to anyone looking for quality work.</p>
                    <p class="size-14 font-semibold pb-1">Michael Chen</p>
                    <p class="size-13 text-mode-subtle tracking-wide">Founder, Innovation Labs</p>
                </div>

            </div>
            <div class="column">

                <!-- Content -->

            </div>
        </div>
        
        <div class="row">
            <div class="column">

                <!-- Content -->

            </div>
        </div>
    </div>
</div>

## QUALITY BAR
- AWWWards / Codrops level art direction. Storytelling & editorial flow (asymmetry, breathing room, expressive hierarchy) — NOT a conversion landing page, unless I say otherwise.
- Light mode (white/paper backgrounds) unless I say otherwise.
