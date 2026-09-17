# Tasks: Enroll P2P Extended Flow – Choose, Select Account & Spinner

**Feature**: `005-enroll-p2p-extended-flow`  
**Branch**: `005-enroll-p2p-extended-flow`  
**Plan**: [plan.md](plan.md) | **Spec**: [spec.md](spec.md)  
**Generated**: 2026-05-19  
**Single File Target**: `ZTK Demo Project/index.html`

---

## Phase 1 – Setup

_Confirm base state from feature 004 before making any changes._

- [X] T001 Open `ZTK Demo Project/index.html` in a browser, run the Enroll P2P flow to the "Choose How to Receive Money 01 P2P" screen, and confirm it is currently a static terminal state (no zones active, no further navigation)

---

## Phase 2 – Foundational (State Machine Rename + Constants + Spinner CSS)

_These changes are prerequisites for all user stories. No new behavior is visible until Phase 3._

- [X] T002 In `ZTK Demo Project/index.html`, rename `flowState = 'choose-receive'` to `flowState = 'choose-receive-01'` inside `showChooseScreen()` — one line change; verify no other references to `'choose-receive'` remain
- [X] T003 In `ZTK Demo Project/index.html`, rename constant `CHOOSE_IMAGE` to `CHOOSE_01_IMAGE` and update all references in the script (preload array, `showChooseScreen()` function)
- [X] T004 [P] In `ZTK Demo Project/index.html`, add four new image constants after `CHOOSE_01_IMAGE`: `CHOOSE_02_IMAGE`, `SELECT_01_IMAGE`, `SELECT_02_IMAGE`, `CONGRATS_IMAGE` with correct `images/` paths
- [X] T005 [P] In `ZTK Demo Project/index.html`, add the four new image constants to the preload `forEach` array so all images load before first interaction
- [X] T006 In `ZTK Demo Project/index.html`, add `let spinnerTimer = null;` after `let isAnimating = false;` in the state declarations block
- [X] T007 In `ZTK Demo Project/index.html` `<style>`, add the `@keyframes spin` rule and `#spinner` CSS rule (48px circle, white border-top, z-index 20, pointer-events none, centered via translate)
- [X] T008 In `ZTK Demo Project/index.html` `<style>`, add the shared hotspot base rule for `#choose-radio-hotspot, #choose-continue-hotspot, #select-radio-hotspot, #select-continue-hotspot` (position absolute, left 0, width 100%, pointer-events none, z-index 11, touch-action manipulation) plus individual position rules: radio hotspots `top: 40%/45%, height: 30%`; continue hotspots `top: 88%, height: 8%`

---

## Phase 3 – User Story 1: Radio Button on Choose 01 Swaps to Choose 02 (P1)

_Goal: Tapping either radio button zone on Choose How to Receive Money 01 instantly shows Choose 02 with no animation._

**Independent Test**: Navigate to Choose How to Receive Money 01 P2P → tap radio area → Choose 02 appears immediately with no slide.

- [X] T009 [US1] In `ZTK Demo Project/index.html`, add four new `<div>` hotspot elements inside `.phone-screen` immediately after `#accept-hotspot`: `#choose-radio-hotspot`, `#choose-continue-hotspot`, `#select-radio-hotspot`, `#select-continue-hotspot` — all with no inline styles (CSS handles defaults)
- [X] T010 [US1] In `ZTK Demo Project/index.html`, add `swapToChooseReceive02()` function: sets `flowState = 'choose-receive-02'`; sets `current.style.transition = 'none'` and `current.src = CHOOSE_02_IMAGE`; disables `#choose-radio-hotspot` pointer events; enables `#choose-continue-hotspot` pointer events
- [X] T011 [US1] In `ZTK Demo Project/index.html`, update `showChooseScreen()` to enable `#choose-radio-hotspot` pointer events after the slide-in animation completes (in the `setTimeout` callback at 300ms)
- [X] T012 [US1] In `ZTK Demo Project/index.html`, add `pointerdown` event listener on `#choose-radio-hotspot` calling `swapToChooseReceive02()`
- [X] T013 [US1] Verify in browser: complete flow to Choose 01 → tap radio zone → Choose 02 appears instantly; tap outside radio zone → no change

---

## Phase 4 – User Story 2: Continue CTA on Choose 02 Slides to Select Account 01 (P1)

_Goal: Tapping Continue on Choose 02 slides Select Account 01 in from the right using 300ms animation._

**Independent Test**: With Choose 02 displayed, tap Continue zone → Select Account 01 slides in smoothly from right.

- [X] T014 [US2] In `ZTK Demo Project/index.html`, add `slideToSelectAccount01()` function: guards on `isAnimating`; sets `isAnimating = true`; disables `#choose-continue-hotspot`; stages `incoming.src = SELECT_01_IMAGE` off-screen right; runs double-rAF + 300ms slide-in pattern; on completion sets `flowState = 'select-account-01'`, enables `#select-radio-hotspot`, clears `isAnimating`
- [X] T015 [US2] In `ZTK Demo Project/index.html`, add `pointerdown` event listener on `#choose-continue-hotspot` calling `slideToSelectAccount01()`
- [X] T016 [US2] Verify in browser: from Choose 02, tap Continue → Select Account 01 slides in with 300ms animation; tap elsewhere on Choose 02 → no transition

---

