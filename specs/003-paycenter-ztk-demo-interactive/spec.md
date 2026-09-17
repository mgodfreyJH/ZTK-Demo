# Feature Specification: PayCenter ZTK Interactive Demo

**Feature Branch**: `003-paycenter-ztk-demo-interactive`  
**Created**: 2026-05-15  
**Status**: Draft  
**Input**: User description: "Build a demo application of the PayCenter ZTK product displaying a mobile device UI with a Zelle P2P Enrollment Carousel (images 001–004) that slides left/right on user clicks, wraps around at the ends, and transitions to a Terms and Conditions screen when the user taps the GET STARTED call-to-action."

## Clarifications

### Session 2026-05-15

- Q: After the user views the Terms and Conditions screen, what should they be able to do? → A: ~~Tapping anywhere returns to Screen 001~~ **Superseded** — Only the Accept & Continue button advances to Choose How to Receive Money P2P; tapping anywhere else does nothing.
- Q: How should the boundary between the left tap zone and the right tap zone be defined on the phone screen? → A: 50/50 vertical split — left half navigates back, right half navigates forward.
- Q: Should the demo display any visual navigation indicators (dot indicators, arrows)? → A: No indicators — tap zones only, no overlaid UI elements.
- Q: How should the GET STARTED tap region be defined for each carousel image? → A: A bottom percentage of the phone screen area (e.g., bottom 20%) acts as the GET STARTED region, taking priority over left/right navigation.
- Q: What should the slide transition duration feel like? → A: 300ms.

### Session 2026-05-15 (addendum)

- Q: After the Choose How to Receive Money P2P screen is displayed, what should happen when the user interacts with it? → A: Terminal state — the screen is static; no taps do anything. Reload the page to restart.
- Q: How should the Accept & Continue tap region be defined on the Terms and Conditions screen? → A: Hardcoded pixel coordinates determined by visual inspection of `Terms and Conditions.png` during implementation.
- Q: Should the outdated T&C clarification be corrected? → A: Yes — superseded note updated in place.
- Q: What transition should Accept & Continue use to show the Choose screen? → A: Same 300ms ease-in-out slide from the right as carousel navigation.
- Q: Should the asset inventory be updated to list `Choose How to Receive Money P2P.png` as in-scope? → A: Yes.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - View Enrollment Carousel (Priority: P1)

A prospective Zelle user opens the demo and sees the first screen of the Zelle P2P Enrollment onboarding flow displayed inside a realistic mobile phone frame. They can scroll through all four carousel screens by tapping the right or left side of the phone screen.

**Why this priority**: The carousel is the core deliverable. Without it, no other interaction is possible. It is the entry point for all subsequent flows.

**Independent Test**: Open the demo in a browser. Verify the phone frame is visible and Screen 001 is displayed. Tap right to advance through Screens 002, 003, 004, and then back to 001 (wrap-around). Tap left to go backwards with the same wrap-around behavior. Each transition must animate the outgoing screen sliding off in the tapped direction while the incoming screen slides in from the opposite side.

**Acceptance Scenarios**:

1. **Given** the demo is loaded, **When** the user views the page, **Then** a mobile phone frame is displayed with Zelle P2P Enrollment Carousel Image 001 visible on its screen.
2. **Given** Screen 001 is displayed, **When** the user taps the right side of the screen, **Then** Screen 001 slides off to the left and Screen 002 slides in from the right.
3. **Given** Screen 002 is displayed, **When** the user taps the right side of the screen, **Then** Screen 002 slides off to the left and Screen 003 slides in from the right.
4. **Given** Screen 003 is displayed, **When** the user taps the right side of the screen, **Then** Screen 003 slides off to the left and Screen 004 slides in from the right.
5. **Given** Screen 004 is displayed, **When** the user taps the right side of the screen, **Then** Screen 004 slides off to the left and Screen 001 slides in from the right (wrap-around).
6. **Given** Screen 001 is displayed, **When** the user taps the left side of the screen, **Then** Screen 001 slides off to the right and Screen 004 slides in from the left (wrap-around).
7. **Given** any screen is displayed, **When** the user taps the left side of the screen, **Then** the current screen slides off to the right and the previous screen slides in from the left.

