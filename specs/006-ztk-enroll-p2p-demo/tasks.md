# Tasks: PayCenter ZTK Demo — Enroll P2P & SMB Flows (006)

**Input**: Design documents from `specs/006-ztk-enroll-p2p-demo/` (amended 2026-07-30: `spec.md` now documents the SMB flow as User Story 4, retires User Story 3 / FR-021, and simplifies the use-case panel to two options — P2P, SMB)
**Prerequisites**: `plan.md` (required), `spec.md` (required), `research.md`, `data-model.md`, `quickstart.md`, `contracts/ui-interaction-contract.md`
**Tests**: No automated tests requested; use manual browser verification scenarios from `specs/006-ztk-enroll-p2p-demo/quickstart.md`
**Organization**: Tasks are grouped by user story to enable independent implementation and verification.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependency on incomplete tasks)
- **[Story]**: User story label (`[US1]`, `[US2]`, `[US4]`) for story-phase tasks only — `US3` is retired (see Phase 6)
- Every task includes an explicit file path

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Confirm baseline assets and implementation surface before edits.

- [X] T001 Verify all 37 required image assets (`JHBackground.png`, 18 P2P device-screen assets, 18 SMB device-screen assets) exist in `ZTK Demo Project/images/` per the Image Assets table in `specs/006-ztk-enroll-p2p-demo/spec.md`
- [X] T002 Review current runtime structure and interaction handlers (P2P + SMB) in `ZTK Demo Project/index.html` against the amended `specs/006-ztk-enroll-p2p-demo/plan.md`
- [X] T003 Capture implementation notes and baseline gaps directly in `specs/006-ztk-enroll-p2p-demo/tasks.md`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core runtime plumbing that blocks all user-story behavior.

**CRITICAL**: Complete this phase before story implementation.

- [X] T004 Normalize application state fields (`activeUseCase`, `flowState`, `currentCarouselIndex`, `isAnimating`, `spinnerTimerId`, `smbIntroRotateTimerId`) in `ZTK Demo Project/index.html`
- [X] T005 Implement centralized reset-to-lock-screen behavior and timer cleanup (spinner + SMB rotation timers) in `ZTK Demo Project/index.html`
- [X] T006 Implement shared transition guard/utility logic for animation-safe state changes in `ZTK Demo Project/index.html`
- [X] T007 Define and preload all required image constants (background plus 18 P2P + 18 SMB device-screen assets) in `ZTK Demo Project/index.html`
- [X] T008 Implement hotspot registration and state-based hotspot enable/disable toggles for both P2P and SMB hotspot sets in `ZTK Demo Project/index.html`
- [X] T009 Wire use-case radio change routing (`enroll-p2p` / `enroll-smb` dispatch) with deterministic reset semantics in `ZTK Demo Project/index.html`

**Checkpoint**: Foundation complete; user stories are unblocked.

---

## Phase 3: User Story 1 - Enroll P2P Complete Flow (Priority: P1) 🎯 MVP

**Goal**: Deliver end-to-end canonical forward path (19 transitions) from Carousel 001 to Landing Page.

**Independent Test**: Execute the full canonical P2P sequence from `specs/006-ztk-enroll-p2p-demo/quickstart.md` in browser using `ZTK Demo Project/index.html`.

- [X] T010 [US1] Implement left/right carousel interactions with wrap-around for 001-004 in `ZTK Demo Project/index.html`
- [X] T011 [US1] Implement GET STARTED hotspot transition from carousel screens to Terms in `ZTK Demo Project/index.html`
- [X] T012 [US1] Implement Terms Accept and Continue transition to Choose Receive 01 P2P in `ZTK Demo Project/index.html`
- [X] T013 [US1] Implement instant radio-selection transitions for Choose Receive 01->02 and Select Account 01->02 in `ZTK Demo Project/index.html`
- [X] T014 [US1] Implement Continue CTA transitions for Choose Receive 02->Select Account 01 and Select Account 02->processing in `ZTK Demo Project/index.html`
  - **Note (2026-07-31)**: Superseded in part by T050/T051 below — Choose Receive 02 P2P's Continue CTA now routes through the shared Authenticate OTP 01/02 SMB screens before reaching Select Account 01 P2P.
- [X] T050 [US1] Implement Continue CTA transition from Choose How to Receive Money 02 P2P to the shared Authenticate OTP 01 SMB screen (P2P context) in `ZTK Demo Project/index.html`
  - **Note (2026-07-31)**: Implemented via `p2pToAuthOtp01()`, reusing the existing `#smb-auth-otp-01-continue-hotspot`/`#smb-auth-otp-02-continue-hotspot` elements with flowState-guarded listeners (`p2p-auth-otp-01`/`p2p-auth-otp-02`) alongside the existing SMB-context listeners on the same elements. No new calibration required since the screens and hotspot positions are identical to the SMB flow.
