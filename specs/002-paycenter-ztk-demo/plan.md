# Implementation Plan: PayCenter ZTK Demo — Zelle P2P Enrollment Carousel

**Branch**: `002-paycenter-ztk-demo` | **Date**: 2026-05-11 | **Spec**: [spec.md](spec.md)  
**Input**: Feature specification from `specs/002-paycenter-ztk-demo/spec.md`

---

## Summary

Single-page presentational demo rendering a CSS phone mockup. The phone screen hosts a two-image CSS `transform`/`transition` carousel for four Zelle P2P Enrollment screens. Clicking the right or left half of the screen navigates forward or backward with 350ms directional slide animations and wrap-around. An invisible GET STARTED hotspot on all four carousel screens transitions to the Zelle Access Contacts Permission screen. Both nav zones and the CTA hotspot are disabled while the permission screen is showing. Permission screen is currently a terminal state — further requirements forthcoming.

**Current state**: Fully implemented. No open gaps.

---

## Technical Context

**Language/Version**: HTML5, CSS3, ES6+ (ES2020 target — no transpilation)  
**Primary Dependencies**: None (zero external dependencies)  
**Storage**: N/A  
**Testing**: Manual browser verification  
**Target Platform**: Desktop/laptop browsers, 1280×720 and above  
**Project Type**: Single-page presentational demo (static HTML)  
**Performance Goals**: 60fps slide animation; <3s initial load on broadband  
**Constraints**: No build step, no server, `index.html` + `images/` only, offline-capable  
**Scale/Scope**: 1 HTML file, 5 PNG images

---

## Constitution Check

**Status**: Constitution not yet ratified. No binding gates apply.

- Simplicity: ✅ Zero-dependency, single-file approach. No violations.

---

## Project Structure

### Documentation

```text
specs/002-paycenter-ztk-demo/
├── plan.md          ← this file
├── research.md      ← Phase 0 (complete)
├── data-model.md    ← Phase 1 (complete)
├── quickstart.md    ← Phase 1 (complete)
└── tasks.md         ← Phase 2 (complete — all tasks done)
```

### Source Code

```text
ZTK Demo Project/
├── index.html       ← single application file (fully implemented)
└── images/
    ├── Zelle P2P Enrollment Carousel Image 001.png  (750 × 1624)
    ├── Zelle P2P Enrollment Carousel Image 002.png  (750 × 1624)
    ├── Zelle P2P Enrollment Carousel Image 003.png  (750 × 1624)
    ├── Zelle P2P Enrollment Carousel Image 004.png  (750 × 1624)
    └── Zelle Access Contacts Permission.png         (147 × 315)
```

---

## Design Decisions

### Phone mockup
CSS box model, 290×684px frame. Screen viewport: `inset: 48px 12px 60px 12px` = 266×576px. Matches 750×1624 image ratio to <0.01%. See [research.md](research.md).

### Animation mechanism
CSS `transform: translateX` + `transition: transform 350ms ease-in-out` on two layered `<img>` elements (`#slide-current`, `#slide-incoming`). Direction determined by sign of offset (`±100%`). Double `requestAnimationFrame` before firing transition ensures browser registers the off-screen start position. See [research.md](research.md).

### Carousel state
Three runtime variables: `currentIndex` (0–3), `isAnimating` (boolean), `isPermissionScreen` (boolean). See [data-model.md](data-model.md).

### Navigation zones
Two invisible full-height `position: absolute` divs (`.left-zone`, `.click-zone`). `cursor: pointer` while active. `pointer-events: none; cursor: default` set after permission screen activates.

### Debounce
`isAnimating` flag; `advance()` and `retreat()` guard `if (isAnimating || isPermissionScreen) return`.

### GET STARTED hotspot
Invisible `#cta-hotspot` div: `top: 78%, left: 5%, width: 90%, height: 10%`, `z-index: 11`. Active (`pointer-events: all`) on all four carousel screens (`!isPermissionScreen`). Disabled when permission screen shows.

### Permission image rendering
147×315px image via `object-fit: contain` in 266×576px screen. Nearly identical aspect ratio — fills screen vertically with negligible letterboxing. `.slide` CSS rule includes `border: none; outline: none; box-shadow: none`.

### Permission screen — terminal state
`isPermissionScreen = true` after transition completes. All three interaction zones (`clickZone`, `leftZone`, `ctaHotspot`) have `pointer-events: none`. No further navigation until new requirements are specified.

---

## What Needs to Be Built

| # | Item | Status |
|---|------|--------|
| 1 | Phone mockup (CSS box model, 290×684px) | ✅ Done |
| 2 | Image registry (`IMAGES` array, 4 PNGs) | ✅ Done |
| 3 | Initial display of image 001 | ✅ Done |
| 4 | Forward navigation — `advance()` + right-zone | ✅ Done |
| 5 | Backward navigation — `retreat()` + left-zone | ✅ Done |
| 6 | Debounce via `isAnimating` flag | ✅ Done |
| 7 | GET STARTED hotspot (all 4 screens) + `showPermission()` | ✅ Done |
| 8 | `isPermissionScreen` flag — disable all zones on permission screen | ✅ Done |
| 9 | CSS `.slide { border: none; outline: none; box-shadow: none }` | ✅ Done |
| 10 | Permission screen — next interaction | ⏳ Deferred (requirements forthcoming) |
