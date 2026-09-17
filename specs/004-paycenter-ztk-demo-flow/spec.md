# Feature Specification: PayCenter ZTK Demo – Enroll P2P Flow

**Feature Branch**: `004-paycenter-ztk-demo-flow`  
**Created**: 2026-05-18  
**Status**: Draft  
**Input**: User description: "Build a demo application of the PayCenter ZTK product showcasing the Enroll P2P use case with a carousel, Terms and Conditions screen, and post-acceptance flow."

## Clarifications

### Session 2026-05-18

- Q: After "Choose How to Receive Money 01 P2P" appears, what can the presenter do to reset or navigate from that point? → A: The screen is static (terminal state); the presenter reloads the page to restart.
- Q: What should the slide animation duration be? → A: 300 ms.
- Q: When the GET STARTED CTA overlaps a left/right nav zone, which takes priority? → A: GET STARTED CTA takes priority over nav zones.
- Q: Should rapid clicks during a slide animation be queued or dropped? → A: Dropped — taps during an active animation are ignored; only the next tap after the animation completes is registered.
- Q: What is the minimum expected viewport width for the demo? → A: 1280 px.



### User Story 1 – View Demo with Background and Phone Mockup (Priority: P1)

A demo viewer opens the application and sees a branded page with the JH background image filling the full viewport. A generic mobile phone frame is positioned on the right side of the screen with a black (empty) screen. A panel of use-case radio buttons is displayed to the left of the phone.

**Why this priority**: This is the entry point for every demo session; nothing else is usable without this working.

**Independent Test**: Can be tested by opening the application in a browser with no interaction — the background renders, the phone frame is visible on the right, and the use-case list is visible on the left.

**Acceptance Scenarios**:

1. **Given** the application loads, **When** no use case is selected, **Then** the phone screen is blank/black and no carousel images are shown.
2. **Given** the application loads, **When** the page is viewed at standard desktop resolution, **Then** the JH background image covers the full viewport and the phone + panel are correctly positioned.

---

### User Story 2 – Select Enroll P2P and Navigate Carousel (Priority: P1)

A demo presenter selects the "Enroll P2P" radio button. The phone screen immediately shows the first carousel image (001). The presenter can tap the right half of the phone screen to advance forward and the left half to go back, with a smooth sliding animation. After the last image (004), advancing wraps back to image 001.

**Why this priority**: This is the primary demo path and the core deliverable of this feature.

**Independent Test**: Select "Enroll P2P", then click right/left sides of the phone — images slide in/out correctly, wrapping at the ends.

**Acceptance Scenarios**:

1. **Given** "Enroll P2P" is selected, **When** the use case is first activated, **Then** Zelle P2P Enrollment Carousel Image 001 appears on the phone screen.
2. **Given** image 001 is displayed, **When** the user clicks the right side of the phone screen, **Then** image 001 slides left off-screen while image 002 slides in from the right.
3. **Given** image 002 is displayed, **When** the user clicks the left side of the phone screen, **Then** image 002 slides right off-screen while image 001 slides in from the left.
4. **Given** image 004 is displayed, **When** the user clicks the right side of the phone screen, **Then** image 004 slides left off-screen while image 001 slides in from the right (circular wrap).
5. **Given** image 001 is displayed, **When** the user clicks the left side of the phone screen, **Then** image 001 slides right off-screen while image 004 slides in from the left (circular wrap).

---

### User Story 3 – Trigger Terms and Conditions from Carousel (Priority: P1)

On any of the four carousel images (001–004), there is a "GET STARTED" call-to-action area. When the presenter taps anywhere within that CTA zone, the Terms and Conditions screen replaces the current carousel image on the phone screen.

**Why this priority**: The T&C gate is a required step in the enrollment flow and must be demonstrable.

**Independent Test**: With any carousel image displayed, click the GET STARTED area — the T&C screen appears.

**Acceptance Scenarios**:

1. **Given** any carousel image (001–004) is displayed, **When** the user clicks within the GET STARTED CTA area, **Then** the Terms and Conditions image is shown on the phone screen.
2. **Given** any carousel image is displayed, **When** the user clicks outside the GET STARTED CTA area, **Then** normal left/right navigation behavior applies (no unintended T&C transition).

---

### User Story 4 – Accept Terms and Continue to Next Step (Priority: P1)

On the Terms and Conditions screen, there is an "Accept & Continue" button. When the presenter taps it, the phone screen transitions to the "Choose How to Receive Money 01 P2P" image. Tapping anywhere else on the T&C screen does nothing.

**Why this priority**: This completes the minimum viable enrollment flow and is essential for the demo narrative.

**Independent Test**: Navigate to T&C, click Accept & Continue — the Choose How to Receive Money screen appears.

**Acceptance Scenarios**:

1. **Given** the Terms and Conditions screen is displayed, **When** the user clicks the Accept & Continue button, **Then** the "Choose How to Receive Money 01 P2P" image appears on the phone screen.
2. **Given** the Terms and Conditions screen is displayed, **When** the user clicks anywhere other than Accept & Continue, **Then** no screen transition occurs.

---

### User Story 5 – Select Other Use Cases (Priority: P2)

A demo viewer can see and select the radio buttons for "Enroll SMB", "Send P2P Payment", and "Send SMB Payment". These use cases are placeholders for future flows. Selecting them resets the phone screen to blank (black), indicating no active flow.

