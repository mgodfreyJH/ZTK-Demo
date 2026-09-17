# Feature Specification: PayCenter ZTK Demo Carousel

**Feature Branch**: `001-ztk-demo-carousel`  
**Created**: 2026-05-07  
**Status**: Draft  
**Input**: User description: "Build a PayCenter ZTK demo app displaying a generic mobile phone with a Zelle P2P Enrollment image carousel (4 images). Clicking the right side of the phone screen slides to the next image. After image 004, navigation wraps back to image 001."

## Clarifications

### Session 2026-05-07

- Q: How should the demo access the source images — copy into project with relative paths, serve via local server, or absolute file:// paths? → A: Copy images into the project folder and reference them with relative paths.
- Q: What is the intended delivery format of the demo — single HTML file (double-click to open) or a multi-file project requiring a dev server? → A: Single `.html` file opened by double-clicking; no server required.
- Q: How is "right side of the screen" defined — right half, narrow right edge zone, or a visible button? → A: Right 50% of the phone screen area (right half).
- Q: What should the slide transition duration be? → A: 350ms (standard mobile transition speed).
- Q: What background color should the page use behind the phone frame? → A: Dark charcoal/near-black background (`#1a1a2e` or similar).

## User Scenarios & Testing *(mandatory)*

### User Story 1 - View Demo on Load (Priority: P1)

A user opens the demo application and immediately sees a realistic-looking generic mobile phone displayed on screen. The phone's screen is populated with the first Zelle P2P Enrollment Carousel image, giving the impression of a real mobile enrollment experience.

**Why this priority**: This is the foundational state of the demo — without it, no other functionality is usable or meaningful.

**Independent Test**: Open the demo in a browser. Confirm a mobile phone frame is visible and its screen shows Zelle P2P Enrollment Carousel Image 001.

**Acceptance Scenarios**:

1. **Given** the demo application page is opened, **When** the page finishes loading, **Then** a generic mobile phone frame is displayed centered on screen.
2. **Given** the demo application page is opened, **When** the page finishes loading, **Then** the phone screen displays Zelle P2P Enrollment Carousel Image 001.
3. **Given** the demo application page is opened, **When** the page finishes loading, **Then** no navigation controls other than the clickable screen area are visible.

---

### User Story 2 - Navigate to Next Enrollment Screen (Priority: P2)

A user clicks the right side of the phone screen and watches the current enrollment screen slide smoothly off to the left while the next enrollment screen slides in from the right — mimicking a natural mobile swipe gesture.

**Why this priority**: Navigation is the core interactive behavior of this demo. It demonstrates the enrollment flow to stakeholders.

**Independent Test**: With Image 001 displayed, click the right side of the phone screen. Confirm Image 002 slides in from the right while Image 001 exits to the left.

**Acceptance Scenarios**:

1. **Given** Image 001 is displayed on the phone screen, **When** the user clicks the right side of the screen, **Then** Image 001 slides out to the left and Image 002 slides in from the right.
2. **Given** Image 002 is displayed on the phone screen, **When** the user clicks the right side of the screen, **Then** Image 002 slides out to the left and Image 003 slides in from the right.
3. **Given** Image 003 is displayed on the phone screen, **When** the user clicks the right side of the screen, **Then** Image 003 slides out to the left and Image 004 slides in from the right.
4. **Given** an image is actively mid-transition, **When** the user clicks again, **Then** the click is ignored until the current transition completes.

---

### User Story 3 - Circular Wrap-Around Navigation (Priority: P3)

A user has navigated through all four enrollment screens and clicks the right side again. Instead of stopping, the carousel wraps back to Image 001, allowing the demo to loop indefinitely for presentation purposes.

**Why this priority**: This ensures the demo can run continuously during presentations without requiring a manual reset.

**Independent Test**: Navigate forward through all 4 images. On Image 004, click the right side. Confirm Image 001 slides in from the right.

**Acceptance Scenarios**:

1. **Given** Image 004 is displayed on the phone screen, **When** the user clicks the right side of the screen, **Then** Image 004 slides out to the left and Image 001 slides in from the right.
2. **Given** the demo has looped back to Image 001, **When** the user continues clicking the right side, **Then** images advance in order (001 → 002 → 003 → 004) as normal.

---

### Edge Cases

- What happens when an image file fails to load? The phone screen should show a placeholder and not crash the demo.
- What happens when the user clicks rapidly multiple times in succession? Each click during an active animation is ignored; only one transition occurs at a time.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The demo MUST display a generic front-facing mobile phone frame as the primary visual element.
- **FR-002**: The phone screen MUST display Zelle P2P Enrollment Carousel Image 001 upon the demo loading.
- **FR-003**: Clicking anywhere within the right 50% of the phone screen area MUST advance to the next image in sequence. No visible button or arrow is required — the click zone is invisible.
- **FR-004**: Image transitions MUST be animated: the current image slides out to the left while the next image slides in from the right simultaneously.
- **FR-005**: The carousel MUST follow the order: Image 001 → 002 → 003 → 004 → 001 (circular).
- **FR-006**: The demo MUST display all four Zelle P2P Enrollment Carousel images (001, 002, 003, 004) and make them accessible via right-side navigation.
- **FR-007**: The demo MUST prevent additional navigation clicks from being registered while a slide transition is in progress.
- **FR-008**: The four source images MUST be stored in an `images/` subfolder alongside the HTML file and referenced using relative paths (not absolute file system paths).
- **FR-009**: The deliverable MUST be a single `.html` file that opens correctly by double-clicking in Windows Explorer, with no local server or build step required.
- **FR-010**: Each slide transition MUST complete in exactly 350ms using a smooth easing curve.
- **FR-011**: The page background behind the phone frame MUST be a dark charcoal/near-black color (`#1a1a2e` or visually equivalent) to provide high contrast for the phone frame and screen imagery.

### Key Entities

- **Mobile Phone Frame**: The decorative generic phone outline that wraps the carousel screen. Not tied to any specific brand.
- **Carousel Screen**: The display area inside the phone frame where enrollment images appear.
- **Enrollment Image**: One of four Zelle P2P Enrollment Carousel images (001–004) loaded from the local images directory.
- **Slide Transition**: The animated movement (current image exits left, next image enters right) triggered by a right-side screen click.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: The demo renders completely and is fully interactive within 3 seconds of being opened on a standard desktop machine.
- **SC-002**: All four enrollment images are reachable by clicking the right side of the screen sequentially, with no images skipped.
- **SC-003**: Each slide transition completes in 350ms with a smooth easing curve — no visible flicker, image overlap, or blank-screen gaps.
- **SC-004**: After image 004, one additional click returns to image 001 with the same smooth slide-in animation — confirmed across at least 3 consecutive full loops.
- **SC-005**: Rapid clicking does not cause the carousel to display images out of sequence or skip images.

## Assumptions

- The four source images will be copied from `c:\users\migodfrey\ZTK Images\` into an `images/` subfolder in the project, and referenced with relative paths.
- The deliverable is a single `.html` file; images live in a sibling `images/` folder. No server or build tool is needed.
- The demo runs in a modern desktop web browser (no mobile browser optimization required for v1).
- Only forward (right-side click) navigation is required; no back/left navigation is in scope.
- The mobile phone frame is a generic, non-branded decorative element — no specific device model needs to be replicated.
- The demo is a standalone, self-contained deliverable with no backend, authentication, or user data requirements.
- Image aspect ratios will be preserved within the phone screen area; no cropping logic is required.
- The page background color is dark charcoal (`#1a1a2e` or visually equivalent); no theming toggle is required.