---

### User Story 2 - Access Terms and Conditions (Priority: P2)

While viewing any of the four carousel screens, the user taps the "GET STARTED" call-to-action area. The demo responds by displaying the Terms and Conditions screen inside the phone frame, replacing the carousel.

**Why this priority**: The Terms and Conditions transition is a key demo interaction showing the next step in the enrollment flow. It must be reachable from all carousel screens.

**Independent Test**: Starting from any of the four carousel screens, tap the GET STARTED area. Verify the Terms and Conditions image is displayed in the phone frame.

**Acceptance Scenarios**:

1. **Given** any carousel screen (001–004) is displayed, **When** the user taps anywhere within the GET STARTED call-to-action area, **Then** the Terms and Conditions screen is displayed in the phone frame.
2. **Given** the GET STARTED area is tapped, **When** the transition occurs, **Then** the Terms and Conditions image completely replaces the carousel image in the phone screen area.

---

### User Story 3 - Accept Terms and Conditions (Priority: P3)

Having reached the Terms and Conditions screen, the user taps the Accept & Continue button to advance to the Choose How to Receive Money P2P screen. Tapping anywhere else on the T&C screen does nothing.

**Why this priority**: Enables the demo to show the next meaningful step in the enrollment flow after the user accepts terms.

**Independent Test**: Navigate to the T&C screen via GET STARTED, then tap the Accept & Continue button region. Verify the Choose How to Receive Money P2P screen is displayed. Verify that tapping outside the button area does nothing.

**Acceptance Scenarios**:

1. **Given** the Terms and Conditions screen is displayed, **When** the user taps the Accept & Continue button area, **Then** the Choose How to Receive Money P2P screen slides in from the right.
2. **Given** the Terms and Conditions screen is displayed, **When** the user taps anywhere outside the Accept & Continue button area, **Then** nothing happens.

---

### User Story 4 - View Choose How to Receive Money (Priority: P4)

After accepting terms, the user sees the Choose How to Receive Money P2P screen. This is the terminal state of the demo flow; no further interactions are defined.

**Why this priority**: Completes the demo narrative. The screen is reached only after traversing the full enrollment flow.

**Independent Test**: Navigate through carousel → GET STARTED → Accept & Continue. Verify Choose How to Receive Money P2P screen is displayed and tapping anywhere does nothing.

**Acceptance Scenarios**:

1. **Given** the Choose How to Receive Money P2P screen is displayed, **When** the user taps anywhere on the screen, **Then** nothing happens.

---

### Edge Cases

- After entering the T&C screen, the only actionable interaction is tapping Accept & Continue. All other taps are no-ops.
- The Choose How to Receive Money P2P screen is a terminal state. The demo must be reloaded (page refresh) to restart the flow.
- How does the carousel behave when transitioning very rapidly between screens? The animation for any in-progress transition should complete or be interrupted cleanly before the next tap is honored.
- What does the demo display if an image fails to load? A neutral fallback (blank/grey area) is acceptable.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The demo MUST display a stylized front-view representation of a generic mobile phone as the primary visual element.
- **FR-002**: On load, the phone screen MUST display Zelle P2P Enrollment Carousel Image 001.
- **FR-003**: The phone screen area MUST be divided into a left tap zone (left 50%) and a right tap zone (right 50%) for navigation. A tap within the GET STARTED region takes priority over zone navigation regardless of horizontal position.
- **FR-004**: When the user taps the right tap zone, the current carousel image MUST animate sliding off to the left while the next image in sequence animates sliding in from the right.
- **FR-005**: When the user taps the left tap zone, the current carousel image MUST animate sliding off to the right while the previous image in sequence animates sliding in from the left.
- **FR-006**: The carousel sequence MUST be: Image 001 → 002 → 003 → 004 → 001 (circular, wrapping at both ends).
- **FR-007**: Each of the four carousel images (001–004) MUST treat the bottom 20% of the phone screen area as the GET STARTED tap region. A tap anywhere within this region MUST trigger the Terms and Conditions transition, taking priority over left/right zone navigation.
- **FR-008**: Tapping the GET STARTED area on any carousel screen MUST replace the current carousel image with the Terms and Conditions image in the phone screen.
- **FR-009**: The demo MUST use the provided image assets: `Zelle P2P Enrollment Carousel Image 001–004.png`, `Terms and Conditions.png`, and `Choose How to Receive Money P2P.png`.
- **FR-010**: The slide transition animation MUST complete in 300ms using an ease-in-out curve and MUST be visually consistent across all transitions. New taps MUST be ignored while a transition is in progress.
- **FR-011**: The Terms and Conditions screen MUST respond only to taps within the Accept & Continue button region (coordinates determined by visual inspection of `Terms and Conditions.png` at 828×1792px). All other taps on the T&C screen MUST be ignored.
- **FR-012**: The demo MUST NOT display dot indicators, arrow overlays, or any other navigation UI elements on top of the phone screen content.
- **FR-013**: Tapping the Accept & Continue button on the Terms and Conditions screen MUST replace it with the Choose How to Receive Money P2P screen using a 300ms slide-in transition.
- **FR-014**: The Choose How to Receive Money P2P screen MUST be a terminal state. No tap on it produces any response.

