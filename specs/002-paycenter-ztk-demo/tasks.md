# Tasks: PayCenter ZTK Demo — Zelle P2P Enrollment Carousel

**Input**: Design documents from `specs/002-paycenter-ztk-demo/`
**Prerequisites**: plan.md ✅ | spec.md ✅ | research.md ✅ | data-model.md ✅ | quickstart.md ✅

---

## Phase 1: Setup

**Status**: ✅ Complete — `ZTK Demo Project/index.html` and all 5 images in place.

---

## Phase 2: Foundational

**Status**: ✅ Complete — phone mockup, image registry, and two-image slide infrastructure implemented.

---

## Phase 3: User Story 1 — View Initial Enrollment Screen (P1) 🎯

**Status**: ✅ Complete

**Independent Test**: Open `ZTK Demo Project/index.html`. Confirm phone mockup renders with image 001 filling the screen.

---

## Phase 4: User Story 2 — Forward Navigation (P1)

**Status**: ✅ Complete

**Independent Test**: Click the right half 4 times. Confirm 001→002→003→004→001 with a left-slide animation each time.

---

## Phase 5: User Story 3 — Backward Navigation (P2)

**Status**: ✅ Complete

**Independent Test**: Navigate to any screen, click the left half, confirm the previous screen slides in from the left. Verify 001→004 wrap-around.

---

## Phase 6: User Story 4 — GET STARTED → Contacts Permission Screen (P2)

**Status**: ✅ Complete

**Independent Test**: From any carousel screen, click the GET STARTED area. Confirm permission screen slides in from the right with no border, outline, or shadow. Confirm clicking either half of the screen does nothing.

### Tasks

- [X] T001 [US4] Add `#cta-hotspot` div and `showPermission()` function in `ZTK Demo Project/index.html`
- [X] T002 [US4] Add `isPermissionScreen` flag; guard `advance()` and `retreat()` in `ZTK Demo Project/index.html`
- [X] T003 [US4] Disable `clickZone` and `leftZone` pointer-events/cursor when permission screen activates in `ZTK Demo Project/index.html`
- [X] T004 [US4] Add `box-shadow: none` to `.slide` CSS rule in `ZTK Demo Project/index.html`
- [X] T005 Enable GET STARTED hotspot on all 4 carousel screens (not just screen 4) in `ZTK Demo Project/index.html`

**Checkpoint**: ✅ All User Story 4 tasks complete

---

## Phase 7: Polish & Cross-Cutting Concerns

**Status**: ✅ Complete

- [X] T006 [P] Verify full forward/backward carousel flow
- [X] T007 [P] Verify GET STARTED → permission screen (no decoration)
- [X] T008 Verify permission screen terminal state (no nav possible)

---

## Deferred

- [ ] T009 Permission screen — next interaction (requirements forthcoming; run `/speckit.clarify` when ready)

---

## Dependency Graph

```
All T001–T008 complete ✅
T009 blocked on future requirements
```

---

## Implementation Strategy

**Current status**: Demo is fully functional and ready for presentation.  
**Next step**: Gather requirements for post-permission flow, then run `/speckit.clarify` → `/speckit.tasks` → `/speckit.implement`.

**Input**: Design documents from `specs/002-paycenter-ztk-demo/`
**Prerequisites**: plan.md ✅ | spec.md ✅ | research.md ✅ | data-model.md ✅ | quickstart.md ✅

---

## Phase 1: Setup (Shared Infrastructure)

**Status**: ✅ Already complete — no tasks required

---

## Phase 2: Foundational (Blocking Prerequisites)

**Status**: ✅ Already complete — no tasks required

---

## Phase 3: User Story 1 — View Initial Enrollment Screen (Priority: P1) 🎯

**Status**: ✅ Already complete — no tasks required

**Independent Test**: Open `ZTK Demo Project/index.html`. Confirm phone mockup renders with image 001 filling the screen.

---

## Phase 4: User Story 2 — Forward Navigation (Priority: P1)

**Status**: ✅ Already complete — no tasks required

