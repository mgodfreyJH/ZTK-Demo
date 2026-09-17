# Research: PayCenter ZTK Demo – Enroll P2P Flow

**Phase**: 0 — Pre-Design Research  
**Date**: 2026-05-18  
**Branch**: `004-paycenter-ztk-demo-flow`

---

## 1. Technology Stack

**Decision**: HTML5 + CSS3 + ES2020 vanilla JavaScript. Single `index.html` file with embedded `<style>` and `<script>` tags.

**Rationale**: The user explicitly stated "simple, modern web tools." No transpilation, no bundler, no external dependencies. This matches the existing codebase convention (all three prior features use the same stack). The demo is a static file delivered from a file system, not a server — dependencies would require a build step, defeating the purpose.

**Alternatives Considered**:
- React/Vue: Rejected — introduces build tools and dependencies for a single-page demo.
- Separate CSS/JS files: Viable but unnecessary for a single-page demo; embedded approach keeps deployment a single file drag-and-drop.

---

## 2. Existing Code Audit

**Decision**: Extend the existing `index.html` rather than rewrite it.

**Rationale**: A prior implementation pass already built and verified:
- Phone frame (290×684 px, border radius, notch, home bar)
- GPU-composited background using `#bg` fixed-position element
- Carousel slide mechanism with double-rAF paint-sync pattern
- 300 ms ease-in-out transitions with `isAnimating` guard (drop behavior)
- CTA hotspot at `top: 80% / height: 20%` (empirically positioned to cover GET STARTED area)
- Accept & Continue hotspot at `top: 93% / height: 6%` (measured from T&C image: y=1672–1766 in 1792px source)
- `showTerms()`, `showChooseScreen()`, `advance()`, `retreat()` functions all operational

**Gaps identified** (delta to implement):
1. No radio button panel — phone screen loads directly with carousel image 001
2. No blank initial state — screen shows image on load, not on use-case selection
3. No use-case selection logic — non-P2P selections not wired
4. Bug: `CHOOSE_IMAGE` path is `'images/Choose How to Receive Money P2P.png'` but actual file is `'images/Choose How to Receive Money 01 P2P.png'`

**Alternatives Considered**:
- Full rewrite: Rejected — existing animation engine is correct and tested.

---

## 3. Layout Strategy for Radio Button Panel

**Decision**: Wrap the phone frame in a `<div class="demo-wrapper">` flex container with a `<aside class="use-case-panel">` placed to its left. The wrapper inherits the existing body right-alignment.

**Rationale**: The body already uses `display: flex; justify-content: flex-end; padding-right: 27%` to position the content to the right. Wrapping the phone in a new flex row adds the panel without disturbing the overall positioning. The panel uses a semi-transparent dark card style that reads well over the JH background.

**Alternatives Considered**:
- Absolutely positioned panel: Rejected — fragile on resize, harder to keep aligned with phone.
- Separate column in body flex: Same result, cleaner with a wrapper.

---

## 4. CTA Hotspot Position

**Decision**: Retain existing position `top: 80% / height: 20%` confirmed from prior implementation.

**Rationale**: The prior implementation measured and set this position empirically against the actual PNG files. The GET STARTED button appears in the lower ~20% of all four carousel images. Since vision tooling is unavailable to re-measure, the existing value is accepted as ground truth.

**Risk**: If the button position varies significantly between images 001–004, the hotspot may miss on some images. Mitigation: the hotspot spans 100% width and 20% height so variance is absorbed.

---

## 5. State Machine Design

**Decision**: Use a flat JS state object `{ useCase, flowState, carouselIndex, isAnimating }` with explicit state-transition functions rather than a framework.

**Rationale**: The behavioral complexity is low (5 distinct states, ~6 transitions). A framework or class hierarchy would be over-engineering. Flat state with guard conditions is readable and directly testable by manual walkthrough.

**States**:
| `flowState` | Description |
|---|---|
| `'blank'` | No use case selected or non-P2P selected |
| `'carousel'` | Enroll P2P active, showing carousel image |
| `'terms'` | T&C screen active |
| `'choose-receive'` | Choose How to Receive Money screen (terminal) |

**Transitions**:
| From | Trigger | To |
|---|---|---|
| `blank` | Select "Enroll P2P" | `carousel` (index 0) |
| `carousel` | Select non-P2P use case | `blank` |
| `carousel` | Click right nav zone | `carousel` (index+1) |
| `carousel` | Click left nav zone | `carousel` (index-1) |
| `carousel` | Click CTA zone | `terms` |
| `terms` | Click Accept & Continue | `choose-receive` |
| `terms` | Click elsewhere | `terms` (no-op) |
| `choose-receive` | Any interaction | `choose-receive` (inert) |

---

## 6. No Build / No Tests

**Decision**: No automated tests. No lint/build step.

**Rationale**: The project `copilot-instructions.md` lists `npm test; npm run lint` but there are no `package.json` or test files in the project. For a single-file vanilla JS demo, manual browser testing is the verification method. Creating a test harness would require a build step, contradicting the zero-dependency constraint.
