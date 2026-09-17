# Data Model: Enroll P2P Extended Flow – Choose, Select Account & Spinner

**Phase**: 1 — Design  
**Date**: 2026-05-19  
**Branch**: `005-enroll-p2p-extended-flow`

---

## Runtime State (extended from 004)

```js
// ── State machine ──
// flowState extended values (added by this feature):
// 'choose-receive-01' | 'choose-receive-02'
// 'select-account-01' | 'select-account-02'
// 'processing'        | 'congratulations'
//
// NOTE: '004' state 'choose-receive' is renamed to 'choose-receive-01'

let flowState    = 'blank';
let currentIndex = 0;
let isAnimating  = false;
let spinnerTimer = null;   // NEW — holds the setTimeout ID for the processing state
```

---

## Full State Machine (004 + 005 combined)

| State | Screen | Active Zones |
|---|---|---|
| `blank` | Black | None |
| `carousel` | Carousel images 001–004 | Right nav, Left nav, CTA hotspot |
| `terms` | Terms and Conditions | Accept hotspot |
| `choose-receive-01` | Choose How to Receive Money 01 P2P | Radio hotspot *(NEW)* |
| `choose-receive-02` | Choose How to Receive Money 02 P2P | Continue hotspot *(NEW)* |
| `select-account-01` | Select Account 01 P2P | Radio hotspot *(NEW)* |
| `select-account-02` | Select Account 02 P2P | Continue/process hotspot *(NEW)* |
| `processing` | Faded + spinner overlay | None |
| `congratulations` | Congratulations P2P | None |

---

## State Transitions (extended)

| From | Event | Guard | To | Animation |
|---|---|---|---|---|
| `terms` | Accept & Continue tap | `!isAnimating` | `choose-receive-01` | 300ms slide (existing `showChooseScreen`) |
| `choose-receive-01` | Radio zone tap | — | `choose-receive-02` | Instant swap (< 50ms) |
| `choose-receive-02` | Continue tap | `!isAnimating` | `select-account-01` | 300ms slide |
| `select-account-01` | Radio zone tap | — | `select-account-02` | Instant swap (< 50ms) |
| `select-account-02` | Continue tap | `!isAnimating` | `processing` | Immediate then 1s timer |
| `processing` | 1s timer fires | — | `congratulations` | Direct src swap |
| `congratulations` | Any phone tap | — | `congratulations` | No-op |
| any | Use-case radio change | — | `blank` | Immediate + cancel timer |

---

## Image Constants (all)

```js
const IMAGES = [
  'images/Zelle P2P Enrollment Carousel Image 001.png',
  'images/Zelle P2P Enrollment Carousel Image 002.png',
  'images/Zelle P2P Enrollment Carousel Image 003.png',
  'images/Zelle P2P Enrollment Carousel Image 004.png'
];
const TERMS_IMAGE       = 'images/Terms and Conditions.png';
const CHOOSE_01_IMAGE   = 'images/Choose How to Receive Money 01 P2P.png';
const CHOOSE_02_IMAGE   = 'images/Choose How to Receive Money 02 P2P.png';
const SELECT_01_IMAGE   = 'images/Select Account 01 P2P.png';
const SELECT_02_IMAGE   = 'images/Select Account 02 P2P.png';
const CONGRATS_IMAGE    = 'images/Congratulations P2P.png';
```

---

## Hotspot Elements (all, including existing)

| Element ID | Active State | Position (top / height) | Z-index | Notes |
|---|---|---|---|---|
| `#left-zone` | `carousel` | 0 / 100% | 10 | Left nav half |
| `#click-zone` | `carousel` | 0 / 100% | 10 | Right nav half |
| `#cta-hotspot` | `carousel` | 80% / 20% | 11 | GET STARTED CTA |
| `#accept-hotspot` | `terms` | 93% / 6% | 12 | Accept & Continue button |
| `#choose-radio-hotspot` | `choose-receive-01` | ~40% / ~30% | 11 | Radio buttons on Choose 01 |
| `#choose-continue-hotspot` | `choose-receive-02` | ~88% / ~8% | 11 | Continue CTA on Choose 02 |
| `#select-radio-hotspot` | `select-account-01` | ~45% / ~30% | 11 | Radio buttons on Select 01 |
| `#select-continue-hotspot` | `select-account-02` | ~88% / ~8% | 11 | Continue CTA on Select 02 |

**All hotspots default to `pointer-events: none`** and are activated only in their designated state via a dedicated `update*Hotspot()` function or inline in the transition function.

---

## Spinner Element

Created dynamically and appended to `.phone-screen` when entering `processing` state. Removed when exiting.

```css
#spinner {
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  width: 48px; height: 48px;
  border-radius: 50%;
  border: 3px solid rgba(255,255,255,0.25);
  border-top-color: #fff;
  animation: spin 0.7s linear infinite;
  z-index: 20;
  pointer-events: none;
}
@keyframes spin { to { transform: translate(-50%, -50%) rotate(360deg); } }
```

---

## Validation Rules

- Instant-swap functions do NOT set `isAnimating = true` — they are synchronous and complete before the next event.
- All slide-in transitions set `isAnimating = true` on entry and `false` on completion (300 ms `setTimeout` callback).
- `spinnerTimer` stores the `setTimeout` ID; `resetToBlank()` calls `clearTimeout(spinnerTimer)` before doing anything else.
- `resetToBlank()` removes `#spinner` from DOM if it exists (`document.getElementById('spinner')?.remove()`).
- `resetToBlank()` resets `current.style.opacity = '1'` and `current.style.transition = 'none'` to undo any in-progress fade.
