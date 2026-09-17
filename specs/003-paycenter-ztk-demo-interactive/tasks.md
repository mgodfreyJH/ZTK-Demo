---
description: "Task list for PayCenter ZTK Interactive Demo (v2)"
---

# Tasks: PayCenter ZTK Interactive Demo (v2)

**Input**: Design documents from `specs/003-paycenter-ztk-demo-interactive/`  
**Prerequisites**: plan.md ✅, spec.md ✅, research.md ✅, data-model.md ✅, quickstart.md ✅  
**Tests**: Not requested — manual browser verification only  
**Deliverable**: `ZTK Demo Project/index.html` (single file, updated in place)  
**Implementation status**: 95% complete. All carousel, T&C, and Choose screen logic is in place. One calibration task remains (T010).

## Format: `[ID] [P?] [Story?] Description`

- **[P]**: Can run in parallel (independent, no blocking dependency)
- **[Story]**: Maps to user story from spec.md (US1–US4)
- Exact file path included in every task description

---

## Phase 1: Setup

**Purpose**: Confirm all required assets are present before making any changes.

- [X] T001 Confirm all 6 required PNG assets exist in `ZTK Demo Project/images/`: carousel 001–004, `Terms and Conditions.png`, `Choose How to Receive Money P2P.png`

**Checkpoint**: Assets confirmed ✅ — safe to proceed.

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Verify the current implementation baseline is coherent — correct identifiers, correct animation duration, correct image constants. All previously-implemented fixes are confirmed shipped.

⚠️ **CRITICAL**: Confirm all items before beginning story work.

- [X] T002 [P] Confirm `const TERMS_IMAGE = 'images/Terms and Conditions.png'` is present in `<script>` block of `ZTK Demo Project/index.html`
- [X] T003 [P] Confirm `const CHOOSE_IMAGE = 'images/Choose How to Receive Money P2P.png'` is present in `<script>` block of `ZTK Demo Project/index.html`
- [X] T004 [P] Confirm `let isTermsScreen = false` and `let isChooseScreen = false` are present (no `isPermissionScreen`) in `ZTK Demo Project/index.html`
- [X] T005 [P] Confirm all animation durations are `300ms` (CSS `.slide` transition + all `setTimeout` calls) in `ZTK Demo Project/index.html`
- [X] T006 [P] Confirm `#accept-hotspot` div is present in HTML and `#accept-hotspot` CSS rule exists in `ZTK Demo Project/index.html`

**Checkpoint**: Baseline verified ✅ — user story work can begin.

---

## Phase 3: User Story 1 — View Enrollment Carousel (Priority: P1) 🎯 MVP

**Goal**: Carousel slides left/right with 300ms animation, circular wrap, tap-lock during transition.

**Independent Test**: Load demo → tap right 4× → 001→002→003→004→001. Tap left from 001 → 004. Rapid taps during animation do not double-fire.

- [X] T007 [US1] Verify carousel is fully functional by running the independent test above in a browser against `ZTK Demo Project/index.html`. No code changes expected.

**Checkpoint**: US1 ✅ — carousel verified.

---

## Phase 4: User Story 2 — Access Terms and Conditions (Priority: P2)

**Goal**: Tapping bottom 20% of any carousel screen slides in `Terms and Conditions.png`.

**Independent Test**: From each of the four carousel screens, tap the bottom strip (GET STARTED area). Each must display the T&C image inside the phone frame.

- [X] T008 [US2] Verify GET STARTED tap on all 4 carousel screens displays T&C screen in a browser against `ZTK Demo Project/index.html`. No code changes expected.

**Checkpoint**: US2 ✅ — GET STARTED flow verified.

---

## Phase 5: User Story 3 — Accept Terms and Conditions (Priority: P3)

**Goal**: Tapping Accept & Continue on the T&C screen slides in `Choose How to Receive Money P2P.png`. All other T&C taps are no-ops.

**Independent Test**: Navigate to T&C → tap outside Accept & Continue area → nothing. Tap Accept & Continue area → Choose screen slides in from right.