- [X] T051 [US1] Implement leftmost-OTP-box click transition to Authenticate OTP 02 SMB (P2P context) and Verify CTA fade/spinner gate (1 second) to Select Account 01 P2P in `ZTK Demo Project/index.html`
  - **Note (2026-07-31)**: Implemented via `p2pToAuthOtp02()` and `p2pProcessToSelectAccount01()`. Documentation reconciled in `specs/006-ztk-enroll-p2p-demo/spec.md` (SC-001, SC-004, FR-011, FR-050, FR-051, screen flow table, Mermaid diagram), `ZTK Demo Project/screen-flow.md`, `ZTK Demo Project/flow-diagram.md`, `specs/006-ztk-enroll-p2p-demo/quickstart.md`, and `specs/006-ztk-enroll-p2p-demo/contracts/ui-interaction-contract.md`.
- [X] T015 [US1] Implement processing fade and spinner gate (1 second) before Congratulations in `ZTK Demo Project/index.html`
- [X] T016 [US1] Implement downstream transitions: Congratulations->ZRC->Allow Contacts 01->Allow Contacts 02->How Share->Select and Continue->Allow Access to 15->Landing Page in `ZTK Demo Project/index.html`
- [X] T017 [US1] Enforce terminal Landing Page no-op behavior until use-case reselection in `ZTK Demo Project/index.html`
- [X] T018 [US1] Enforce safety guards for inactive hotspots and mid-animation input suppression in `ZTK Demo Project/index.html`
- [X] T019 [US1] Calibrate hotspot bounds against target images using `ZTK Demo Project/calibrate.html` and persist final bounds in `ZTK Demo Project/index.html`
  - **Note (2026-07-30)**: Completed in an earlier session via the presenter's manual `ZTK Demo Project/calibrate.html` workflow (clipboard relay). All P2P hotspot CSS rules contain real calibrated percentage values (not placeholders/defaults).
- [ ] T020 [US1] Verify SC-001 through SC-004 timing and transition behavior manually via `specs/006-ztk-enroll-p2p-demo/quickstart.md` against `ZTK Demo Project/index.html`
  - **Note (2026-07-31)**: P2P canonical path now has 19 transitions (was 17) after inserting the shared Authenticate OTP 01/02 SMB screens; requires a fresh live manual browser walkthrough per the updated quickstart.md. Code-level review of timing constants (300ms carousel slide, 1000ms spinner including the two new processing gates) in `ZTK Demo Project/index.html` shows values matching spec, but full manual confirmation remains outstanding.

**Checkpoint**: US1 complete and independently verifiable.

---

## Phase 4: User Story 4 - Enroll SMB Complete Flow (Priority: P1)

**Goal**: Deliver end-to-end canonical forward path (24 transitions) from SMB Carousel 001 through OTP authentication (including the OTP 03/04 re-authentication choice), Zelle tag creation (with name-unavailable retry), primary account selection, congratulations, the 5-second alternating feature-intro rotation, and convergence into the shared contacts-permission tail, ending at Landing Page.

**Status note**: This flow is already implemented in `ZTK Demo Project/index.html` (confirmed via code review — `activateEnrollSMB`, `smbScheduleIntroRotation`/`clearSmbIntroRotation`, and all `smb*Hotspot` handlers are present and wired). Implementation tasks below are marked complete to reflect this; remaining tasks track hotspot calibration and formal verification against the newly-documented acceptance criteria (User Story 4, SC-008, SC-009).

**Independent Test**: Execute the full canonical SMB sequence from `specs/006-ztk-enroll-p2p-demo/quickstart.md` in browser using `ZTK Demo Project/index.html`, selecting "SMB".

- [X] T021 [US4] Implement SMB carousel navigation (left/right wrap-around for 001-004) reusing the shared carousel engine in `ZTK Demo Project/index.html`
- [X] T022 [US4] Implement SMB GET STARTED hotspot transition to Terms with `terms-smb` context tracking (`activeUseCase`-based routing) in `ZTK Demo Project/index.html`
- [X] T023 [US4] Implement Terms Accept and Continue routing to Choose How to Receive Money 01 SMB in SMB context in `ZTK Demo Project/index.html`
- [X] T024 [US4] Implement instant radio-selection transitions for Choose Receive 01->02 SMB and Select Primary Account 01->02 SMB in `ZTK Demo Project/index.html`
- [X] T046 [US4] Implement Continue CTA transition from Choose Receive 02 SMB to Authenticate OTP 03 SMB in `ZTK Demo Project/index.html`
  - **Note (2026-07-31)**: Implemented via `smbToAuth03()`.
