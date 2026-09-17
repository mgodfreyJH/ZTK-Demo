# Research: PayCenter ZTK Demo — Zelle P2P Enrollment Carousel

**Phase**: 0 — Pre-Design Research (updated 2026-05-11 for permission screen)  
**Branch**: `002-paycenter-ztk-demo`

---

## 1. Animation Technique

**Decision**: CSS `transform: translateX` + `transition` property  
**Rationale**: GPU-composited; zero JS animation loop; single property assignment per direction.  
**Alternatives considered**: `left`/`margin` (layout reflow, rejected), Web Animations API (over-engineered for 350ms), GSAP/Anime.js (external dep, rejected).

---

## 2. Debounce Strategy

**Decision**: Boolean `isAnimating` flag; ignore all clicks while `true`  
**Rationale**: Simplest correct implementation. Flag set at animation start, reset in `setTimeout` after 350ms.

---

## 3. Permission Screen State Guard

**Decision**: Separate `isPermissionScreen` boolean flag  
**Rationale**: When the Contacts Permission screen is showing, `currentIndex` stays at 3 (the last carousel image) but the visible content is no longer a carousel slide. `advance()` and `retreat()` must be blocked entirely. A dedicated flag makes the guard explicit and avoids confusion with `currentIndex`.  
**Implementation**: Set `isPermissionScreen = true` when `showPermission()` completes. Guard `advance()` and `retreat()` with `if (isPermissionScreen) return`.

---

## 4. Image Delivery

**Decision**: PNG files in `ZTK Demo Project/images/`, relative paths  
**Status**: All 5 images present (4 carousel + 1 permission).

| File | Dimensions |
|------|-----------|
| Zelle P2P Enrollment Carousel Image 001.png | 750 × 1624 |
| Zelle P2P Enrollment Carousel Image 002.png | 750 × 1624 |
| Zelle P2P Enrollment Carousel Image 003.png | 750 × 1624 |
| Zelle P2P Enrollment Carousel Image 004.png | 750 × 1624 |
| Zelle Access Contacts Permission.png | 147 × 315 |

---

## 5. Permission Image Sizing

**Decision**: `object-fit: contain` with CSS border/shadow suppression  
**Rationale**: Permission image (147×315, ratio 0.467) vs. phone screen area (266×576, ratio 0.462) — nearly identical aspect ratio, fills the screen vertically with negligible letterboxing. No cropping needed.  
The spec says "removing the border from it" — this refers to any browser-default or CSS-added decorations. CSS `border: none; outline: none; box-shadow: none;` on `.slide` ensures clean rendering.

---

## 6. File Structure

**Decision**: Single `index.html` with inline `<style>` and `<script>` blocks  
**Status**: Implemented. No build toolchain.

---

## 7. CTA Hotspot Positioning

**Decision**: Invisible `position: absolute` div at `top: 78%, left: 5%, width: 90%, height: 10%`  
**Rationale**: Estimated position of the GET STARTED button on carousel image 004. `pointer-events` toggled to `all` only when `currentIndex === 3`. May need pixel-level adjustment after visual verification.

**No unresolved items remain.**

**Phase**: 0 — Pre-Design Research  
**Branch**: `002-paycenter-ztk-demo`  
**Date**: 2026-05-11

---

## 1. Animation Technique

**Decision**: CSS `transform: translateX` + `transition` property  
**Rationale**: The browser GPU-composites `transform` changes natively, producing smooth 60fps animations with zero JavaScript animation loop overhead. Combined with a CSS `transition`, the implementation is a single property assignment per direction — no third-party library needed.  
**Alternatives considered**:
- `left`/`margin` positioning — rejected; triggers layout reflow, jank on lower-end hardware.
- Web Animations API (`element.animate()`) — more powerful but unnecessary complexity for a fixed 350ms ease-in-out slide.
- GSAP / Anime.js — external dependency; violates the zero-dependency constraint for a simple demo.

**Implementation detail**: Two `<img>` elements are layered with `position: absolute`. The outgoing image translates to `±100%` while the incoming image translates from `±100%` to `0`. The direction sign (`+` or `−`) is determined by navigation direction. A double `requestAnimationFrame` ensures the browser registers the initial off-screen position before the transition fires.

---

## 2. Debounce Strategy

**Decision**: Boolean `isAnimating` flag; ignore all clicks while `true`  
**Rationale**: Simplest correct implementation. The flag is set to `true` at animation start and `false` in the `setTimeout` callback after 350ms. All click handlers return early when flag is set. This prevents image source and transform state from getting out of sync.  
**Alternatives considered**:
- `pointer-events: none` on zones during animation — equivalent effect, CSS-only, but requires DOM writes; flag is simpler to reason about.
- Event queue / Promise chain — over-engineering for a demo; a queue of clicks is not useful UX.

---

## 3. Image Delivery

**Decision**: PNG files copied to `ZTK Demo Project/images/` and referenced via relative paths  
**Rationale**: The project opens as a local file (`file://`). Absolute Windows paths in `src` attributes are unreliable across machines and browsers. Relative paths from `index.html` to `images/` are portable and require no web server.  
**Alternatives considered**:
- `file:///c:/users/...` absolute paths — machine-specific, breaks if moved.
- Local HTTP server (e.g., VS Code Live Server) — additional tooling dependency not needed for a drag-and-drop demo.

**Current status**: All four PNG files already present in `ZTK Demo Project/images/`.

---

## 4. File Structure

**Decision**: Single `index.html` with inline `<style>` and `<script>` blocks  
**Rationale**: Zero build toolchain, zero module resolution, opens directly from the file system. For a ~150 LOC demo this keeps everything in one file for maximum portability and ease of sharing.  
**Alternatives considered**:
- External `style.css` + `main.js` — separates concerns but adds relative-path dependency; marginally more complex for a demo file.
- Web Components / Custom Elements — unnecessary abstraction.

---

## 5. Phone Mockup Approach

**Decision**: Pure CSS box model (no SVG, no image asset for the phone frame)  
**Rationale**: A rectangled-with-rounded-corners CSS box with notch and home bar decorations is sufficient to read as a smartphone. No external image dependency; fully scalable; easily adjusted.  
**Alternatives considered**:
- SVG phone outline — higher fidelity but requires maintaining an SVG asset.
- Background image of a device frame — not portable, copyright concern for third-party device imagery.

---

## 6. Backward Navigation (Missing from Current Build)

**Decision**: Add a `left-zone` click handler mirroring `advance()` but in reverse direction  
**Rationale**: The current `index.html` only implements forward navigation. A `retreat()` function uses `(currentIndex - 1 + IMAGES.length) % IMAGES.length` for wrap-around and positions the incoming image at `translateX(-100%)` before sliding both elements in the opposite direction.  

**No unresolved items remain.**
