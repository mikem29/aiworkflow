# AI Block Interaction Library Recommendations

This companion note to `ai_block_management_spec.md` identifies Drupal 11-friendly JavaScript and CSS libraries that enhance AI-generated blocks without relying on jQuery UI or Node-based build tooling. Each option can be attached through Drupal's `libraries.yml` system or CDN asset management while staying within security guardrails described in the block management spec.

## Selection Principles

- Pure browser delivery (ES module or UMD bundles available via CDN) so Drupal libraries can declare them without Node pipelines.
- No hard dependency on jQuery or other deprecated browser APIs; verified to run cleanly in Drupal 11's modern JS stack.
- Active maintenance, permissive licenses, and accessible patterns suitable for editorial review.
- Provides tangible block-value: animation, sliders, rollovers, parallax, filtering, or other richer UI affordances.

## Recommended Libraries

1. **GSAP 3 + ScrollTrigger addon**  
   - Deep animation toolkit covering timelines, scroll-driven reveals, counters, and SVG effects for hero or feature blocks.  
   - CDN-hosted UMD builds integrate via `core/drupal` behaviors; defensively sandboxed via configuration objects stored in `drupalSettings`.

2. **AOS (Animate On Scroll)**  
   - Attribute-driven motion for fade/slide/zoom effects triggered by viewport entry, ideal for marketing block sequences.  
   - Single CSS/JS pair with no dependencies; initialization fits Drupal behaviors and respects prefers-reduced-motion checks.

3. **Rellax.js**  
   - Lightweight parallax scroller for background imagery or layered blocks to create depth without heavy CPU cost.  
   - Works as a single constructor call inside a Drupal behavior; ships as plain ES5 for broad browser support.

4. **VanillaTilt.js**  
   - Adds subtle 3D tilt and glare hover states to feature cards or CTAs, replacing legacy jQuery rollover plugins.  
   - Distributed as an IIFE/UMD bundle; can be throttled through config to satisfy accessibility and motion settings.

5. **Swiper 11**  
   - Responsive, touch-enabled sliders and carousels for testimonial or product blocks with extensive ARIA support.  
   - Provides standalone bundle with CSS; Drupal library can expose a narrow options schema (slidesPerView, autoplay) through `drupalSettings`.

6. **PhotoSwipe 5**  
   - Modern lightbox/gallery experience for media-heavy blocks, supporting zoom gestures, captions, and keyboard navigation.  
   - Core script is framework-agnostic and initializes from declarative HTML data attributes approved in the behavior catalog.

7. **Micromodal**  
   - Accessible modal/dialog helper for AI blocks that need promo overlays or inline form focus traps.  
   - Delivered as a minified UMD file; integrates with Drupal's focus management policies without global namespace pollution.

8. **Tippy.js (UMD build)**  
   - Tooltip/popover layer for glossary, help, or microcopy enhancements inside blocks, powered by Popper's positioning engine.  
   - Offers `cdn.jsdelivr` bundles that bundle Popper; attach via Drupal library definitions and supply content via sanitized data attributes.

9. **MixItUp 3**  
   - Animated filtering and sorting for portfolio or product grid blocks, providing interactive category toggles.  
   - Requires only a single UMD script; integrates with Drupal behaviors and respects progressive enhancement fallbacks.

10. **ScrollReveal 4**  
    - Scroll-based reveal animations with fine-grained control over origins, durations, and staggered sequences.  
    - Available as UMD/ES module builds; initialize per-block with schema-validated option maps to stay within guardrails.

## Integration Notes

- Define each library under a dedicated entry in `ai_assistant.libraries.yml`, declaring CDN URLs (with SRI) or locally hosted assets.  
- Initialization should live in centralized AI behavior controllers so options can be validated against JSON Schemas before attaching.  
- Respect `prefers-reduced-motion` and Drupal's accessibility linting by providing opt-out toggles and accessible fallbacks.  
- Log library usage via the block provenance metadata to support auditing and coordinated updates across shared blocks.
