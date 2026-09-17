# Feature Specification: PayCenter ZTK Demo — Zelle P2P Enrollment Carousel

**Feature Branch**: `002-paycenter-ztk-demo`  
**Created**: 2026-05-11  
**Status**: Draft  
**Input**: User description: "Display the front view of a generic mobile device populated with Zelle P2P Enrollment Carousel images. Users navigate forward and backward through 4 screens using left/right click interactions with smooth slide animations, with wrap-around from last to first screen and vice versa."

---

## User Scenarios & Testing *(mandatory)*

### User Story 1 — View Initial Enrollment Screen (Priority: P1)

A demo presenter opens the ZTK Demo application in a browser. They see a realistic front-view rendering of a generic mobile phone. The phone screen displays the first Zelle P2P Enrollment screen (image 001), giving the audience an immediate sense of the Zelle enrollment flow within a mobile-native context.

**Why this priority**: This is the entry point of the entire demo. Without it, no other story is possible. Delivering this alone constitutes a working, demonstrable product.

**Independent Test**: Can be fully tested by opening the application and confirming the mobile phone mockup displays image 001 in the screen area without any navigation.

**Acceptance Scenarios**:

1. **Given** the application is opened, **When** the page loads, **Then** a generic mobile phone front-view is displayed with a defined screen area.
2. **Given** the application has loaded, **When** no user action has occurred, **Then** the phone screen shows the Zelle P2P Enrollment Carousel image 001.
3. **Given** the application is open, **When** the page is viewed, **Then** the phone mockup and its screen area are visually distinct and proportional.

---

### User Story 2 — Navigate Forward Through Enrollment Screens (Priority: P1)

A demo presenter clicks the right side of the phone screen to advance the carousel. The current screen slides smoothly to the left while the next enrollment screen slides in from the right, mirroring a natural forward-swipe gesture on a real mobile device. Screens progress in order: 001 → 002 → 003 → 004.

**Why this priority**: Forward navigation through the enrollment flow is the primary demo interaction and must work for the demo to have any value.

**Independent Test**: Can be tested by clicking the right side of the phone screen multiple times and verifying each subsequent image appears with a left-slide animation.

**Acceptance Scenarios**:

1. **Given** image 001 is displayed, **When** the user clicks the right side of the screen, **Then** image 001 slides left out of view while image 002 slides in from the right.
2. **Given** image 002 is displayed, **When** the user clicks the right side, **Then** image 003 appears with the same left-slide animation.
3. **Given** image 003 is displayed, **When** the user clicks the right side, **Then** image 004 appears with a left-slide animation.
4. **Given** image 004 is displayed (last screen), **When** the user clicks the right side, **Then** the carousel wraps around and image 001 slides in from the right.

---

### User Story 3 — Navigate Backward Through Enrollment Screens (Priority: P2)

A demo presenter clicks the left side of the phone screen to step backward through the carousel. The current screen slides smoothly to the right while the previous enrollment screen slides in from the left, reflecting a backward-swipe gesture.

**Why this priority**: Backward navigation enhances the demo's flexibility but the core forward flow is more critical.

**Independent Test**: Can be tested independently by navigating to any screen other than 001, clicking the left side, and confirming the previous image appears with a right-slide animation.

**Acceptance Scenarios**:

1. **Given** image 002 is displayed, **When** the user clicks the left side of the screen, **Then** image 001 slides in from the left while image 002 slides out to the right.
2. **Given** image 004 is displayed, **When** the user clicks the left side, **Then** image 003 appears with a right-slide animation.
3. **Given** image 001 is displayed (first screen), **When** the user clicks the left side, **Then** the carousel wraps around and image 004 slides in from the left.

---

### Edge Cases

- What happens when the user clicks rapidly before the current animation completes?
- What happens if one or more image files fail to load?
- How does the layout behave on very small or very large browser windows?