### Key Entities

- **Carousel Screen**: One of the four Zelle P2P Enrollment images (001–004) displayed sequentially in the phone frame. Has a defined sequence order and an associated GET STARTED tap region.
- **Phone Frame**: The visual container representing a mobile device. Holds the currently active screen image.
- **Terms and Conditions Screen**: An intermediate screen displayed when GET STARTED is tapped. Represented by `Terms and Conditions.png`. Only the Accept & Continue button region is interactive.
- **Choose How to Receive Money Screen**: The terminal destination screen displayed after accepting Terms and Conditions. Represented by `Choose How to Receive Money P2P.png`. No interactions defined.
- **Tap Zone**: A region of the phone screen the user interacts with (left zone, right zone, GET STARTED area, or Accept & Continue area) to trigger a screen transition.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: All four carousel screens are reachable via right-tap navigation from Screen 001 in four taps or fewer, and wrap back to Screen 001 on the fifth tap.
- **SC-002**: All four carousel screens are reachable via left-tap navigation from Screen 001 in four taps or fewer with correct wrap-around behavior.
- **SC-003**: Tapping GET STARTED on each of the four carousel screens (100% of carousel screens) successfully displays the Terms and Conditions screen.
- **SC-004**: Each slide transition animation completes in 300ms without visible flickering, image overlap, or layout shift.
- **SC-005**: The demo loads and displays correctly in a standard desktop web browser without any additional installation steps.
- **SC-006**: Tapping the Accept & Continue button on the T&C screen displays the Choose How to Receive Money P2P screen.
- **SC-007**: The Choose How to Receive Money P2P screen is reached only via Accept & Continue and produces no response to any tap.

## Assumptions

- The demo is a single-page, self-contained browser application requiring no server-side components or authentication.
- The required image assets (seven PNG files in `ZTK Demo Project/images/`) are: `Zelle P2P Enrollment Carousel Image 001–004.png`, `Terms and Conditions.png`, and `Choose How to Receive Money P2P.png`. These are the authoritative source images; no additional images will be produced.
- The GET STARTED tap region is defined as the bottom 20% of the phone screen area, consistent across all four carousel images. This approximates the visible GET STARTED button position in each PNG asset and takes priority over left/right navigation zones.
- On the Terms and Conditions screen, only the Accept & Continue button region is active. Tapping elsewhere does nothing. The `Terms and Conditions.png` asset is 828×1792px; the Accept & Continue button coordinates will be determined by visual inspection during implementation.
- The demo is intended for desktop browser presentation, not native mobile installation. The phone frame is a visual representation only.
- No dot indicators, progress bars, or arrow overlays will be displayed on the phone screen; navigation is entirely driven by left/right tap zones.
- Rapid successive taps need not queue transitions; it is acceptable to prevent new taps while a transition animation is in progress.
- The Choose How to Receive Money P2P screen is a terminal state. The demo presenter must reload the browser page to restart the flow for a repeat showing.
- The `Zelle Access Contacts Permission.png` image present in the images folder is out of scope for this feature and will not be used in this demo.
