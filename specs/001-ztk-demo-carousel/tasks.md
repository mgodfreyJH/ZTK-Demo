# Tasks: PayCenter ZTK Demo Carousel

**Input**: Design documents from `specs/001-ztk-demo-carousel/`
**Prerequisites**: plan.md ✓, spec.md ✓, research.md ✓, data-model.md ✓, quickstart.md ✓

**Tech stack**: HTML5, CSS3, ES6+ JavaScript (vanilla) — no framework, no build step  
**Deliverable**: `ZTK Demo Project/index.html` + `ZTK Demo Project/images/`

---

## Phase 1: Setup

**Purpose**: Create project file structure and get source assets in place

- [X] T001 Create `ZTK Demo Project/index.html` as an HTML5 boilerplate file (doctype, `<html lang="en">`, `<head>` with `<meta charset>`, `<meta name="viewport">`, `<title>PayCenter ZTK Demo</title>`, empty `<style>`, empty `<body>`, empty `<script>`)
- [X] T002 [P] Create `ZTK Demo Project/images/` folder and copy all 4 PNG files from `c:\users\migodfrey\ZTK Images\` into it, preserving exact filenames

**Checkpoint**: `index.html` opens in a browser (blank page); `images/` folder contains all 4 PNG files

---

## Phase 2: Foundational

**Purpose**: Page shell and phone frame — prerequisites for all three user stories

**⚠️ CRITICAL**: US1, US2, and US3 all depend on the phone frame being present

- [X] T003 Add `<style>` block to `ZTK Demo Project/index.html`: set `body` to `margin: 0; min-height: 100vh; display: flex; justify-content: center; align-items: center; background: #1a1a2e; font-family: sans-serif`
- [X] T004 Add CSS phone frame to `ZTK Demo Project/index.html` `<style>` block: `.phone-frame` (320×640px, `background: #1c1c1e`, `border-radius: 40px`, `box-shadow: 0 0 0 2px #3a3a3c, 0 20px 60px rgba(0,0,0,0.6)`, `position: relative`); `.phone-notch` (120×28px centered top bar, `background: #1c1c1e`, `border-radius: 0 0 20px 20px`); `.home-bar` (120×4px, `background: #3a3a3c`, `border-radius: 2px`, centered at bottom); and corresponding HTML structure inside `<body>`

**Checkpoint**: Open `index.html` — dark charcoal page with a dark phone silhouette visible centered on screen

---

## Phase 3: User Story 1 — View Demo on Load (Priority: P1) 🎯 MVP

**Goal**: Phone screen displays Image 001 on page load — the complete initial state of the demo

**Independent Test**: Open `ZTK Demo Project/index.html` in a browser. Confirm: (1) phone frame is visible and centered on dark background, (2) the phone screen area shows `Zelle P2P Enrollment Carousel Image 001.png`, (3) no buttons or controls are visible

- [X] T005 [US1] Add `.phone-screen` CSS to `ZTK Demo Project/index.html`
- [X] T006 [US1] Add two `<img>` elements inside `.phone-screen` in `ZTK Demo Project/index.html`: `<img class="slide" id="slide-current" src="images/Zelle P2P Enrollment Carousel Image 001.png" alt="Enrollment screen 1">` at `transform: translateX(0)` and `<img class="slide" id="slide-incoming" src="images/Zelle P2P Enrollment Carousel Image 002.png" alt="">` at `transform: translateX(100%)` (pre-staged, hidden off-screen)

**Checkpoint — US1 complete**: Open `index.html`, confirm phone frame presents Image 001 on screen. US1 acceptance scenarios pass independently.

---

## Phase 4: User Story 2 — Navigate to Next Enrollment Screen (Priority: P2)

**Goal**: Clicking the right half of the phone screen slides to the next image with a 350ms ease-in-out animation; rapid clicks during animation are ignored