## Phase 5 – User Story 3: Radio Button on Select Account 01 Swaps to Select Account 02 (P1)

_Goal: Tapping either radio button zone on Select Account 01 instantly shows Select Account 02 with no animation._

**Independent Test**: With Select Account 01 displayed, tap radio area → Select Account 02 appears immediately with no slide.

- [X] T017 [US3] In `ZTK Demo Project/index.html`, add `swapToSelectAccount02()` function: sets `flowState = 'select-account-02'`; sets `current.style.transition = 'none'` and `current.src = SELECT_02_IMAGE`; disables `#select-radio-hotspot` pointer events; enables `#select-continue-hotspot` pointer events
- [X] T018 [US3] In `ZTK Demo Project/index.html`, add `pointerdown` event listener on `#select-radio-hotspot` calling `swapToSelectAccount02()`
- [X] T019 [US3] Verify in browser: from Select Account 01, tap radio zone → Select Account 02 appears instantly; tap outside radio zone → no change

---

## Phase 6 – User Story 4: Continue on Select Account 02 → Fade + Spinner → Congratulations (P1)

_Goal: Tapping Continue on Select Account 02 fades the screen, shows spinner for 1 second, then shows Congratulations P2P._

**Independent Test**: With Select Account 02 displayed, tap Continue → screen fades, spinner appears, after 1 second Congratulations P2P appears. Tapping during spinner → no effect.

- [X] T020 [US4] In `ZTK Demo Project/index.html`, add `showProcessing()` function: disables `#select-continue-hotspot`; sets `isAnimating = true`; applies `current.style.transition = 'opacity 300ms ease-in'` and `current.style.opacity = '0'`; creates `#spinner` div and appends to `.phone-screen`; stores `spinnerTimer = setTimeout(...)` for 1000ms; in the timer callback: removes `#spinner`, sets `current.style.transition = 'none'`, `current.style.opacity = '1'`, `current.src = CONGRATS_IMAGE`, `flowState = 'congratulations'`, `isAnimating = false`
- [X] T021 [US4] In `ZTK Demo Project/index.html`, add `pointerdown` event listener on `#select-continue-hotspot` calling `showProcessing()`
- [X] T022 [US4] Verify in browser: from Select Account 02, tap Continue → screen fades to black with spinner centered; after 1 second spinner disappears and Congratulations P2P appears; tapping phone screen during spinner → no effect

---

## Phase 7 – Polish: resetToBlank() Extension & Cross-Cutting

_Ensure mid-flow resets work from all new states, including during processing and from Congratulations._

- [X] T023 In `ZTK Demo Project/index.html`, extend `resetToBlank()` to: (1) call `clearTimeout(spinnerTimer); spinnerTimer = null`, (2) call `document.getElementById('spinner')?.remove()`, (3) set `current.style.opacity = '1'; current.style.transition = 'none'`, (4) disable pointer events on all 4 new hotspots (`#choose-radio-hotspot`, `#choose-continue-hotspot`, `#select-radio-hotspot`, `#select-continue-hotspot`)
- [X] T024 [P] Verify in browser: start Enroll P2P, advance to Choose 01, then select "Enroll SMB" → phone immediately resets to black with no visual artifacts
- [X] T025 [P] Verify in browser: start Enroll P2P, advance all the way to the spinner/processing state, then select "Enroll SMB" → spinner disappears instantly, phone resets to black
- [X] T026 [P] Verify in browser: reach Congratulations P2P, then select "Enroll P2P" again → phone resets to blank then loads carousel image 001 correctly
- [X] T027 Verify hotspot positions are accurate in all 4 screens using DevTools overlay: add temporary `outline: 2px solid red` to each hotspot CSS rule, confirm coverage of radio button areas and Continue CTAs visually, then remove debug outlines

---

## Dependency Graph

```
T001 (baseline audit)
  └── T002 (rename choose-receive state)
  └── T003 (rename CHOOSE_IMAGE constant)
  └── T004 (add new image constants)
  └── T005 (extend preload array)
  └── T006 (add spinnerTimer)
  └── T007 (spinner CSS)
  └── T008 (hotspot CSS)
        └── T009 (add hotspot HTML elements)
              ├── T010–T013 (US1: Choose 01 radio → Choose 02)
              │     └── T014–T016 (US2: Choose 02 Continue → Select 01)
              │           └── T017–T019 (US3: Select 01 radio → Select 02)
              │                 └── T020–T022 (US4: Select 02 Continue → fade+spinner → Congratulations)
              │                       └── T023–T027 (Polish: resetToBlank extension + verification)
```

---

## Parallel Execution Opportunities

| Tasks | Why Parallelizable |
|---|---|
| T004, T005 | Both add to JS constants section, non-overlapping |
| T024, T025, T026 | Independent browser verification paths |

---

## Implementation Strategy

**MVP Scope**: T001–T022 covers all four P1 user stories end-to-end.  
**Full scope**: T023–T027 adds reset safety net and hotspot calibration.

All tasks target a single file: `ZTK Demo Project/index.html`. Each task is independently verifiable in a browser with no build step.

---

## Format Validation

- All 27 tasks follow the checklist format: `- [X] [ID] [P?] [Story?] description with file path`
- Task IDs: T001–T027
- [P] markers on parallelizable items (T004+T005, T024+T025+T026)
- [US#] labels on all user story phase tasks (US1–US4)
- Setup and Foundational phase tasks have no story label
