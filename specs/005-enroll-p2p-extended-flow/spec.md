# Feature Specification: Enroll P2P Extended Flow – Choose, Select Account & Spinner

**Feature Branch**: `005-enroll-p2p-extended-flow`  
**Created**: 2026-05-19  
**Status**: Draft  
**Input**: User description: "Extend the Enroll P2P demo flow beyond the Terms & Conditions screen: radio button selection on Choose How to Receive Money screens, Continue CTA navigation to Select Account screens, radio button selection on Select Account screens, and a fade + spinner transition from Select Account 02."

## Clarifications

### Session 2026-05-19

- Q: What screen should display after the 1-second spinner completes? → A: "Congratulations P2P" screen.
- Q: After "Congratulations P2P" appears, is the screen fully inert or is any tap area active? → A: Fully inert — no tap zones active; presenter reloads the page to restart.
- Q: How long should the fade-out last on Select Account 02 before the Congratulations screen appears? → A: The fade persists continuously — the screen stays faded/dark with the spinner overlaid for the full 1 second, then Congratulations P2P appears (replacing the faded state).
- Q: When Congratulations P2P is showing, does selecting a use case from the radio panel reset to blank? → A: Yes — any use-case radio change from any screen (including Congratulations) always resets the phone to blank.
- Note: Step 7 typo confirmed — "display the Choose How to Receive Money 01 P2P image" is interpreted as 02 P2P (documented in Assumptions).

## User Scenarios & Testing *(mandatory)*

### User Story 1 – Radio Button on Choose How to Receive Money 01 Swaps to 02 (Priority: P1)

After accepting Terms & Conditions, the phone displays "Choose How to Receive Money 01 P2P." The presenter selects one of the two radio buttons on that screen. The display immediately swaps to the "Choose How to Receive Money 02 P2P" image with no slide animation — the substitution is instant.

**Why this priority**: This is the next step in the enrollment flow and gates all subsequent screens.

**Independent Test**: Navigate to Choose How to Receive Money 01 P2P, tap a radio button area → the screen instantly changes to 02 with no animation.

**Acceptance Scenarios**:

1. **Given** "Choose How to Receive Money 01 P2P" is displayed, **When** the presenter taps the first radio button area, **Then** "Choose How to Receive Money 02 P2P" appears immediately (no slide transition).
2. **Given** "Choose How to Receive Money 01 P2P" is displayed, **When** the presenter taps the second radio button area, **Then** "Choose How to Receive Money 02 P2P" appears immediately (no slide transition).
3. **Given** "Choose How to Receive Money 01 P2P" is displayed, **When** the presenter taps anywhere outside the radio button zones, **Then** no screen transition occurs.

---

### User Story 2 – Continue CTA on Choose How to Receive Money 02 Navigates to Select Account 01 (Priority: P1)

With "Choose How to Receive Money 02 P2P" on screen, the presenter taps the "Continue" call-to-action area. The phone screen slides to "Select Account 01 P2P" with the standard 300 ms left-slide animation.

**Why this priority**: Required to progress the demo flow; otherwise the journey dead-ends at 02.

**Independent Test**: With Choose How to Receive Money 02 P2P displayed, tap the Continue CTA → Select Account 01 P2P slides in from the right.

**Acceptance Scenarios**:

1. **Given** "Choose How to Receive Money 02 P2P" is displayed, **When** the presenter taps the Continue CTA area, **Then** "Select Account 01 P2P" slides in from the right using a 300 ms ease-in-out animation.
2. **Given** "Choose How to Receive Money 02 P2P" is displayed, **When** the presenter taps anywhere outside the Continue CTA area, **Then** no screen transition occurs.

---

### User Story 3 – Radio Button on Select Account 01 Swaps to Select Account 02 (Priority: P1)

With "Select Account 01 P2P" on screen, the presenter taps one of the two radio button areas. The display immediately swaps to "Select Account 02 P2P" with no slide animation.

**Why this priority**: Same pattern as User Story 1 — mirrors the radio button selection UX established for Choose How to Receive Money.

**Independent Test**: With Select Account 01 P2P displayed, tap a radio button area → the screen instantly shows Select Account 02 P2P with no animation.

**Acceptance Scenarios**:

1. **Given** "Select Account 01 P2P" is displayed, **When** the presenter taps the first radio button area, **Then** "Select Account 02 P2P" appears immediately (no slide transition).
2. **Given** "Select Account 01 P2P" is displayed, **When** the presenter taps the second radio button area, **Then** "Select Account 02 P2P" appears immediately (no slide transition).
3. **Given** "Select Account 01 P2P" is displayed, **When** the presenter taps anywhere outside the radio button zones, **Then** no screen transition occurs.

---

### User Story 4 – Continue CTA on Select Account 02 Triggers Fade + Spinner (Priority: P1)

With "Select Account 02 P2P" on screen, the presenter taps the "Continue" call-to-action area. The current screen immediately begins fading to a dark/black state. While the image is fading, a centered spinning loading indicator appears overlaid on the phone screen. The screen remains faded with the spinner visible for 1 second, then the "Congratulations P2P" screen replaces the faded state.

**Why this priority**: This is the climactic action of the enrollment demo — the "processing" moment before completion.

**Independent Test**: With Select Account 02 P2P displayed, tap the Continue CTA → current screen fades, spinner appears for 1 second.

**Acceptance Scenarios**:

1. **Given** "Select Account 02 P2P" is displayed, **When** the presenter taps the Continue CTA area, **Then** the screen fades out and a spinning indicator appears centered on the phone screen.
2. **Given** the spinner is active, **When** 1 second elapses, **Then** the spinner is removed and the "Congratulations P2P" screen is displayed.
3. **Given** the spinner/fade is in progress, **When** the presenter taps the phone screen, **Then** no additional transitions are triggered (all zones are inert during spinner).

---

### Edge Cases

- What happens if a presenter taps the radio buttons on 01 multiple times? (Each tap re-applies the instant swap to 02 — idempotent, no visual change since 02 is already shown after first tap.)
- What happens if the presenter taps outside defined CTA zones on 02 screens? (No transition — all non-CTA areas are inert.)
- What happens during the fade/spinner if the presenter selects a different use case from the radio panel? (The flow resets to blank immediately, spinner is cancelled — consistent with all other states.)
- What happens if the presenter selects a use case while on Congratulations P2P? (Phone resets to blank immediately — the radio panel always wins.)
- What happens if the presenter selects a use case while on Congratulations P2P? (Phone resets to blank immediately — the radio panel always wins.)

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: When "Choose How to Receive Money 01 P2P" is displayed, tapping either radio button zone MUST instantly replace the screen with "Choose How to Receive Money 02 P2P" with no animation.
- **FR-002**: When "Choose How to Receive Money 01 P2P" is displayed, tapping outside the radio button zones MUST produce no screen transition.
- **FR-003**: When "Choose How to Receive Money 02 P2P" is displayed, tapping the Continue CTA zone MUST slide "Select Account 01 P2P" in from the right using a 300 ms ease-in-out animation.
- **FR-004**: When "Choose How to Receive Money 02 P2P" is displayed, tapping outside the Continue CTA zone MUST produce no screen transition.
- **FR-005**: When "Select Account 01 P2P" is displayed, tapping either radio button zone MUST instantly replace the screen with "Select Account 02 P2P" with no animation.
- **FR-006**: When "Select Account 01 P2P" is displayed, tapping outside the radio button zones MUST produce no screen transition.
- **FR-007**: When "Select Account 02 P2P" is displayed, tapping the Continue CTA zone MUST begin fading out the current screen immediately while simultaneously displaying a centered spinning indicator overlaid on the phone screen.
- **FR-008**: The faded state with spinner MUST persist for exactly 1 second, after which the "Congratulations P2P" screen MUST appear (replacing the faded overlay). The fade is continuous — it does not complete and then restart; the screen stays dark throughout the spinner duration.
- **FR-013**: The "Congratulations P2P" screen is the terminal state for the Enroll P2P flow. All tap zones MUST remain disabled; the presenter reloads the page to restart.
- **FR-009**: All tap zones on the phone screen MUST be disabled while the fade or spinner is active.
- **FR-010**: All instant-swap transitions (FR-001, FR-005) MUST complete in under 50 ms (imperceptible to human eye — effectively immediate).
- **FR-011**: The Continue CTA slide animations (FR-003) MUST use the same 300 ms ease-in-out timing established in the prior flow.
- **FR-012**: Switching to a different use case from the radio panel at any point during this extended flow MUST immediately reset the phone screen to blank and cancel any in-progress animation or spinner.

### Key Entities

- **Choose-Receive State**: A sub-state within the Enroll P2P flow representing the "Choose How to Receive Money" step; has two variants: `choose-receive-01` (radio buttons active) and `choose-receive-02` (Continue active).
- **Select-Account State**: A sub-state representing the "Select Account" step; has two variants: `select-account-01` (radio buttons active) and `select-account-02` (Continue active).
- **Spinner State**: A transient `processing` state that overlays the phone screen for exactly 1 second before transitioning to the terminal screen.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: An instant swap (radio button tap) completes in under 50 ms — no perceptible delay or flash.
- **SC-002**: The Continue CTA slide from Choose How to Receive Money 02 to Select Account 01 completes in 300 ms (±50 ms).
- **SC-003**: The fade + spinner on Select Account 02 is visually smooth — the screen begins fading immediately on tap, the spinner is visible throughout the 1-second duration (no flicker), and Congratulations P2P appears cleanly at the end.
- **SC-004**: Tapping outside any defined CTA or radio zone during the extended flow produces zero unintended transitions in a standard demo run.
- **SC-005**: Switching use cases from the panel at any point in the flow resets to blank in under 100 ms with no visible artifacts.
- **SC-006**: The "Congratulations P2P" screen is fully inert — zero tap interactions produce any screen change after it appears.

## Assumptions

- All extended-flow image files are available as static PNGs in the `images/` directory: `Choose How to Receive Money 01 P2P.png`, `Choose How to Receive Money 02 P2P.png`, `Select Account 01 P2P.png`, `Select Account 02 P2P.png`.
- The radio button tap zones on "Choose How to Receive Money 01 P2P" and "Select Account 01 P2P" will be determined by visual inspection of the PNG files (same empirical approach used for GET STARTED and Accept & Continue in the prior flow).
- The Continue CTA tap zone on "Choose How to Receive Money 02 P2P" and "Select Account 02 P2P" will be determined by visual inspection.
- The spinner is a CSS-animated element rendered on top of the phone screen; no external library is used.
- Step 7 in the user description contains a typo: "display the Choose How to Receive Money **01** P2P image" is interpreted as "display the Choose How to Receive Money **02** P2P image" based on context.
- The post-spinner screen is "Congratulations P2P" (`Congratulations P2P.png`), available in the `images/` directory. It is the terminal state; no navigation away from it is provided in-demo.