**Independent Test**: With Image 001 displayed, click the right side of the phone screen — Image 001 slides left, Image 002 slides in from the right in ~350ms. Repeat through to Image 003 then Image 004.

- [X] T007 [US2] Add `.click-zone` CSS and `<div>` to `ZTK Demo Project/index.html`
- [X] T008 [P] [US2] Add CSS transition `transform 350ms ease-in-out` to `.slide` in `ZTK Demo Project/index.html`
- [X] T009 [P] [US2] Add `<script>` block with `IMAGES`, `currentIndex`, `isAnimating`, and element refs
- [X] T010 [US2] Implement `advance()` with double-rAF and `setTimeout(350)` lock
- [X] T011 [US2] Wire `clickZone.addEventListener('click', advance)`

**Checkpoint — US2 complete**: Forward navigation works smoothly through all 4 images with 350ms slide. US2 acceptance scenarios pass independently.

---

## Phase 5: User Story 3 — Circular Wrap-Around Navigation (Priority: P3)

**Goal**: After Image 004, the next click wraps back to Image 001 with the same slide-in motion — the demo loops indefinitely

**Independent Test**: Navigate to Image 004 (3 clicks from 001). Click the right side — Image 001 slides in from the right. Continue clicking through a full second loop (001 → 002 → 003 → 004 → 001) to confirm continuous looping.

- [X] T012 [US3] Verified: `nextIndex = (currentIndex + 1) % IMAGES.length` wraps 3→0; no clamping logic present
- [X] T013 [US3] Wrap-around confirmed in code: modulo arithmetic handles all 4→1 transitions

**Checkpoint — US3 complete**: Demo loops indefinitely. US3 acceptance scenarios pass. SC-004 satisfied.

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Robustness, usability, and final validation

- [X] T014 [P] `onerror` handlers added to both `<img class="slide">` elements
- [X] T015 [P] `.left-zone` div added covering left 50% with `cursor: default`
- [X] T016 Final validation — open `ZTK Demo Project/index.html` by double-clicking; verify SC-001–SC-005

---

## Dependencies & Execution Order

### Phase Dependencies

- **Phase 1 (Setup)**: No dependencies — start immediately; T001 and T002 are parallel
- **Phase 2 (Foundational)**: Depends on T001 — BLOCKS all user stories
- **Phase 3 (US1)**: Depends on Phase 2 — can start as soon as phone frame exists
- **Phase 4 (US2)**: Depends on Phase 3 (needs screen + images in place) — T007/T008/T009 are parallel once US1 is complete
- **Phase 5 (US3)**: Depends on Phase 4 (modulo wrap is already in `advance()`) — primarily verification
- **Phase 6 (Polish)**: Depends on all user stories complete — T014 and T015 are parallel

### User Story Dependencies

- **US1 (P1)**: Depends on Foundational only — independently testable MVP
- **US2 (P2)**: Depends on US1 (screen + images must render before navigation can be tested)
- **US3 (P3)**: Depends on US2 (wrap-around is part of the same `advance()` function)

### Within US2 (Parallel Opportunities)

```
T007  (click-zone div + CSS)    ─┐
T008  (transition CSS on .slide) ─┤─ all parallel once US1 complete
T009  (JS state + element refs)  ─┘
         ↓
T010  (advance() function)       ← depends on T007, T008, T009
         ↓
T011  (wire event + verify)      ← depends on T010
```

---

## Implementation Strategy

**MVP** = Phase 1 + Phase 2 + Phase 3 (T001–T006): Phone frame with Image 001 visible. Deliverable and demoable before any JS is written.

**Full feature** = All phases, sequential. Estimated total: ~200–300 lines in a single `index.html`.

**Suggested execution order for a single developer**:
T001 → T002 → T003 → T004 → T005 → T006 → *[US1 checkpoint]* → T007 → T008 → T009 → T010 → T011 → *[US2 checkpoint]* → T012 → T013 → *[US3 checkpoint]* → T014 → T015 → T016
