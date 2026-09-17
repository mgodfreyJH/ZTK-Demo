# Data Model: PayCenter ZTK Demo — Zelle P2P Enrollment Carousel

**Phase**: 1 — Design (updated 2026-05-11 for permission screen)  
**Branch**: `002-paycenter-ztk-demo`

---

## Entities

### 1. CarouselImage

| Attribute | Type | Description |
|-----------|------|-------------|
| `index` | integer (0–3) | Zero-based position in the ordered sequence |
| `src` | string (relative path) | Relative path from `index.html` to the PNG file |
| `alt` | string | Accessible description |

| index | src | alt |
|-------|-----|-----|
| 0 | `images/Zelle P2P Enrollment Carousel Image 001.png` | Zelle P2P Enrollment screen 1 |
| 1 | `images/Zelle P2P Enrollment Carousel Image 002.png` | Zelle P2P Enrollment screen 2 |
| 2 | `images/Zelle P2P Enrollment Carousel Image 003.png` | Zelle P2P Enrollment screen 3 |
| 3 | `images/Zelle P2P Enrollment Carousel Image 004.png` | Zelle P2P Enrollment screen 4 |

---

### 2. PermissionScreen

A non-carousel screen that appears after the GET STARTED CTA is tapped.

| Attribute | Type | Description |
|-----------|------|-------------|
| `src` | string | `images/Zelle Access Contacts Permission.png` |
| `dimensions` | 147 × 315px | Fills phone screen via `object-fit: contain` |

---

### 3. AppState

Runtime state managed in plain JavaScript. Ephemeral — no persistence.

| Attribute | Type | Initial value | Description |
|-----------|------|---------------|-------------|
| `currentIndex` | integer (0–3) | `0` | Index of the currently displayed carousel image |
| `isAnimating` | boolean | `false` | Debounce flag; `true` during 350ms transition |
| `isPermissionScreen` | boolean | `false` | `true` when the permission screen is currently displayed; blocks all carousel navigation |

**State transitions**:

```
Forward click — right zone (guard: !isAnimating && !isPermissionScreen):
  nextIndex = (currentIndex + 1) % 4
  [slide animation: current → −100%, incoming ← +100%]
  After 350ms: currentIndex = nextIndex

Backward click — left zone (guard: !isAnimating && !isPermissionScreen):
  prevIndex = (currentIndex − 1 + 4) % 4
  [slide animation: current → +100%, incoming ← −100%]
  After 350ms: currentIndex = prevIndex

GET STARTED CTA click (guard: !isAnimating && currentIndex === 3):
  [slide animation: current → −100%, permission image ← +100%]
  After 350ms: isPermissionScreen = true
  (clickZone and leftZone disabled via pointer-events: none; ctaHotspot already disabled)

Permission screen — terminal state:
  No further navigation until additional requirements are defined.
```

---

### 4. PhoneMockup (presentational — CSS only)

| Element | Role |
|---------|------|
| `.phone-frame` | 290 × 684px device body; rounded rectangle with shadow |
| `.phone-notch` | Top notch decoration |
| `.phone-screen` | `inset: 48px 12px 60px 12px`; overflow hidden viewport; 266 × 576px |
| `.home-bar` | Bottom home indicator |
| `.left-zone` | Invisible click target — left 50% of `.phone-screen`; `cursor: pointer` |
| `.click-zone` | Invisible click target — right 50% of `.phone-screen`; `cursor: pointer` |
| `#cta-hotspot` | Invisible hotspot at ~78% top of screen; active only on carousel image 004 |
| `#slide-current` | `<img>` showing the currently visible screen |
| `#slide-incoming` | `<img>` staged off-screen for the next transition |

---

## Validation Rules

- `currentIndex` MUST always be in `[0, 3]` — enforced by modulo arithmetic.
- `isAnimating` MUST be reset to `false` exactly 350ms after each transition — enforced by `setTimeout(fn, 350)`.
- `isPermissionScreen` MUST be `false` for any carousel navigation to proceed.
- `#cta-hotspot` `pointer-events` MUST be `all` only when `currentIndex === 3` AND `isPermissionScreen === false`.

**Phase**: 1 — Design  
**Branch**: `002-paycenter-ztk-demo`  
**Date**: 2026-05-11

---

## Entities

### 1. CarouselImage

Represents one Zelle P2P Enrollment screen asset.

| Attribute | Type | Description |
|-----------|------|-------------|
| `index` | integer (0–3) | Zero-based position in the ordered sequence |
| `src` | string (relative path) | Relative path from `index.html` to the PNG file |
| `alt` | string | Accessible description of the screen |

**Static registry** (no dynamic data source; fixed at build time):

| index | src | alt |
|-------|-----|-----|
| 0 | `images/Zelle P2P Enrollment Carousel Image 001.png` | Zelle P2P Enrollment screen 1 |
| 1 | `images/Zelle P2P Enrollment Carousel Image 002.png` | Zelle P2P Enrollment screen 2 |
| 2 | `images/Zelle P2P Enrollment Carousel Image 003.png` | Zelle P2P Enrollment screen 3 |
| 3 | `images/Zelle P2P Enrollment Carousel Image 004.png` | Zelle P2P Enrollment screen 4 |

---

### 2. CarouselState

Runtime state managed in JavaScript. Ephemeral — no persistence.

| Attribute | Type | Initial value | Description |
|-----------|------|---------------|-------------|
| `currentIndex` | integer (0–3) | `0` | Index of the currently displayed image |
| `isAnimating` | boolean | `false` | Debounce flag; `true` during a 350ms slide transition |

**State transitions**:

```
Forward click (right zone):
  Guard: isAnimating === false
  Action: isAnimating = true
          nextIndex = (currentIndex + 1) % 4
          [slide animation: current → −100%, incoming ← +100%]
          After 350ms: currentIndex = nextIndex, isAnimating = false

Backward click (left zone):
  Guard: isAnimating === false
  Action: isAnimating = true
          prevIndex = (currentIndex − 1 + 4) % 4
          [slide animation: current → +100%, incoming ← −100%]
          After 350ms: currentIndex = prevIndex, isAnimating = false
```

---

### 3. PhoneMockup (Presentational — CSS only)

No JavaScript state. Pure visual structure.

| Element | Role |
|---------|------|
| `.phone-frame` | Outer device body; rounded rectangle with shadow |
| `.phone-notch` | Top notch decoration |
| `.phone-screen` | Clipped viewport; contains carousel slides |
| `.home-bar` | Bottom home indicator decoration |
| `.left-zone` | Invisible click target — left 50% of `.phone-screen` |
| `.right-zone` | Invisible click target — right 50% of `.phone-screen` |
| `#slide-current` | `<img>` showing the currently displayed screen |
| `#slide-incoming` | `<img>` pre-loaded with the next/previous image; off-screen |

---

## Validation Rules

- `currentIndex` MUST always be in `[0, 3]` — enforced by modulo wrap arithmetic.
- `isAnimating` MUST be reset to `false` exactly 350ms after each transition starts — enforced by `setTimeout(fn, 350)` matching the CSS `transition-duration`.
- `src` paths MUST be relative (no `file://` or absolute Windows paths).
