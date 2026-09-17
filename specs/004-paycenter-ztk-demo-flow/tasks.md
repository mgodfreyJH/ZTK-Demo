# Tasks: PayCenter ZTK Demo – Enroll P2P Flow

**Feature**: `004-paycenter-ztk-demo-flow`  
**Branch**: `004-paycenter-ztk-demo-flow`  
**Plan**: [plan.md](plan.md) | **Spec**: [spec.md](spec.md)  
**Generated**: 2026-05-18  
**Single File Target**: `ZTK Demo Project/index.html`

---

## Phase 1 – Setup

_Verify the working baseline before making any changes._

- [X] T001 Open `ZTK Demo Project/index.html` in a browser and confirm the current state: background renders, phone frame shows carousel image 001, right/left nav click works, GET STARTED shows T&C, Accept & Continue currently fails (broken image path)

---

## Phase 2 – Foundational (Bug Fix + Blank Initial State)

_These changes unblock all user stories. Must be complete before testing any story._

- [X] T002 Fix `CHOOSE_IMAGE` constant in `ZTK Demo Project/index.html` — change `'images/Choose How to Receive Money P2P.png'` to `'images/Choose How to Receive Money 01 P2P.png'`
- [X] T003 Set blank initial phone state in `ZTK Demo Project/index.html`: both `#slide-current` and `#slide-incoming` `src` attributes must start empty (`src=""`); disable all tap zones on load (`pointer-events: none` on `#click-zone`, `#left-zone`, `#cta-hotspot`, `#accept-hotspot`); ensure `isAnimating = false`, `flowState = 'blank'`
- [X] T004 Add `useCase` field to the JS state object in `ZTK Demo Project/index.html` and rename the two existing boolean flags (`isTermsScreen`, `isChooseScreen`) to align with the state machine (`flowState = 'blank' | 'carousel' | 'terms' | 'choose-receive'`)

---

## Phase 3 – User Story 1: Background, Phone Frame & Use-Case Panel (P1)

_Goal: Page loads with JH background, phone on right, radio-button panel on left, phone screen blank._

**Independent Test**: Open `index.html` in a browser — background fills viewport, phone frame is visible on the right, all four radio buttons are visible on the left, phone screen is black.

- [X] T005 [US1] Add `.demo-wrapper` CSS in `ZTK Demo Project/index.html`: `display: flex; flex-direction: row; align-items: center; gap: 32px` — this wraps the panel and phone side-by-side
- [X] T006 [US1] Add `.use-case-panel` CSS in `ZTK Demo Project/index.html`: semi-transparent dark card (`background: rgba(0,0,0,0.55); border-radius: 12px; padding: 20px 24px`), white text, readable at 1280 px viewport width
- [X] T007 [P] [US1] Wrap the existing `<div class="phone-frame">` in a `<div class="demo-wrapper">` in `ZTK Demo Project/index.html`
- [X] T008 [US1] Insert `<aside class="use-case-panel">` as the first child of `.demo-wrapper` in `ZTK Demo Project/index.html`, containing a `<fieldset>` with legend "Use Cases" and four `<label>` elements each wrapping a `<input type="radio" name="use-case">` for values `enroll-p2p`, `enroll-smb`, `send-p2p`, `send-smb` with display text "Enroll P2P", "Enroll SMB", "Send P2P Payment", "Send SMB Payment"
- [ ] T009 [US1] Verify in browser: background renders, phone is to the right of the radio panel, phone screen is black, no radio button is pre-selected

---

## Phase 4 – User Story 2: Enroll P2P Selection & Carousel Navigation (P1)

_Goal: Selecting "Enroll P2P" loads carousel image 001; right/left taps slide through images 001–004 with circular wrap._

**Independent Test**: Select "Enroll P2P" → image 001 appears. Click right 4× → 002, 003, 004, back to 001. Click left from 001 → 004.

- [X] T010 [US2] Add `activateEnrollP2P()` function in `ZTK Demo Project/index.html`: sets `flowState = 'carousel'`, `carouselIndex = 0`; sets `#slide-current` `src` to `IMAGES[0]`, makes it visible; enables `#click-zone` and `#left-zone` pointer events and cursor; calls `updateCtaHotspot()`
- [X] T011 [US2] Add `resetToBlank()` function in `ZTK Demo Project/index.html`: sets `flowState = 'blank'`; clears `#slide-current` and `#slide-incoming` `src` to `""`; disables pointer events on all zones (`#click-zone`, `#left-zone`, `#cta-hotspot`, `#accept-hotspot`); resets nav zone cursors to `default`
- [X] T012 [US2] Wire radio button `change` event listener in `ZTK Demo Project/index.html`: when value is `'enroll-p2p'` call `activateEnrollP2P()`; for all other values call `resetToBlank()`
- [ ] T013 [US2] Verify in browser: selecting "Enroll P2P" shows image 001; right-click advances through 001→002→003→004→001 (wrap); left-click from 001 wraps to 004; selecting "Enroll SMB" clears to black

---

## Phase 5 – User Story 3: GET STARTED CTA Triggers Terms & Conditions (P1)

_Goal: Tapping the bottom 20% of any carousel image shows the T&C screen. Tapping outside that zone does not._

**Independent Test**: With any carousel image displayed, tap the GET STARTED area → T&C screen appears. Tap the nav zones → images slide normally.

