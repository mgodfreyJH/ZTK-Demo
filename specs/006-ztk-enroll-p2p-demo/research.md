# Research: PayCenter ZTK Demo - Enroll P2P Flow (006)

**Phase**: 0 - Pre-Design Research
**Date**: 2026-07-30 (amended; original 2026-07-27)
**Branch**: `006-ztk-enroll-p2p-demo`

> Amendment note (2026-07-30): The spec now formally documents the previously-implemented Enroll SMB flow (User Story 4) and simplifies the use-case panel to exactly two options (P2P, SMB). User Story 3 (placeholder use cases) is retired. Sections 4 and 6 below are updated accordingly; all other research decisions are unchanged.

---

## 1. Implementation Stack

**Decision**: Use HTML5, CSS3, and ES2020 JavaScript with no external frameworks or build tooling.

**Rationale**: The requested approach is modern but simple web tooling. The feature is a static interactive demo, so native browser capabilities are sufficient and avoid operational overhead.

**Alternatives considered**:
- React/Vite app: rejected due to unnecessary complexity for a single static demo flow.
- TypeScript build pipeline: rejected to preserve zero-build execution and faster handoff.

---

## 2. State Management Strategy

**Decision**: Use a finite in-memory `flowState` with explicit transitions per screen and per hotspot event.

**Rationale**: Requirements are deterministic and linear for the canonical path. A compact state machine makes behavior testable and prevents invalid mid-animation transitions.

**Alternatives considered**:
- DOM-derived implicit state: rejected because it is harder to reason about and easier to break.
- URL/hash routing: rejected because the demo does not need deep linking.

---

## 3. Transition and Timing Policy

**Decision**:
- Carousel interactions use 300ms slide animations.
- Radio-selection steps transition immediately (no slide).
- Non-loading, non-carousel-animated transitions must begin within 250ms.
- Processing step from Select Account 02 to Congratulations shows a spinner for exactly 1 second before advancing.

**Rationale**: This aligns with clarified success criteria and preserves perceived responsiveness while keeping presentation polish.

**Alternatives considered**:
- Making all transitions instant: rejected because carousel animation behavior is explicitly required.
- Longer delay thresholds (>250ms): rejected due to weaker UX guarantees.

---

## 4. Use-Case Selection Behavior

**Decision**:
- The use-case panel offers exactly two options: **P2P** and **SMB** (the former "Send P2P Payment" / "Send SMB Payment" placeholders are removed).
- Selecting P2P always starts at P2P Carousel 001; selecting SMB always starts at SMB Carousel 001. Both discard any prior state.
- Switching use cases at any time discards current flow progress and resets the device screen to `JH50 Lock Screen.png`.

**Rationale**: This reflects the 2026-07-30 clarification session — both remaining use cases now have complete, fully-specified flows, so the lock-screen-reset behavior only applies transiently between selections, not as a permanent placeholder state.

**Alternatives considered**:
- Placeholder "Coming Soon" views: rejected because both remaining use cases are fully implemented.
- Retaining current screen on use-case switch: rejected due to unpredictable demo behavior.
- Keeping four use-case options: rejected per clarification to reduce the panel to only the flows that are fully built.

---

## 5. Hotspot Modeling

**Decision**: Model each tappable area as a dedicated positioned overlay element, disabled by default and enabled only for its active state.

**Rationale**: Keeps click behavior explicit, testable, and easy to calibrate against static PNG screens.

**Alternatives considered**:
- Pixel-map click detection on raw images: rejected due to complexity and maintainability cost.
- Single dynamic hotspot element reused across states: rejected because state-specific bounds are clearer with named hotspots.

---

## 6. Scope Boundaries

**Decision**:
- Keep feature fully offline and local-asset based.
- Enroll SMB is now a complete, fully-specified flow (22 canonical transitions from SMB Carousel 001 to Landing Page), including OTP authentication, Zelle tag creation with a name-unavailable retry path, and a 5-second alternating feature-intro rotation before converging into the shared contacts-permission tail.
- Keep landing page terminal until a use-case is reselected.

**Rationale**: All 18 SMB device-screen image assets are present, so the SMB flow that was already implemented in `index.html` is now documented and formalized in the spec rather than left as an undocumented placeholder.

**Alternatives considered**:
- Leaving SMB undocumented in spec.md while implemented in code: rejected because it caused spec/code drift (the exact gap this amendment fixes).
- Building a new SMB flow from scratch: rejected — the existing implementation already satisfies the acceptance scenarios; only documentation needed to catch up.

## 8. SMB Feature-Intro Rotation

**Decision**: After Congratulations SMB → ZRC Feature Intro, if no qualifying click occurs within 5 seconds, slide in Zelle Tag Feature Introduction SMB from the right; keep alternating every 5 seconds (±0.5s tolerance) until the presenter clicks "Access Contacts" (ZRC) or "Skip For Now" (Zelle Tag Intro), either of which cancels the rotation timer and navigates to the shared Allow Access to Contacts 01 screen.

**Rationale**: Clarified as a formal, testable requirement (FR-043/FR-044, SC-009) rather than a loose visual flourish, so it can be verified with a simple timer-based check.

**Alternatives considered**:
- Untimed/manual-only toggle: rejected because the spec explicitly requires automatic alternation.
- Single-shot auto-advance (no repeat): rejected because the spec requires indefinite alternation until a click occurs.

---

## 7. Contracts Requirement

**Decision**: Produce a UI interaction contract file under `contracts/` describing states, events, timing expectations, and required behaviors.

**Rationale**: Although there is no external API, this demo exposes a human-visible interaction surface that benefits from explicit contract documentation for implementation and verification.

**Alternatives considered**:
- No contracts directory: rejected for this planning cycle to improve testability and handoff clarity.
