# Research: Enroll P2P Extended Flow – Choose, Select Account & Spinner

**Phase**: 0 — Pre-Design Research  
**Date**: 2026-05-19  
**Branch**: `005-enroll-p2p-extended-flow`

---

## 1. Technology Stack

**Decision**: HTML5 + CSS3 + ES2020 vanilla JavaScript. Same single `index.html` file as the prior feature (004).

**Rationale**: User specified "modern, simple web tools." Zero dependencies, zero build step. Fully consistent with all prior features in this project.

**Alternatives Considered**: None — the constraint is explicit and the existing code is already functional.

---

## 2. Existing Code Audit

**Current `flowState` values in `index.html`**:
- `'blank'` — phone screen is black, all zones disabled
- `'carousel'` — carousel images 001–004 navigable
- `'terms'` — T&C screen, accept hotspot active
- `'choose-receive'` — Choose How to Receive Money 01 P2P, ALL ZONES DISABLED

**Critical gap**: The 004 feature set `flowState = 'choose-receive'` but never added any hotspots or further navigation from that state. The phone screen shows the Choose How to Receive Money 01 P2P image as a static, inert terminal — the flow was not extended.

**Image path confirmed in 004**: `CHOOSE_IMAGE = 'images/Choose How to Receive Money 01 P2P.png'` — correct.

**New `flowState` values needed for this feature**:
| New State | Screen | Active Interaction |
|---|---|---|
| `'choose-receive-01'` | Choose How to Receive Money 01 P2P | Two radio button tap zones |
| `'choose-receive-02'` | Choose How to Receive Money 02 P2P | Continue CTA zone |
| `'select-account-01'` | Select Account 01 P2P | Two radio button tap zones |
| `'select-account-02'` | Select Account 02 P2P | Continue CTA zone |
| `'processing'` | Faded + spinner overlay | All zones inert (1-second timer) |
| `'congratulations'` | Congratulations P2P | All zones inert (terminal) |

**Rename required**: The 004 state `'choose-receive'` must be renamed to `'choose-receive-01'` to be consistent with the new extended state machine. The `showChooseScreen()` function sets this state — that line changes from `flowState = 'choose-receive'` to `flowState = 'choose-receive-01'`.

---

## 3. Tap Zone Strategy for New Screens

**Decision**: Add new hidden `<div>` hotspot elements inside `.phone-screen` for each new interaction zone. Follow the same pattern used for `#cta-hotspot` and `#accept-hotspot`.

**New hotspots needed**:
| ID | Screen | Position (empirical est.) | Z-index | Active in state |
|---|---|---|---|---|
| `#choose-radio-hotspot` | Choose How to Receive Money 01 | ~40–70% height, full width | 11 | `choose-receive-01` |
| `#choose-continue-hotspot` | Choose How to Receive Money 02 | ~88–96% height, full width | 11 | `choose-receive-02` |
| `#select-radio-hotspot` | Select Account 01 | ~45–75% height, full width | 11 | `select-account-01` |
| `#select-continue-hotspot` | Select Account 02 | ~88–96% height, full width | 11 | `select-account-02` |

**Note**: Exact `top`/`height` values for all four new hotspots must be calibrated against the actual PNG files. Estimates above are based on the pattern of UI elements typically appearing in the lower portion of these screens. The same `pointer-events: none` default + per-state activation pattern is used throughout.

**Alternatives Considered**: Using a single hotspot element with dynamically repositioned bounds — rejected as harder to reason about and debug than one dedicated div per interaction zone.

---

## 4. Instant Swap Pattern (Radio Button Screens)

**Decision**: For the two "01 → 02" instant swaps (Choose Receive and Select Account), replace `current.src` directly with no CSS transition and no animation frame scaffolding.

**Implementation**:
```js
function swapToChooseReceive02() {
  current.style.transition = 'none';
  current.src = 'images/Choose How to Receive Money 02 P2P.png';
  flowState = 'choose-receive-02';
  // disable radio hotspot, enable continue hotspot
}
```

**Rationale**: `< 50 ms` requirement is met by a direct `src` assignment — the browser repaints on the next vsync (~16 ms). Using the double-rAF + transition pattern would add ~32 ms overhead and visible motion, violating the "immediate" spec requirement.

---

## 5. Fade + Spinner Design

**Decision**: Use a CSS `opacity` transition on `#slide-current` combined with a CSS `@keyframes` spinner `<div>` that is appended to `.phone-screen` on demand.

**Implementation approach**:
1. Tap Continue on Select Account 02 → disable all tap zones, set `isAnimating = true`
2. Apply `current.style.transition = 'opacity 300ms ease-in'` and `current.style.opacity = '0'`
3. Simultaneously inject a `#spinner` overlay div into `.phone-screen` (CSS animated ring)
4. After 1000 ms total: remove spinner, reset opacity to 1 with `transition: none`, swap `current.src` to Congratulations image, set `flowState = 'congratulations'`

**Clarification encoding**: The fade is continuous — the image fades and stays faded while the spinner is visible. The Congratulations screen appears at the end, replacing the faded state. Total duration = 1 second from tap to Congratulations appearing.

**Spinner CSS**: `border-radius: 50%`, `border: 3px solid rgba(255,255,255,0.3)`, `border-top-color: #fff`, `animation: spin 0.7s linear infinite`. No external library.

**Alternatives Considered**:
- SVG spinner: More markup, same visual result. Rejected for simplicity.
- GIF spinner: External asset, adds file dependency. Rejected.

---

## 6. `resetToBlank()` Must Cancel Spinner

**Decision**: `resetToBlank()` must be extended to:
1. Clear any active `setTimeout` timer (store the timer ID in `spinnerTimer`)
2. Remove the `#spinner` element from the DOM if present
3. Reset `current.style.opacity = '1'` and `current.style.transition = 'none'`

This ensures selecting a use-case radio while the processing animation is running cleanly aborts it.

---

## 7. Image Preloading

**Decision**: Add new images to the existing preload array in `index.html`.

**New images to preload**:
- `images/Choose How to Receive Money 02 P2P.png`
- `images/Select Account 01 P2P.png`
- `images/Select Account 02 P2P.png`
- `images/Congratulations P2P.png`