**Independent Test**: Click the right half 4 times. Confirm 001→002→003→004→001 with left-slide animation each time.

---

## Phase 5: User Story 3 — Backward Navigation (Priority: P2)

**Status**: ✅ Already complete — no tasks required

**Independent Test**: Navigate to image 002, click left half, confirm right-slide to 001. Verify 001→004 wrap-around.

---

## Phase 6: User Story 4 — GET STARTED CTA → Contacts Permission Screen (Priority: P2)

**Goal**: Clicking GET STARTED on screen 4 transitions to the permission screen; both nav zones are disabled while the permission screen is showing.

**Independent Test**: Navigate to image 004, click the GET STARTED area, confirm permission screen slides in from the right. Then click the left or right half of the screen — confirm nothing happens.

### Implementation for User Story 4

- [X] T001 [US4] Add `showPermission()` function and `#cta-hotspot` click handler in `ZTK Demo Project/index.html` — transitions to Zelle Access Contacts Permission screen on GET STARTED click

- [X] T002 [US4] Add `isPermissionScreen` boolean flag to `ZTK Demo Project/index.html`: declare `let isPermissionScreen = false;` alongside existing state variables; set to `true` at the end of `showPermission()`'s `setTimeout` callback; add guard `if (isAnimating || isPermissionScreen) return;` to both `advance()` and `retreat()`

- [X] T003 [US4] Disable nav zone cursor affordance on permission screen in `ZTK Demo Project/index.html`: after `isPermissionScreen = true` in `showPermission()`, add `clickZone.style.pointerEvents = 'none'; leftZone.style.pointerEvents = 'none';` and change their cursors to `default`

- [X] T004 [US4] Add `box-shadow: none` to the `.slide` CSS rule in `ZTK Demo Project/index.html` to ensure no browser decoration appears on the permission image (alongside existing `border: none; outline: none`)

**Checkpoint**: GET STARTED works; permission screen shows; clicking either nav half does nothing ✅

---

## Phase 7: Polish & Cross-Cutting Concerns

**Goal**: Verify all acceptance scenarios end-to-end

- [X] T005 [P] Manually verify full forward/backward carousel flow in `ZTK Demo Project/index.html`: 001→002→003→004→001 and 001→004→003→002→001 with correct slide directions

- [X] T006 [P] Manually verify GET STARTED flow in `ZTK Demo Project/index.html`: navigate to image 004, click GET STARTED area, confirm permission screen appears without border/shadow decoration

- [X] T007 Manually verify permission screen terminal state in `ZTK Demo Project/index.html`: while permission screen is showing, click left half and right half — confirm neither triggers any navigation or animation

**Checkpoint**: All user stories verified ✅ — demo ready for next requirements session

---

## Dependency Graph

```
T002 ──► T003 ──────────────────────────► T005
T004 ────────────────────────────────────► T006
                                           T007
```

T002 must complete before T003 (both touch the same function). T004 is independent.

---

## Implementation Strategy

**MVP**: T002 + T003 + T004 — all in one edit to `index.html`.

**Suggested order**:
1. T002 + T003 + T004 in a single combined edit
2. T005 + T006 + T007 manual verification

Total remaining work: **3 implementation tasks**, all in `ZTK Demo Project/index.html`.

**Input**: Design documents from `specs/002-paycenter-ztk-demo/`
**Prerequisites**: plan.md ✅ | spec.md ✅ | research.md ✅ | data-model.md ✅ | quickstart.md ✅

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project skeleton and image assets  
**Status**: ✅ Already complete — no tasks required

> `ZTK Demo Project/index.html` exists. All 4 PNG images are already present in `ZTK Demo Project/images/`. No build toolchain needed.

**Checkpoint**: Foundation ready ✅

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Phone mockup, image registry, and carousel infrastructure that US1 and US2 depend on  
**Status**: ✅ Already complete — no tasks required

> Phone frame CSS, `.phone-screen` overflow viewport, `IMAGES` array, `#slide-current` / `#slide-incoming` layered `<img>` elements, `isAnimating` debounce flag, and `advance()` forward navigation are all implemented.