- [X] T009 [US3] Visually inspect `ZTK Demo Project/images/Terms and Conditions.png` (828×1792px) to measure the Accept & Continue button's vertical position. Record the button's top-edge and bottom-edge pixel values (e.g., y≈1480–1600), then convert to percentages of image height (top%=y_top÷1792×100, height%=(y_bottom−y_top)÷1792×100).
- [X] T010 [US3] Update the `#accept-hotspot` CSS rule in `ZTK Demo Project/index.html`: replace `top: 80%; height: 20%` with the calibrated `top` and `height` values from T009. Note: `object-fit: contain` renders the 828×1792px image letterboxed inside the ~266×576px phone screen viewport — any blank bars above/below the image must be accounted for when mapping image percentages to viewport percentages.
- [X] T011 [US3] Open `ZTK Demo Project/index.html` in a browser and verify: (a) tapping the Accept & Continue button area slides in the Choose How to Receive Money P2P screen, (b) tapping above the button does nothing.

**Checkpoint**: US3 ✅ — Accept & Continue hotspot correctly aligned and functional.

---

## Phase 6: User Story 4 — View Choose How to Receive Money (Priority: P4)

**Goal**: Choose How to Receive Money P2P screen is a terminal state; all taps are no-ops.

**Independent Test**: Reach the Choose screen. Tap everywhere — nothing should happen. Page reload is the only way to restart.

- [X] T012 [US4] Open `ZTK Demo Project/index.html` in a browser, navigate to the Choose screen via Accept & Continue, and confirm: left/right zones produce no slide, GET STARTED hotspot is inactive, Accept & Continue hotspot is inactive, and all taps are no-ops.

**Checkpoint**: US4 ✅ — Choose screen terminal state confirmed.

---

## Final Phase: Polish & Cross-Cutting Concerns

**Purpose**: Remove dead code and validate the full end-to-end demo flow.

- [X] T013 [P] Search `ZTK Demo Project/index.html` for any remaining occurrences of `PERMISSION_IMAGE`, `isPermissionScreen`, `showPermission`, or `dismissTerms` — confirm none exist.
- [X] T014 Full end-to-end smoke test per `quickstart.md`: load → right/left carousel wrap → GET STARTED from each of 4 screens → tap outside Accept button (no-op) → tap Accept & Continue → Choose screen reaches terminal state.

---

## Dependencies & Execution Order

### Phase Dependencies

- **Phase 1 (Setup)**: No dependencies — start immediately
- **Phase 2 (Foundational)**: Depends on Phase 1
- **Phase 3 (US1)**: Depends on Phase 2 — independent of US2/US3/US4
- **Phase 4 (US2)**: Depends on Phase 2 — independent of US1/US3/US4
- **Phase 5 (US3)**: Depends on Phase 2; do after Phase 4 (T&C screen must be reachable)
- **Phase 6 (US4)**: Depends on Phase 5 (Choose screen only reachable via Accept & Continue)
- **Final Phase**: Depends on all story phases complete

### Within Phase 5

- T009 → T010 → T011 (sequential: measure → apply → verify)

### Parallel Opportunities

- T002–T006 (Phase 2) are independent — can be verified in any order
- T007 and T008 (US1 and US2 verification) are independent
- T013 and T014 (Final Phase) can run in parallel

---

## Parallel Example: Phase 2

```
# All baseline verification checks are independent:
T002: Confirm TERMS_IMAGE constant
T003: Confirm CHOOSE_IMAGE constant
T004: Confirm isTermsScreen / isChooseScreen state vars
T005: Confirm all 300ms durations
T006: Confirm #accept-hotspot presence
```

---

## Implementation Strategy

### Status

- **US1 (Carousel)**: ✅ Complete
- **US2 (GET STARTED → T&C)**: ✅ Complete
- **US3 (Accept & Continue → Choose)**: ✅ Complete — `#accept-hotspot` calibrated to `top:93%; height:6%` (button y=1672–1766 in 1792px image)
- **US4 (Terminal Choose screen)**: ✅ Complete

### All tasks complete ✅

T001–T014 all marked done. Implementation is 100% complete.

---

## Notes

- Tasks T001–T008 are marked `[X]` — already completed in prior implementation sessions
- The only code change remaining is T010: update `top` and `height` on `#accept-hotspot` CSS rule in `ZTK Demo Project/index.html`
- T009 requires opening the PNG in an image viewer — no coding
- [P] tasks within the same phase can be done in any order