**Why this priority**: The UI must account for all four use cases even though only one is fully implemented in this iteration.

**Independent Test**: Click each non-P2P radio button — the phone screen clears to black with no images displayed.

**Acceptance Scenarios**:

1. **Given** "Enroll SMB" is selected, **When** the radio button is activated, **Then** the phone screen returns to blank/black.
2. **Given** "Send P2P Payment" or "Send SMB Payment" is selected, **When** the radio button is activated, **Then** the phone screen returns to blank/black.

---

### Edge Cases

- What happens if the GET STARTED CTA zone overlaps a left/right nav zone? (CTA takes priority; a tap anywhere within the CTA zone triggers the T&C screen, not a carousel slide.)
- What happens when the application loads with no use case pre-selected? (Phone screen remains black.)
- What happens if images fail to load? (Graceful fallback — phone screen stays black; no broken-image icons shown to the audience.)
- What happens if the GET STARTED CTA zone overlaps a left/right nav zone? (CTA takes priority; a tap anywhere within the CTA zone triggers the T&C screen, not a carousel slide.)
- What happens if the user rapidly clicks during a slide animation? (All taps during the 300 ms animation window are dropped; no queuing occurs. The next tap registered after animation completion is acted upon.)

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The application MUST display the JH background image scaled to cover the full viewport.
- **FR-002**: The application MUST render a generic mobile phone frame positioned on the right side of the viewport, visually offset from the background text.
- **FR-003**: The application MUST display four use-case options with radio buttons: Enroll P2P, Enroll SMB, Send P2P Payment, Send SMB Payment.
- **FR-004**: The phone screen MUST be blank (black) when no use case is selected.
- **FR-005**: Selecting "Enroll P2P" MUST populate the phone screen with Zelle P2P Enrollment Carousel Image 001.
- **FR-006**: Tapping the right half of the phone screen MUST advance to the next carousel image using a left-sliding animation.
- **FR-007**: Tapping the left half of the phone screen MUST go back to the previous carousel image using a right-sliding animation.
- **FR-008**: Carousel navigation MUST wrap circularly: advancing past image 004 returns to 001, and going back from 001 wraps to 004.
- **FR-009**: Each carousel image (001–004) MUST contain a defined “GET STARTED” CTA tap zone; tapping within that zone MUST display the Terms and Conditions image. The CTA tap zone MUST take priority over the left/right navigation zones — if a tap falls within the CTA zone, it triggers T&C regardless of which nav half it occupies.
- **FR-010**: On the Terms and Conditions screen, only tapping the Accept & Continue button MUST cause a screen transition; all other tap areas MUST be inert.
- **FR-011**: Tapping Accept & Continue on the Terms and Conditions screen MUST display the "Choose How to Receive Money 01 P2P" image on the phone screen.
- **FR-012**: Slide animations MUST complete in 300 ms using an ease-in-out curve. While an animation is in progress, all tap input on the phone screen MUST be ignored (clicks are dropped, not queued). A new transition only begins after the current one fully completes.
- **FR-013**: The application MUST run in a modern web browser with no external dependencies or build tools required.

### Key Entities

- **Use Case**: A named demo scenario (Enroll P2P, Enroll SMB, Send P2P Payment, Send SMB Payment) that maps to a sequence of phone screen images.
- **Screen**: An image displayed inside the phone frame, representing a step in the selected use-case flow.
- **Carousel**: The ordered, circular sequence of screens (images 001–004) navigable via left/right tap zones.
- **CTA Zone**: A defined tap-sensitive area overlaid on a screen image that triggers a navigation event.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Each carousel slide transition completes in 300 ms (±50 ms); a presenter can navigate all four images forward and backward without visual glitches in under 10 seconds.
- **SC-002**: The full Enroll P2P flow — carousel navigation → GET STARTED → T&C → Accept & Continue — can be completed end-to-end in under 30 seconds by a first-time user.
- **SC-003**: All four use-case radio buttons are visually distinct and selectable; selecting "Enroll P2P" activates the correct flow 100% of the time.
- **SC-004**: Tapping outside the defined GET STARTED zone does NOT accidentally trigger the T&C screen (zero false positives during a standard demo run).
- **SC-005**: Tapping anywhere on the T&C screen other than Accept & Continue does NOT cause any screen change (zero unintended transitions).
- **SC-006**: The demo loads and is fully operable in under 3 seconds on a standard broadband connection.
- **SC-007**: The layout renders correctly (phone frame and radio-button panel visible side-by-side with no overflow or clipping) at a minimum viewport width of 1280 px.

## Assumptions

- All image assets (carousel images, T&C screen, Choose How to Receive Money screen, background) are available as static PNG files in the `images/` directory relative to the HTML file.
- The demo targets desktop browsers at a minimum viewport width of 1280 px (conference-room projector/laptop context); mobile browser and touch input are out of primary scope but should not break.
- No back-end, authentication, or persistent state is required; the entire demo runs from static files in the browser.
- The GET STARTED CTA tap zone coordinates for each carousel image will be determined by visual inspection of the image files (they are in the same relative position on all four images).
- The "Choose How to Receive Money 01 P2P" screen is the terminal state for the Enroll P2P flow; no further navigation from that screen is required or provided. The presenter reloads the page to restart the demo.
- Selecting Enroll SMB, Send P2P Payment, or Send SMB Payment returns the phone screen to blank/black; no images or flows are associated with these use cases in this iteration.
