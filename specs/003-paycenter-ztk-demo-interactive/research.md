# Research: PayCenter ZTK Interactive Demo

**Phase**: 0 — Research & Unknowns Resolution  
**Date**: 2026-05-15  
**Feature**: [spec.md](./spec.md)

---

## Technology Stack

**Decision**: Vanilla HTML5 + CSS3 + ES2020. Single `.html` file. Zero build step. Zero dependencies.

**Rationale**: User explicitly requested "modern, simple, web tools". The demo is a static single-page file that will be opened directly in a browser (file:// or simple HTTP). No framework adds value here — the entire feature is ~150 lines of JS, ~100 lines of CSS, and one HTML document. A framework would add complexity, a build step, and deployment friction with zero benefit.

**Alternatives considered**:
- React / Vue: Overkill for a static demo; introduces npm, bundler, and node_modules.
- Web Components: Valid, but add ceremony for a single-use file.
- Vanilla chosen: Least surface area, maximum portability, easiest to hand off.

---

## Slide Transition Approach

**Decision**: CSS `transform: translateX()` driven by inline style updates from JavaScript, using a double `requestAnimationFrame` pattern to guarantee the browser registers the initial placement before animating.

**Rationale**: CSS transforms are GPU-accelerated and do not trigger layout reflow. The double rAF pattern is the industry-standard technique for "set position without animation, then animate" in vanilla JS — it ensures the browser commits the non-animated frame before the next paint adds the transition. A `setTimeout` after the specified duration handles the post-animation cleanup (snap-back, image swap, state reset).

**Transition duration**: 300ms ease-in-out (per spec FR-010).

**Alternatives considered**:
- CSS animation keyframes: Less flexible for direction-dependent transitions driven by JS state.
- Web Animations API: More powerful but unnecessary; transform + rAF is simpler and equally compatible.
- canvas / WebGL: Unnecessary for image slides.

---

## Image Asset Strategy

**Decision**: `<img>` elements with `object-fit: contain` inside a fixed-size `.phone-screen` viewport. Two `<img>` elements are kept in the DOM at all times (current + incoming), with the incoming element staged off-screen via `translateX(±100%)` and animated in.

**Rationale**: Two-element swap pattern is the simplest approach for a simultaneous slide-out/slide-in effect. It avoids cloning elements or managing a dynamic list. The incoming image is preloaded into the DOM by keeping it as an `<img>` tag; only `src` is swapped before animation.

**Image files in use**:
| File | Role |
|------|------|
| `Zelle P2P Enrollment Carousel Image 001.png` | Carousel screen 1 (initial) |
| `Zelle P2P Enrollment Carousel Image 002.png` | Carousel screen 2 |
| `Zelle P2P Enrollment Carousel Image 003.png` | Carousel screen 3 |
| `Zelle P2P Enrollment Carousel Image 004.png` | Carousel screen 4 |
| `Terms and Conditions.png` | T&C screen (shown on GET STARTED tap) |
| `Choose How to Receive Money P2P.png` | Choose screen (shown on Accept & Continue tap) |
| `Zelle Access Contacts Permission.png` | **Out of scope — not used** |

---

## GET STARTED Hit Region

**Decision**: An absolutely-positioned transparent `<div>` overlay covers the bottom 20% of `.phone-screen`. It sits at `z-index: 11` (above left/right tap zones at `z-index: 10`), giving it click priority. It is only pointer-active when a carousel screen is displayed; it is disabled (`pointer-events: none`) when the T&C screen is shown.

**Rationale**: Percentage-based positioning scales with the phone frame. Bottom 20% reliably covers the GET STARTED button across all four carousel images regardless of exact pixel dimensions. A `<div>` overlay is simpler to position and maintain than trying to compute click coordinates relative to image content.

**Alternatives considered**:
- Pixel-hardcoded coordinates: Fragile if the phone frame is ever resized.
- Image map `<area>`: Per-image coordinates required; difficult to maintain.

---

## Accept & Continue Hit Region

**Decision**: An absolutely-positioned transparent `<div>` overlay (`#accept-hotspot`) covers the Accept & Continue button area of the T&C screen. It sits at `z-index: 12` (above `#cta-hotspot` at 11, above nav zones at 10). It is pointer-active only when `isTermsScreen === true`. `e.stopPropagation()` prevents any underlying click handlers from also firing.

**Coordinate strategy**: The spec requires coordinates determined by visual inspection of `Terms and Conditions.png` (828×1792px). As a baseline, bottom 20% (`top: 80%; height: 20%`) is implemented; the exact `top` and `height` percentages should be calibrated by opening the image and measuring the visible Accept & Continue button position relative to the image height.

**Image aspect ratio**: 828×1792 ≈ 0.46 portrait ratio. The phone screen viewport is ~266×576px (290−24px sides, 684−48−60px vertically). The image will be letterboxed via `object-fit: contain`, so the rendered button position within the viewport will differ from the raw pixel position in the source PNG. Percentage-based overlay positioning relative to the viewport (not the image) is the correct approach.

---

## Choose How to Receive Money P2P Screen

**Decision**: Displayed after Accept & Continue via the same 300ms slide-from-right transition as all other forward navigation. Once displayed, all tap zones are disabled (`pointer-events: none` on nav zones, `pointer-events: none` on all hotspots). Terminal state — no interaction possible.

**Rationale**: Consistent with the spec (FR-014). No additional state or handlers are needed because once `isChooseScreen = true` all guards block interaction.

---

## Gap Analysis: Current `index.html` vs Updated Spec

The implementation as of the previous session is 95% complete. One refinement item remains:

| Gap | Current Behavior | Required Behavior |
|-----|-----------------|-------------------|
| **Accept & Continue hotspot position** | Bottom 20% (`top: 80%`) — approximation | Visual inspection of `Terms and Conditions.png` (828×1792px) to refine `top` and `height` percentages | 

**All other requirements are satisfied**:

| Requirement | Status |
|---|---|
| FR-001 Phone frame | ✅ |
| FR-002 Load Screen 001 | ✅ |
| FR-003 50/50 tap zones | ✅ |
| FR-004/005 Slide animation | ✅ |
| FR-006 Circular wrap | ✅ |
| FR-007/008 GET STARTED bottom 20% | ✅ |
| FR-009 Correct image assets | ✅ |
| FR-010 300ms ease-in-out | ✅ |
| FR-011 T&C only responds to Accept region | ✅ |
| FR-012 No nav indicators | ✅ |
| FR-013 Accept → Choose screen 300ms slide | ✅ |
| FR-014 Choose screen terminal | ✅ |

---

## All NEEDS CLARIFICATION Items Resolved

| Item | Resolution |
|------|------------|
| GET STARTED region strategy | Bottom 20% of phone screen (percentage overlay) |
| Transition duration | 300ms ease-in-out |
| Tap zone split | 50/50 vertical |
| T&C behavior | Accept & Continue only; elsewhere is no-op |
| Accept & Continue region | Percentage-based overlay (bottom 20% baseline), calibrate via visual inspection |
| Choose screen behavior | Terminal state — no interactions |
| Navigation indicators | None |