- [X] T014 [US3] Confirm `updateCtaHotspot()` in `ZTK Demo Project/index.html` enables `#cta-hotspot` pointer events only when `flowState === 'carousel'` and disables it in all other states — update function body to reference `flowState` instead of `!isTermsScreen && !isChooseScreen`
- [X] T015 [US3] Confirm CTA hotspot CSS `z-index: 11` exceeds nav zone `z-index: 10` in `ZTK Demo Project/index.html`, ensuring CTA takes priority on overlap — adjust if needed
- [ ] T016 [US3] Verify in browser: tap bottom area of carousel image 001 → T&C screen slides in; tap right/left nav areas above the CTA zone → carousel advances/retreats normally

---

## Phase 6 – User Story 4: Accept & Continue Completes the Flow (P1)

_Goal: Tapping Accept & Continue on the T&C screen shows "Choose How to Receive Money 01 P2P". Tapping elsewhere on T&C does nothing._

**Independent Test**: Navigate to T&C → tap Accept & Continue → "Choose How to Receive Money 01 P2P" appears. Tap anywhere else → no change.

- [X] T017 [US4] Update `showChooseScreen()` in `ZTK Demo Project/index.html` to set `flowState = 'choose-receive'` (replacing the old `isChooseScreen = true` flag) after the animation completes; ensure all zones remain disabled after transition
- [X] T018 [US4] Update `updateAcceptHotspot()` in `ZTK Demo Project/index.html` to enable `#accept-hotspot` pointer events only when `flowState === 'terms'` — reference `flowState` instead of `isTermsScreen`
- [ ] T019 [US4] Verify in browser: T&C → Accept & Continue → "Choose How to Receive Money 01 P2P" image appears; tapping anywhere else on T&C screen produces no change; phone screen is inert on the final screen

---

## Phase 7 – User Story 5: Non-P2P Use Cases Reset to Blank (P2)

_Goal: Selecting Enroll SMB, Send P2P Payment, or Send SMB Payment clears the phone screen to black._

**Independent Test**: Activate Enroll P2P, navigate a few screens, then select Enroll SMB → phone screen immediately clears to black.

- [X] T020 [US5] Verify `resetToBlank()` (added in T011) correctly handles mid-flow resets in `ZTK Demo Project/index.html`: if `flowState` is `'terms'` or `'choose-receive'` when a non-P2P radio fires, all zones are still disabled and screen clears cleanly with no lingering animation state
- [ ] T021 [US5] Verify in browser: start Enroll P2P flow, advance to T&C, then select "Send P2P Payment" → phone screen clears to black; no residual overlay or animation artifacts visible

---

## Phase 8 – Polish & Cross-Cutting

_Verify all success criteria, viewport, and animation edge cases._

- [ ] T022 Verify at 1280 px browser window width: phone frame and use-case panel are both fully visible side-by-side with no horizontal overflow in `ZTK Demo Project/index.html`
- [ ] T023 [P] Verify image preloading: all 7 images (`IMAGES[0–3]`, `TERMS_IMAGE`, `CHOOSE_IMAGE`, background) load before first interaction — open DevTools Network tab and confirm no images load lazily on first tap
- [ ] T024 [P] Verify animation drop behavior: click right rapidly 5× during a transition — only one transition fires per 300 ms window; no queued animations play out after clicks stop
- [X] T025 Confirm `onerror` fallback on both slide `<img>` elements in `ZTK Demo Project/index.html` sets `this.src = ''` and `this.style.background = '#111'` to prevent broken-image icons if a file is missing

---

## Dependency Graph

```
T001 (baseline audit)
  └── T002 (fix CHOOSE_IMAGE path)
  └── T003 (blank initial state)
  └── T004 (state machine refactor)
        ├── T005–T009 (US1: layout + panel)
        │     └── T010–T013 (US2: Enroll P2P + carousel)
        │           ├── T014–T016 (US3: CTA → T&C)
        │           │     └── T017–T019 (US4: Accept → Choose)
        │           │           └── T020–T021 (US5: non-P2P reset)
        │           │                 └── T022–T025 (Polish)
        │           └── T020–T021 (US5: can be developed in parallel with US3)
```

---

## Parallel Execution Opportunities

| Story | Parallelizable Tasks | Why |
|---|---|---|
| US1 | T007, T008 can be coded simultaneously | Different HTML sections |
| Polish | T023, T024 are independent browser verifications | Different behavior axes |

---

## Implementation Strategy

**MVP Scope** (minimum to demonstrate the full Enroll P2P flow):
T001 → T002 → T003 → T004 → T005–T009 → T010–T013 → T014–T016 → T017–T019

**Full scope** adds T020–T025 for non-P2P reset, viewport check, and polish.

All tasks target a single file: `ZTK Demo Project/index.html`. Each task is independently verifiable in a browser with no build step.

---

## Format Validation

- All 25 tasks follow the checklist format: `- [ ] [ID] [P?] [Story?] description with file path`
- Task IDs are sequential: T001–T025
- [P] markers applied to parallelizable tasks (T005 omitted per single-file constraint — parallel only where HTML/JS sections don't overlap)
- [US#] labels applied to all user story phase tasks
- Setup and Foundational phase tasks have no story label
