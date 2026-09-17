# Research: PayCenter ZTK Demo Carousel

**Phase**: 0 — Research & Unknowns Resolution  
**Date**: 2026-05-07  
**Feature**: [spec.md](spec.md)

---

## Decision 1: Carousel Animation Technique

**Decision**: CSS `transform: translateX()` + `transition` on absolutely-positioned `<img>` elements inside a clipped `<div>` container.

**Rationale**: Pure CSS transitions are hardware-accelerated (GPU compositing), produce zero jitter at 350ms, and require no library. The pattern is: position the incoming image at `translateX(100%)` (off-screen right), then simultaneously transition the outgoing image to `translateX(-100%)` and the incoming image to `translateX(0)`. A single `overflow: hidden` wrapper on the screen area, clips both images during the transition.

**Alternatives considered**:
- *CSS animation keyframes*: More verbose, no meaningful benefit over `transition` for this use case.
- *JavaScript-driven `requestAnimationFrame`*: Unnecessary complexity; CSS `transition` is sufficient and simpler.
- *Absolute-positioned single-image swap (fade)*: Ruled out — the spec requires a slide left/right motion, not a fade.

---

## Decision 2: Phone Frame Rendering

**Decision**: Pure CSS phone frame using styled `<div>` elements — a rounded-rectangle body, top speaker bar, home indicator bar, and inner screen area.

**Rationale**: No image assets needed for the phone frame itself. Pure CSS is infinitely scalable, modifiable, and avoids external dependencies. A dark `#1c1c1e` body with `box-shadow` for depth, rounded corners (`border-radius: 40px`), and a slightly recessed screen area produces a convincing generic phone silhouette.

**Alternatives considered**:
- *SVG phone frame*: More faithful representation but over-engineered for a demo. Would require finding or creating a phone SVG.
- *Phone frame image (PNG/SVG asset)*: Adds an external asset dependency; harder to adjust sizing/proportions later.
- *Third-party CSS phone mockup library*: Violates the "no dependencies" constraint and the "simple tools" intent.

---

## Decision 3: Click Zone Implementation

**Decision**: Transparent `<div>` overlay covering the right 50% of the phone screen area (`position: absolute; right: 0; width: 50%; height: 100%`), with a click event listener. `cursor: pointer` on hover.

**Rationale**: An invisible overlay div is the cleanest, most reliable approach. It works regardless of the image content underneath and doesn't require hit-testing logic against image coordinates. `pointer-events` is isolated to this element, preventing stray clicks on the image content from firing navigation.

**Alternatives considered**:
- *Click event on the screen container with `offsetX` check*: Functionally equivalent but more fragile — the `offsetX` coordinate can behave unexpectedly when the pointer hovers over child elements (the images). The overlay approach is simpler and more reliable.
- *Visible arrow button on right side*: Out of scope per spec (FR-003: "no visible button or arrow is required").

---

## Decision 4: Animation Lock (Rapid-Click Prevention)

**Decision**: Boolean `isAnimating` flag in JavaScript scope. Set to `true` when a transition begins; reset to `false` in a `setTimeout` callback at exactly 350ms (matching the CSS `transition-duration`). Clicks are no-ops when `isAnimating === true`.

**Rationale**: Using `setTimeout` at the known duration (350ms) is more reliable than listening for `transitionend`, because `transitionend` fires once per transitioned property and can fire multiple times if multiple properties are animated. The 350ms value is a constant defined once and shared between the CSS `transition-duration` and the JS `setTimeout`.

**Alternatives considered**:
- *`transitionend` event listener*: Prone to firing multiple times. Would require a counter or `once: true` option, adding complexity.
- *Disabling the click overlay element's pointer-events*: Clean alternative, but requires DOM manipulation each cycle. The flag approach is simpler.

---

## Decision 5: Image Preloading

**Decision**: All four images are declared as `<img>` tags in the HTML with `display: none` (or rendered but hidden by the carousel mechanism) so the browser fetches all of them on page load. No JavaScript preloading logic needed.

**Rationale**: Since there are only 4 images (total ~7MB), preloading all at startup is acceptable and eliminates any pop-in on first navigation. With `file://` protocol and local NVMe storage, load time will be well under the 3-second SC-001 target.

**Alternatives considered**:
- *`new Image()` preload in JS*: Adds boilerplate for zero benefit given small image count.
- *Lazy load on demand*: Would risk a blank screen flash on first navigation to any image, violating SC-003.

---

## Decision 6: Project Deliverable Layout

**Decision**:
```
ZTK Demo Project/
├── index.html
└── images/
    ├── Zelle P2P Enrollment Carousel Image 001.png
    ├── Zelle P2P Enrollment Carousel Image 002.png
    ├── Zelle P2P Enrollment Carousel Image 003.png
    └── Zelle P2P Enrollment Carousel Image 004.png
```

**Rationale**: Single HTML file with inline `<style>` and `<script>` blocks satisfies FR-009 (double-click to open, no server). Images in a sibling `images/` folder satisfy FR-008 (relative paths). The `index.html` name is conventional and universally recognised.

**Alternatives considered**:
- *Separate `.css` and `.js` files*: Violates the spirit of FR-009 — with `file://` protocol, a multi-file project opens correctly, but keeping everything in one HTML file is simpler to share and less prone to broken relative path issues.
- *Base64-embed images in HTML*: Would make the HTML file ~10MB, slow to open in an editor, and hard to update images. Rejected.

---

## All NEEDS CLARIFICATION Items: Resolved

No unknowns remain. All technical decisions above are grounded in the spec clarifications (delivered as "modern, simple web tools") and standard browser capabilities.