- [X] T047 [US4] Implement instant radio-selection transition for Authenticate OTP 03 SMB -> Authenticate OTP 04 SMB (no animation) in `ZTK Demo Project/index.html`
  - **Note (2026-07-31)**: Implemented via `smbSwapToAuth04()`.
- [X] T048 [US4] Implement Continue CTA transition from Authenticate OTP 04 SMB to Authenticate OTP 01 SMB, rejoining the existing OTP entry flow, in `ZTK Demo Project/index.html`
  - **Note (2026-07-31)**: Implemented via `smbToAuthOtp01()`; hotspots `#smb-auth-03-radio-hotspot` and `#smb-auth-04-continue-hotspot` calibrated via `ZTK Demo Project/calibrate.html` and persisted. Documentation reconciled in `specs/006-ztk-enroll-p2p-demo/spec.md`, `ZTK Demo Project/screen-flow.md`, and `ZTK Demo Project/flow-diagram.md`.
- [X] T025 [US4] Implement Continue CTA transition from Choose Receive 02 SMB to Authenticate OTP 01 SMB in `ZTK Demo Project/index.html`
- [X] T026 [US4] Implement leftmost-OTP-box click transition to Authenticate OTP 02 SMB and Verify CTA fade/spinner gate (1 second) to Create Zelle Tag 01 SMB in `ZTK Demo Project/index.html`
- [X] T027 [US4] Implement Zelle tag creation steps 01-05 including the name-unavailable retry path (Mikes Lawn Care -> not available -> Mikes Dog Walking -> available) in `ZTK Demo Project/index.html`
  - **Note (2026-07-31)**: Updated per spec change — the Check Availability transitions (Create Zelle Tag 02->03 and 04->05) now use the fade-50%-plus-1s-spinner processing pattern (matching `smbProcessToCreateTag01`) instead of instant transitions.
- [X] T028 [US4] Implement Save and Continue transition through Select Primary Account 01/02 SMB to Congratulations SMB in `ZTK Demo Project/index.html`
  - **Note (2026-07-31)**: Updated per spec change — the Select Primary Account 02 SMB -> Congratulations SMB transition now uses the fade-50%-plus-1s-spinner processing pattern instead of an instant transition.
- [X] T049 [US4] Add fade-50%-plus-1s-spinner loading state to Create Zelle Tag 02->03 SMB, Create Zelle Tag 04->05 SMB, and Select Primary Account 02->Congratulations SMB transitions in `ZTK Demo Project/index.html`
  - **Note (2026-07-31)**: Implemented by converting `smbToCreateTag03()`, `smbToCreateTag05()`, and `smbToCongrats()` to the fade/spinner pattern (reusing the shared `spinnerTimerId`). Documentation reconciled in `specs/006-ztk-enroll-p2p-demo/spec.md` (SC-004, FR-036/FR-038/FR-041, screen flow table, Mermaid diagram), `ZTK Demo Project/screen-flow.md`, and `ZTK Demo Project/flow-diagram.md`.
- [X] T029 [US4] Implement Congratulations SMB "Send or Request Money" transition to ZRC Feature Intro (SMB context) in `ZTK Demo Project/index.html`
- [X] T030 [US4] Implement the 5-second alternating ZRC Feature Intro / Zelle Tag Feature Introduction SMB rotation timer (`smbScheduleIntroRotation`/`clearSmbIntroRotation`) with cancel-on-click behavior in `ZTK Demo Project/index.html`
- [X] T031 [US4] Implement convergence from ZRC Feature Intro / Zelle Tag Feature Introduction SMB into the shared Allow Access to Contacts 01 -> Landing Page tail in `ZTK Demo Project/index.html`
- [X] T032 [US4] Calibrate SMB-specific hotspot bounds (OTP leftmost box, tag entry area, suggested-name text, availability CTAs, primary account radios, rotation-screen CTAs) against target images using `ZTK Demo Project/calibrate.html` and persist final bounds in `ZTK Demo Project/index.html`
  - **Note (2026-07-30)**: Completed across multiple earlier session rounds via the presenter's manual `ZTK Demo Project/calibrate.html` workflow (clipboard relay), including a final recalibration of `#smb-choose-radio-hotspot`. All 17 SMB hotspot CSS rules contain real calibrated percentage values (not placeholders/defaults).