**Checkpoint**: All foundational carousel infrastructure in place ✅

---

## Phase 3: User Story 1 — View Initial Enrollment Screen (Priority: P1) 🎯 MVP

**Goal**: Phone mockup loads with image 001 displayed in the screen area  
**Status**: ✅ Already complete — no tasks required

**Independent Test**: Open `ZTK Demo Project/index.html` in a browser. Confirm the phone mockup renders and image 001 (`Zelle P2P Enrollment Carousel Image 001.png`) is displayed.

**Checkpoint**: US1 fully functional and verified ✅

---

## Phase 4: User Story 2 — Forward Navigation (Priority: P1)

**Goal**: Right-zone click advances carousel forward with left-slide animation, wraps 004→001  
**Status**: ✅ Already complete — no tasks required

**Independent Test**: Click the right half of the phone screen 4 times. Confirm: 001→002→003→004→001 each with a left-slide animation. Rapid clicks must be ignored during transition.

**Checkpoint**: US2 fully functional and verified ✅

---

## Phase 5: User Story 3 — Backward Navigation (Priority: P2)

**Goal**: Left-zone click retreats carousel backward with right-slide animation, wraps 001→004

**Independent Test**: Navigate to any screen other than 001, click the left half, confirm the previous screen slides in from the left. Verify 001→004 wrap-around.

### Implementation for User Story 3

- [X] T001 [US3] Fix `.left-zone` CSS rule: change `cursor: default` to `cursor: pointer` in `ZTK Demo Project/index.html`

- [X] T002 [US3] Add `retreat()` function to `ZTK Demo Project/index.html` — mirrors `advance()` but uses `prevIndex = (currentIndex - 1 + IMAGES.length) % IMAGES.length`, positions incoming at `translateX(-100%)`, and slides current to `translateX(+100%)`

- [X] T003 [US3] Wire left-zone click handler: add `leftZone.addEventListener('click', retreat)` in `ZTK Demo Project/index.html` (requires T002; add `const leftZone = document.querySelector('.left-zone')` selector alongside existing `clickZone` selector)

**Checkpoint**: US3 complete — backward navigation works in both directions with wrap-around ✅

---

## Phase 6: Polish & Cross-Cutting Concerns

**Goal**: Verify all acceptance scenarios across user stories; confirm cross-browser rendering

- [X] T004 [P] Manually verify US1 + US2 acceptance scenarios in `ZTK Demo Project/index.html`: open file, confirm phone mockup displays image 001 on load; click right zone 4 times and confirm 001→002→003→004→001 with left-slide animations

- [X] T005 [P] Manually verify US3 acceptance scenarios in `ZTK Demo Project/index.html`: navigate to image 002, click left zone and confirm slide right to 001; navigate to 001, click left zone and confirm wrap-around to 004; navigate to 004, click left zone and confirm 003 appears

- [X] T006 Manually verify debounce in `ZTK Demo Project/index.html`: click right zone rapidly 5+ times and confirm no broken/out-of-sequence transitions occur

**Checkpoint**: All 3 user stories verified ✅ — demo ready for presentation

---

## Dependency Graph

```
T001 ──────────────────────────────────────────► T004 (US3 done)
T002 ──► T003 ─────────────────────────────────► T004
                                                  T005
                                                  T006
```

US3 implementation (T001–T003) must complete before polish verification (T004–T006).  
T001 and T002 are independent of each other and can be done in parallel (different sections of `index.html`).

---

## Parallel Execution

T001 and T002 touch different parts of the same file (CSS block vs. `<script>` block) and can be written in a single file edit:

```
Phase 5 parallel batch:
  T001 + T002 → single edit to index.html (CSS fix + retreat() function)
  T003        → second edit to wire the event listener
```

---

## Implementation Strategy

**MVP scope**: T001 + T002 + T003 complete the only missing feature (US3 backward navigation). The demo is immediately presentable after these three tasks.

**Suggested order**:
1. T001 + T002 in one combined edit to `index.html`
2. T003 to wire the handler
3. T004 + T005 + T006 verification

Total remaining work: **3 implementation tasks** across a single file.