---

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The application MUST display a front-view rendering of a generic mobile phone that visually simulates a real handheld device.
- **FR-002**: The phone rendering MUST contain a clearly defined screen area into which the carousel images are displayed.
- **FR-003**: On initial load, the phone screen MUST display Zelle P2P Enrollment Carousel image 001.
- **FR-004**: The phone screen area MUST be divided into a left interactive zone and a right interactive zone for navigation input. These zones MUST be invisible — no arrows, chevrons, or other visual affordances are shown; the cursor MUST change to a pointer over each zone to signal interactivity.
- **FR-005**: Clicking the right zone MUST trigger a forward transition: the current image exits to the left while the next image enters from the right.
- **FR-006**: Clicking the left zone MUST trigger a backward transition: the current image exits to the right while the previous image enters from the left.
- **FR-007**: The image sequence MUST follow the numeric order indicated by the image name suffix: 001 → 002 → 003 → 004.
- **FR-008**: After image 004, forward navigation MUST wrap around to image 001.
- **FR-009**: Before image 001, backward navigation MUST wrap around to image 004.
- **FR-010**: All image transitions MUST use a smooth, directional sliding animation that reinforces the direction of navigation. The animation duration MUST be 350ms.
- **FR-011**: During an active slide transition, all navigation clicks MUST be ignored (debounced) until the animation fully completes; navigation resumes normally once the transition ends.
- **FR-012**: The four carousel images MUST be stored in an `images/` subfolder relative to the application's HTML file and referenced using relative paths, making the application fully self-contained with no web server dependency.
- **FR-013**: On all four carousel screens (001–004), the GET STARTED CTA area MUST be an invisible clickable hotspot that triggers a forward slide transition to the Zelle Access Contacts Permission screen.
- **FR-014**: While the Contacts Permission screen is displayed, both the left navigation zone and the right navigation zone MUST be disabled (no navigation is possible from the permission screen until further requirements are defined).

### Key Entities

- **Enrollment Screen Image**: One of four numbered Zelle P2P Enrollment Carousel images (001–004) sourced from the local image directory.
- **Phone Mockup**: A front-view visual of a generic mobile device with a defined screen viewport area.
- **Carousel State**: The identity of the currently displayed image and the directional context for transitions.

---

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: All four enrollment screens are reachable and display correctly by clicking forward through the carousel.
- **SC-002**: All four enrollment screens are reachable and display correctly by clicking backward through the carousel.
- **SC-003**: Every screen transition completes with a visible, smooth directional slide animation.
- **SC-004**: Forward wrap-around (004 → 001) and backward wrap-around (001 → 004) work correctly without error.
- **SC-005**: The demo application loads and displays the initial screen within 3 seconds on a standard broadband connection.
- **SC-006**: The phone mockup and screen content are visually clear and correctly proportioned at typical laptop and desktop browser window sizes.

---

## Clarifications

### Session 2026-05-11

- Q: How should rapid consecutive clicks be handled during an active slide transition? → A: Ignore / debounce — clicks are silently discarded until the animation completes; navigation resumes after.
- Q: Should the left/right navigation zones have visible visual affordances (arrows, chevrons)? → A: Invisible zones — no visual indicators; cursor changes to pointer on hover to signal interactivity.
- Q: What should the slide animation duration be? → A: 350ms.
- Q: How should image files be referenced in the delivered application? → A: Images are copied into the project's `images/` folder and referenced via relative paths (self-contained, no server required).
- Q: After the Contacts Permission screen appears, what should happen when the user clicks the left (back) zone? → A: Do nothing — left zone is disabled while the permission screen is showing.
- Q: After the Contacts Permission screen appears, what should happen when the user clicks the right (forward) zone? → A: Do nothing — right zone is disabled while the permission screen is showing.
- Q: Is there a clickable element on the permission screen that continues the flow? → A: No — permission screen is a terminal state for now; further navigation will be specified after implementation is verified.
- Q: Now that implementation is verified, should the Allow/OK button on the permission screen be wired? → A: No — permission screen remains a terminal state; further requirements will be added in a future session.

---

## Assumptions

- The four Zelle P2P Enrollment Carousel images are named with numbered suffixes (001, 002, 003, 004). They are sourced from `c:\users\migodfrey\ZTK Images` and MUST be copied into the project's `images/` subfolder alongside `index.html` for relative-path browser delivery.
- The demo application is a single-page web application delivered in a modern browser with no backend requirement.
- No authentication, user accounts, or data persistence are needed — this is a purely presentational demo.
- The phone mockup is a purely visual element rendered in the browser; it does not simulate device interactions beyond left/right click zones.
- Exactly four carousel images exist; the sequence is fixed and does not need to be dynamically configurable.
- The demo targets desktop and laptop browsers at common resolutions; mobile browser support is not required for this demo.