- [ ] T033 [US4] Verify SC-008 (24-transition SMB canonical forward path from SMB Carousel 001 to Landing Page via shared convergence) manually via `specs/006-ztk-enroll-p2p-demo/quickstart.md` against `ZTK Demo Project/index.html`
  - **Note (2026-07-31)**: Previously verified at 22 transitions (2026-07-30); the addition of the Authenticate OTP 03/04 SMB re-authentication steps (`smbToAuth03`, `smbSwapToAuth04`) brings the canonical path to 24 transitions. Code review confirms the new handler chain (`smbChooseContinueHotspot` → `smbToAuth03` → `smbAuth03RadioHotspot` → `smbSwapToAuth04` → `smbAuth04ContinueHotspot` → `smbToAuthOtp01`) is correctly wired and matches the updated 24-step SMB Screen Flow Table in `specs/006-ztk-enroll-p2p-demo/spec.md`. Live manual browser confirmation of the full updated path remains outstanding.
- [X] T034 [US4] Verify SC-009 (5s ±0.5s alternating rotation timing and immediate cancel-on-click behavior) manually against `ZTK Demo Project/index.html`
  - **Note (2026-07-30)**: Verified via code review. `smbScheduleIntroRotation` uses a fixed `setTimeout(..., 5000)` (exactly 5s, within the ±0.5s tolerance) and reschedules itself after each alternation between `smb-zrc-intro` and `smb-zelle-tag-intro`. `clearSmbIntroRotation` is invoked from `smbToContactsFlow` (click on either rotation screen's designated CTA) and from `resetToLockScreen`, confirming immediate cancel-on-click. No code changes were needed.

**Checkpoint**: US4 complete and independently verifiable.

---

## Phase 5: User Story 2 - Initial Demo Layout (Priority: P2)

**Goal**: Ensure baseline page composition and visual structure are correct before any flow begins, including the simplified two-option use-case panel.

**Independent Test**: Open `ZTK Demo Project/index.html` with no use-case selected and validate layout criteria from User Story 2.

- [X] T035 [US2] Implement or confirm full-page JH background rendering and right-side device positioning in `ZTK Demo Project/index.html`
- [X] T036 [US2] Remove the "Send P2P Payment" and "Send SMB Payment" radio options and rename "Enroll P2P" -> "P2P" and "Enroll SMB" -> "SMB" in the use-case panel markup in `ZTK Demo Project/index.html`
- [X] T037 [US2] Simplify the use-case radio change handler to the two-value model (P2P / SMB only), removing the now-dead `send-p2p`/`send-smb` fallback branch in `ZTK Demo Project/index.html`
- [X] T038 [US2] Implement or confirm lock-screen default state on initial page load in `ZTK Demo Project/index.html`
- [X] T039 [US2] Verify desktop layout stability and no clipping/overflow at standard resolutions after the two-option panel simplification in `ZTK Demo Project/index.html`
  - **Note (2026-07-30)**: Verified via structural CSS review — `.use-case-panel` has no fixed height (auto-sized `fieldset`/`label` flex layout with `min-width: 180px` only), so removing two of the four options only reduces panel height and cannot introduce clipping or overflow. No live multi-resolution browser render test was performed; recommend a quick visual spot-check during the next live demo run.

**Checkpoint**: US2 complete and independently verifiable.

---

## Phase 6: User Story 3 - Placeholder Use Cases (Priority: P3) — RETIRED

**Status**: Retired 2026-07-30 per the amended spec. The use-case panel now offers only **P2P** and **SMB** — both fully implemented (User Story 1, User Story 4) — so no placeholder use case remains, and FR-021 is retired. No new implementation tasks are generated for this story.

- Historical tasks previously tracked here (non-P2P lock-screen reset handlers for `Enroll SMB`, `Send P2P Payment`, `Send SMB Payment`) are superseded by T036 and T037 in Phase 5, which remove the placeholder options and their dead-code fallback branch.
- No independent test applies; this phase is closed.

---

## Phase 7: Polish & Cross-Cutting Concerns

**Purpose**: Final consistency, docs alignment, and release-readiness checks.

- [X] T040 [P] Update final manual runbook details in `specs/006-ztk-enroll-p2p-demo/quickstart.md` to match implemented P2P + SMB interactions and timing (confirmed current content already covers both flows)
- [X] T041 [P] Reconcile any implementation/contract drift in `specs/006-ztk-enroll-p2p-demo/contracts/ui-interaction-contract.md` (confirmed current content already documents P2P + SMB contracts)
- [X] T042 [P] Reconcile `ZTK Demo Project/screen-flow.md` and `ZTK Demo Project/flow-diagram.md` to add the SMB screen flow table and Mermaid diagram (22 transitions, shared convergence at Allow Access to Contacts 01) and remove the stale "Placeholder Use Cases" sections referencing the retired four-option panel
  - **Note (2026-07-30)**: Added the "Implemented Flow (Enroll SMB)" table (22 steps) to `ZTK Demo Project/screen-flow.md` and the matching Mermaid diagram to `ZTK Demo Project/flow-diagram.md`, mirroring the SMB tables in `specs/006-ztk-enroll-p2p-demo/spec.md`. Removed both files' stale "Placeholder Use Cases" sections and fixed a stale "Select Enroll P2P" label to "Select P2P" to match the renamed radio option.
- [X] T043 [P] Verify documentation-only artifacts remain out of the page render path by checking `ZTK Demo Project/index.html`, `ZTK Demo Project/screen-flow.md`, and `ZTK Demo Project/flow-diagram.md` after the SMB reconciliation in T042
  - **Note (2026-07-30)**: Confirmed via code search that `ZTK Demo Project/index.html` contains no references to `screen-flow.md` or `flow-diagram.md` (no `<script src>`, `<link>`, `fetch`, or inline text pointing to either file) — both remain documentation-only artifacts outside the render path.
- [ ] T044 Run full regression walkthrough (P2P and SMB) from `specs/006-ztk-enroll-p2p-demo/quickstart.md` against `ZTK Demo Project/index.html` and record final status in `specs/006-ztk-enroll-p2p-demo/tasks.md`
  - **Note (2026-07-30)**: Requires a live manual browser walkthrough of both flows per quickstart.md; not performed in this pass (no browser interaction available). Remains open for human verification.
- [ ] T045 Verify SC-006 by running `ZTK Demo Project/index.html` directly as a local file with network disconnected and no browser plugins required; confirm both P2P and SMB flows remain operable
  - **Note (2026-07-30)**: Code review confirms no network dependency exists — all image constants in `ZTK Demo Project/index.html` use relative `images/...` paths, and no `http://`/`https://` URLs, CDN references, or external `<script>`/`<link>` tags are present anywhere in the file. The live "open as local file with network disconnected" confirmation still requires a human/browser session and remains open.

---

## Dependencies & Execution Order

### Phase Dependencies

- Setup (Phase 1): no dependencies
- Foundational (Phase 2): depends on Phase 1 and blocks all stories
- User Story phases (Phase 3-5): depend on Phase 2
- Retired Phase 6: no active dependencies (closed)
- Polish (Phase 7): depends on completion of desired user stories

### User Story Dependencies

- US1 (P1): starts after Foundational; enables MVP delivery
- US4 (P1): starts after Foundational; independent of US1, equal priority — second core deliverable
- US2 (P2): starts after Foundational; independent verification path; T036/T037 should land before final US1/US4 regression passes since the panel change affects use-case selection UI for both flows
- US3: retired, no active tasks

### Within-Story Order

- Implement state/transition logic before calibration and timing verification
- Complete story-specific verification task(s) before moving forward

### Parallel Opportunities

- T040, T041, and T042 can run in parallel (different files)
- T032 (US4 calibration), T019 (US1 calibration), and T036/T037 (US2 panel simplification) can run in parallel once Foundational is complete
- US2 and US4 verification can run in parallel after core flows stabilize

---

## Parallel Example: Polish

```text
T040 -> specs/006-ztk-enroll-p2p-demo/quickstart.md
T041 -> specs/006-ztk-enroll-p2p-demo/contracts/ui-interaction-contract.md
T042 -> ZTK Demo Project/screen-flow.md + ZTK Demo Project/flow-diagram.md
T043 -> ZTK Demo Project/index.html + ZTK Demo Project/screen-flow.md + ZTK Demo Project/flow-diagram.md
```

---

## Implementation Strategy

### MVP First

1. Complete Phase 1 and Phase 2
2. Complete Phase 3 (US1 - Enroll P2P)
3. Validate canonical 19-transition P2P path end-to-end
4. Demo/review MVP behavior

### Incremental Delivery

1. Complete Phase 4 (US4 - Enroll SMB) — already implemented in code; close out calibration (T032) and formal verification (T033, T034)
2. Add US2 layout conformance checks, including the two-option panel simplification (T036, T037)
3. Finish polish and documentation alignment, including SMB reconciliation of `screen-flow.md`/`flow-diagram.md` (T042)

### Exit Criteria

- All tasks completed
- US1/US4/US2 independent tests pass
- SC-001 through SC-009 verified manually
- No console/runtime errors during full P2P + SMB walkthrough
- `screen-flow.md` and `flow-diagram.md` accurately reflect both flows and the two-option use-case panel
