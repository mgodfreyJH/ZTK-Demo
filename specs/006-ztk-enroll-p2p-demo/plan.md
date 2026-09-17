# Implementation Plan: PayCenter ZTK Demo — Enroll P2P Flow

**Branch**: `006-ztk-enroll-p2p-demo` | **Date**: 2026-07-30 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/006-ztk-enroll-p2p-demo/spec.md`

**Note**: This template is filled in by the `/speckit.plan` command. See `.specify/templates/plan-template.md` for the execution workflow.

## Summary

Deliver a single-page, click-through PayCenter ZTK demo that lets a presenter pick a use case (**P2P** or **SMB**) and walk a stakeholder through the corresponding enrollment flow using simple, modern, dependency-free web tools. The device mockup renders static PNG screens with positioned hotspot overlays; a small in-memory finite-state machine drives navigation, carousel animation, radio-driven instant transitions, a 1-second loading gate, and (for SMB) a 5-second alternating feature-intro rotation, converging both flows into a shared contacts-permission tail that ends at Landing Page. No build step, framework, backend, or network dependency is introduced — this keeps the demo trivially portable (open `index.html` in a browser) while remaining aligned with the amended spec that now documents both the P2P and previously-undocumented SMB flows and simplifies the use-case panel to exactly two options.

## Technical Context

**Language/Version**: HTML5, CSS3, JavaScript ES2020 (native browser — no transpilation, no bundler)
**Primary Dependencies**: None (zero third-party runtime or build-time dependencies)
**Storage**: N/A — all state (`activeUseCase`, `flowState`, `currentCarouselIndex`, `isAnimating`, timer handles) lives in memory and resets on page reload
**Testing**: Manual browser verification against the scenarios in [quickstart.md](quickstart.md); no automated test framework requested for this static demo
**Target Platform**: Desktop browsers (Chrome, Edge, Firefox, Safari) opening `ZTK Demo Project/index.html` as a local file — no server or installation
**Project Type**: Single static web page (interactive demo), not a client/server or mobile application
**Performance Goals**: Non-loading/non-carousel transitions begin within 250ms of click (SC-002); carousel slides complete in 300ms ±50ms (SC-003); loading spinner shows for exactly 1s (SC-004); SMB feature-intro rotation alternates every 5s ±0.5s (SC-009)
**Constraints**: Fully offline-capable, zero installation/plugins/network (SC-006); must run as a plain local HTML file; existing calibrated hotspot bounds in `index.html` must not regress
**Scale/Scope**: 1 HTML page, 1 background asset + 38 device-screen PNG assets (18 P2P + 20 SMB), 2 use-case flows — P2P (19-transition canonical path, including the shared Authenticate OTP 01/02 SMB steps) and SMB (24-transition canonical path, including the Authenticate OTP 03/04 re-authentication steps)

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

`.specify/memory/constitution.md` is still the unfilled bootstrap template (placeholder tokens like `[PRINCIPLE_1_NAME]`, no ratified version) — no project-specific principles are defined yet, so there are no concrete gates to violate. Applying general simplicity/YAGNI defaults in the absence of a ratified constitution:

- **Simplicity**: PASS — no framework, build tool, or server is introduced; the plan keeps the existing zero-dependency HTML/CSS/JS approach.
- **No unnecessary abstraction**: PASS — a single finite-state machine and named hotspot overlays are used instead of a routing library or component framework.
- **Testability**: PASS — manual quickstart scenarios map 1:1 to acceptance scenarios and success criteria; a UI interaction contract documents observable behavior for verification.

No violations to record in Complexity Tracking.

## Project Structure

### Documentation (this feature)

```text
specs/006-ztk-enroll-p2p-demo/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── data-model.md        # Phase 1 output (/speckit.plan command)
├── quickstart.md        # Phase 1 output (/speckit.plan command)
├── contracts/
│   └── ui-interaction-contract.md   # Phase 1 output (/speckit.plan command)
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)

```text
# Single project (static web demo) — the only structure used by this feature

ZTK Demo Project/
├── index.html           # Demo application: layout, use-case panel, device mockup,
│                         #   state machine, hotspot handlers (P2P + SMB flows)
├── calibrate.html        # Standalone tool for calibrating hotspot bounds against
│                         #   static screen images
├── screen-flow.md        # Screen flow tables (P2P + SMB) — documentation only,
│                         #   not rendered on the demo page
├── flow-diagram.md        # Mermaid diagrams (P2P + SMB) — documentation only,
│                         #   not rendered on the demo page
├── prompts.md            # Working notes / prompt history for this project
└── images/               # All 37 required PNG assets (background + P2P + SMB screens)
```

**Structure Decision**: Single static-page project. All runtime logic and markup live in `ZTK Demo Project/index.html`; there is no `src/`, `backend/`, or `frontend/` split because there is no server, build pipeline, or compiled artifact — the browser loads the HTML/CSS/JS file directly from disk. Documentation-only flow references (`screen-flow.md`, `flow-diagram.md`) stay outside `index.html` per FR-023/FR-024.

## Complexity Tracking

> No Constitution Check violations were identified; this section is intentionally empty.
