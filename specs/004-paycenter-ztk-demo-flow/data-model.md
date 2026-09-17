# Data Model: PayCenter ZTK Demo – Enroll P2P Flow

**Phase**: 1 — Design  
**Date**: 2026-05-18  
**Branch**: `004-paycenter-ztk-demo-flow`

---

## Runtime State

The application maintains a single flat state object in JavaScript. There is no persistence layer — state resets on page reload.

```js
const state = {
  // Which radio button is selected. null = nothing selected.
  useCase: null,           // null | 'enroll-p2p' | 'enroll-smb' | 'send-p2p' | 'send-smb'

  // Current screen within the flow.
  flowState: 'blank',      // 'blank' | 'carousel' | 'terms' | 'choose-receive'

  // Index into IMAGES array (0–3). Only meaningful when flowState === 'carousel'.
  carouselIndex: 0,

  // Lock-out flag. While true, all tap inputs on the phone screen are ignored.
  isAnimating: false,
};
```

---

## Entities

### UseCase

| Field | Type | Values | Description |
|---|---|---|---|
| `id` | string | `'enroll-p2p'`, `'enroll-smb'`, `'send-p2p'`, `'send-smb'` | Machine identifier |
| `label` | string | "Enroll P2P", "Enroll SMB", "Send P2P Payment", "Send SMB Payment" | Display text on radio button |
| `hasFlow` | boolean | `true` (P2P only), `false` (others) | Whether a flow is implemented |

### Screen

Screens are static image paths. No dynamic data fields.

| Constant | File | When Shown |
|---|---|---|
| `IMAGES[0]` | `Zelle P2P Enrollment Carousel Image 001.png` | Carousel, index 0 |
| `IMAGES[1]` | `Zelle P2P Enrollment Carousel Image 002.png` | Carousel, index 1 |
| `IMAGES[2]` | `Zelle P2P Enrollment Carousel Image 003.png` | Carousel, index 2 |
| `IMAGES[3]` | `Zelle P2P Enrollment Carousel Image 004.png` | Carousel, index 3 |
| `TERMS_IMAGE` | `Terms and Conditions.png` | After tapping GET STARTED CTA |
| `CHOOSE_IMAGE` | `Choose How to Receive Money 01 P2P.png` | After tapping Accept & Continue |

### CTA Zone

Overlay element on the phone screen, active during `flowState === 'carousel'` only.

| Field | Value | Source |
|---|---|---|
| `top` | 80% of phone screen height | Empirically measured from PNG |
| `height` | 20% of phone screen height | Covers full GET STARTED area |
| `width` | 100% | Left-to-right full width |
| `z-index` | 11 (beats nav zones at z=10) | Enforces CTA priority rule |

### Accept & Continue Zone

Overlay element on the phone screen, active during `flowState === 'terms'` only.

| Field | Value | Source |
|---|---|---|
| `top` | 93% of phone screen height | Measured: y=1672–1766 in 1792px T&C image |
| `height` | 6% of phone screen height | Covers button area |
| `width` | 100% | Left-to-right full width |
| `z-index` | 12 (beats CTA and nav zones) | Highest priority while T&C active |

---

## State Transition Table

| Current State | Event | Guard | Next State | Side Effects |
|---|---|---|---|---|
| `blank` | Radio: Enroll P2P | — | `carousel` (index 0) | Load image 001, enable nav+CTA zones |
| `carousel` | Radio: non-P2P | — | `blank` | Clear phone screen, disable all zones |
| `carousel` | Click right nav | `!isAnimating` | `carousel` (index+1 mod 4) | Slide animation left→right |
| `carousel` | Click left nav | `!isAnimating` | `carousel` (index-1 mod 4) | Slide animation right→left |
| `carousel` | Click CTA | `!isAnimating` | `terms` | Slide T&C in from right, disable nav zones |
| `terms` | Click Accept & Continue | `!isAnimating` | `choose-receive` | Slide Choose screen in, disable all zones |
| `terms` | Click elsewhere | — | `terms` | No-op |
| `choose-receive` | Any click | — | `choose-receive` | No-op (all zones disabled) |

---

## Validation Rules

- Carousel advance/retreat is blocked when `isAnimating === true` (clicks dropped, not queued).
- CTA zone is only pointer-active when `flowState === 'carousel'`.
- Accept hotspot is only pointer-active when `flowState === 'terms'`.
- Nav zones are pointer-inactive when `flowState` is `'terms'` or `'choose-receive'`.
- Switching from Enroll P2P to another use case mid-flow resets all state to `blank`.
