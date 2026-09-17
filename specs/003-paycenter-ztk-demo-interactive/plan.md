# Implementation Plan: PayCenter ZTK Interactive Demo (v2)

**Branch**: `003-paycenter-ztk-demo-interactive` | **Date**: 2026-05-15 | **Spec**: [spec.md](./spec.md)  
**Input**: Feature specification from `specs/003-paycenter-ztk-demo-interactive/spec.md`

## Summary

A single-page browser demo of the PayCenter ZTK product. A CSS mobile phone frame displays a Zelle P2P Enrollment carousel (4 screens, 300ms slide transitions, circular wrap). Tapping GET STARTED (bottom 20%) slides in the Terms and Conditions screen. Tapping Accept & Continue on T&C slides in the Choose How to Receive Money P2P screen (terminal state). All other taps on T&C and the Choose screen are no-ops.

The deliverable is `ZTK Demo Project/index.html` — a single file, no build step, no dependencies. The implementation is 95% complete from the previous session. One calibration task remains: visually inspect `Terms and Conditions.png` (828×1792px) and refine the `#accept-hotspot` CSS percentages to match the actual Accept & Continue button position.

**Technology**: Vanilla HTML5 + CSS3 + ES2020 — single file, zero dependencies, zero build step.

## Technical Context

**Language/Version**: HTML5 / CSS3 / ES2020 (native browser, no transpilation)  
**Primary Dependencies**: None  
**Storage**: N/A  
**Testing**: Manual browser verification  
**Target Platform**: Desktop browser (Chrome, Edge, Firefox — latest stable)  
**Project Type**: Single-page static demo  
**Performance Goals**: 300ms ease-in-out slide animation, no layout reflow during transitions  
**Constraints**: Single HTML file, offline-capable (file:// compatible), zero install friction  
**Scale/Scope**: 1 HTML file + 6 PNG image assets in use

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

Constitution is unpopulated (blank template) — no architectural gates are defined. No violations possible.

**Post-design re-check**: No gate violations. Single-file static page, no external services, no authentication, no data persistence, no dependencies.

## Project Structure

### Documentation (this feature)

```text
specs/003-paycenter-ztk-demo-interactive/
├── plan.md              ← This file
├── research.md          ← Phase 0 output (updated)
├── data-model.md        ← Phase 1 output (updated)
├── quickstart.md        ← Phase 1 output (updated)
└── tasks.md             ← Phase 2 output (via /speckit.tasks)
```

### Source Code

```text
ZTK Demo Project/
├── index.html           ← Single deliverable: all HTML, CSS, JavaScript
└── images/
    ├── Zelle P2P Enrollment Carousel Image 001.png
    ├── Zelle P2P Enrollment Carousel Image 002.png
    ├── Zelle P2P Enrollment Carousel Image 003.png
    ├── Zelle P2P Enrollment Carousel Image 004.png
    ├── Terms and Conditions.png       (828×1792px)
    └── Choose How to Receive Money P2P.png
```

**Structure Decision**: Single-file web page. All logic is inline in `index.html` per the zero-dependency, zero-build-step requirement.

---

## Gap Analysis vs Updated Spec

All previously-implemented gaps (wrong image, 350ms animation, no T&C handler) were resolved in the prior session. One calibration item remains:

| # | Gap | Location in index.html | Fix |
|---|-----|------------------------|-----|
| G1 | `#accept-hotspot` uses bottom 20% (approximate) | `#accept-hotspot` CSS: `top: 80%; height: 20%` | Open `Terms and Conditions.png`, measure the Accept & Continue button vertical position as % of image height, adjust `top` and `height` accordingly |

---

## Implementation Design

### State Variables

```js
let currentIndex   = 0;       // 0–3, index into IMAGES[]
let isAnimating    = false;    // blocks new taps during transitions
let isTermsScreen  = false;    // true when T&C is showing
let isChooseScreen = false;    // true when Choose screen is showing (terminal)
```

### Constants

```js
const IMAGES = [
  'images/Zelle P2P Enrollment Carousel Image 001.png',
  'images/Zelle P2P Enrollment Carousel Image 002.png',
  'images/Zelle P2P Enrollment Carousel Image 003.png',
  'images/Zelle P2P Enrollment Carousel Image 004.png',
];
const TERMS_IMAGE  = 'images/Terms and Conditions.png';
const CHOOSE_IMAGE = 'images/Choose How to Receive Money P2P.png';
```

### Functions

| Function | Trigger | Behavior |
|----------|---------|----------|
| `advance()` | Right zone click | Slides current → left, next → from right. Wraps 3→0. Blocked by `isTermsScreen` or `isChooseScreen`. |
| `retreat()` | Left zone click | Slides current → right, prev → from left. Wraps 0→3. Blocked by `isTermsScreen` or `isChooseScreen`. |
| `showTerms()` | CTA hotspot click | Slides in `Terms and Conditions.png`; sets `isTermsScreen = true`; disables nav zones; enables `#accept-hotspot`. |
| `showChooseScreen()` | Accept hotspot click | Slides in `Choose How to Receive Money P2P.png`; sets `isChooseScreen = true`; disables nav zones and all hotspots. |
| `updateCtaHotspot()` | After any state change | Enables/disables CTA pointer-events: active only when `!isTermsScreen && !isChooseScreen`. |
| `updateAcceptHotspot()` | After any state change | Enables/disables Accept pointer-events: active only when `isTermsScreen === true`. |

### Tap Zone Priority (z-index order)

| z-index | Element | Active when |
|---------|---------|-------------|
| 12 | `#accept-hotspot` | `isTermsScreen === true` |
| 11 | `#cta-hotspot` | `!isTermsScreen && !isChooseScreen` |
| 10 | `#click-zone`, `#left-zone` | Carousel active (not disabled via pointer-events) |

### CSS Hotspot Layout

```
#cta-hotspot     top: 80%  height: 20%  z-index: 11  (GET STARTED, all carousel screens)
#accept-hotspot  top: ?    height: ?    z-index: 12  (Accept & Continue, T&C screen only)
                          └─ calibrate by visual inspection of Terms and Conditions.png
```

### Animation Pattern (unchanged from prior session)

```
1. Set incoming.src, incoming.transition = 'none', incoming.transform = translateX(100%)
2. rAF → rAF → set transition = '300ms ease-in-out' on both elements
3. Set current.transform = translateX(-100%), incoming.transform = translateX(0)
4. setTimeout(300ms):
   - Snap current: transition = 'none', src = new image, transform = 0
   - Reset incoming: transition = 'none', transform = 100%
   - Update state; isAnimating = false
```

---

## Constraints & Decisions

| Decision | Rationale |
|----------|-----------|
| Accept & Continue hotspot: percentage-based, not absolute pixels | The phone screen renders the T&C image with `object-fit: contain` — the image is letterboxed inside the viewport. Absolute pixel coordinates from the source PNG would need to be remapped to viewport coordinates accounting for letterboxing; percentage-based overlay positioning relative to the viewport is simpler and sufficient. |
| Terminal state requires page reload | No "restart" button needed per spec. Simplifies state management; demo presenters can reload in < 1 second. |
| `e.stopPropagation()` on accept-hotspot click | Prevents the click from bubbling to any underlying handlers on the phone-screen element. |
| No `dismissTerms()` function | Removed per spec update — T&C screen is not dismissible except via Accept & Continue. |
