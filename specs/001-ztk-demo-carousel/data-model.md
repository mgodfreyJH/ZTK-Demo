# Data Model: PayCenter ZTK Demo Carousel

**Phase**: 1 — Design  
**Date**: 2026-05-07  
**Feature**: [spec.md](spec.md) | [research.md](research.md)

---

## Runtime State

This is a pure client-side, stateless-by-nature application. There is no persistence layer. All state lives in JavaScript memory for the lifetime of the page session.

### CarouselState

| Field | Type | Initial Value | Description |
|---|---|---|---|
| `images` | `string[]` | `['images/Zelle P2P Enrollment Carousel Image 001.png', ...]` | Ordered list of relative paths to the 4 enrollment images. Index position defines display order. |
| `currentIndex` | `number` (integer 0–3) | `0` | Index into `images[]` of the currently displayed image. |
| `isAnimating` | `boolean` | `false` | Lock flag. `true` while a slide transition is in progress. Prevents concurrent transitions. |

### Derived Values

| Expression | Value | Description |
|---|---|---|
| `nextIndex` | `(currentIndex + 1) % images.length` | Index of the next image; wraps 3 → 0 automatically. |
| `currentSrc` | `images[currentIndex]` | `src` of the currently visible image element. |
| `nextSrc` | `images[nextIndex]` | `src` of the image element staged off-screen right. |

---

## DOM Structure

The component tree corresponds directly to the visual hierarchy.

```
<body>                          — background: #1a1a2e
  <div.phone-frame>             — CSS phone silhouette (border, shadow, rounded corners)
    <div.phone-screen>          — clipping viewport (overflow: hidden)
      <img.slide#current>       — currently visible image (translateX: 0)
      <img.slide#incoming>      — pre-positioned off-screen right (translateX: 100%)
    </div.phone-screen>
    <div.click-zone>            — transparent overlay, right 50% of phone-screen bounds
  </div.phone-frame>
```

### Element Roles

| Element | Role | Key CSS |
|---|---|---|
| `.phone-frame` | Decorative phone body | `border-radius: 40px`, `box-shadow`, `background: #1c1c1e` |
| `.phone-screen` | Image clipping area | `overflow: hidden`, `position: relative` |
| `.slide` | Carousel image | `position: absolute`, `width: 100%`, `height: 100%`, `object-fit: contain` |
| `.click-zone` | Invisible right-half click target | `position: absolute`, `right: 0`, `width: 50%`, `height: 100%`, `cursor: pointer` |

---

## State Transitions

```
Initial Load
  → currentIndex = 0
  → current img src = images[0]
  → isAnimating = false

User clicks .click-zone (guard: isAnimating === false)
  → isAnimating = true
  → stage incoming img at translateX(100%)
  → apply CSS transition (350ms ease-in-out)
  → translate current img to translateX(-100%)
  → translate incoming img to translateX(0)
  → setTimeout(350ms):
      → currentIndex = nextIndex
      → reset DOM for next cycle
      → isAnimating = false

User clicks .click-zone (guard: isAnimating === true)
  → no-op (click ignored)
```

---

## Image Asset Registry

| Index | Filename | Relative Path |
|---|---|---|
| 0 | `Zelle P2P Enrollment Carousel Image 001.png` | `images/Zelle P2P Enrollment Carousel Image 001.png` |
| 1 | `Zelle P2P Enrollment Carousel Image 002.png` | `images/Zelle P2P Enrollment Carousel Image 002.png` |
| 2 | `Zelle P2P Enrollment Carousel Image 003.png` | `images/Zelle P2P Enrollment Carousel Image 003.png` |
| 3 | `Zelle P2P Enrollment Carousel Image 004.png` | `images/Zelle P2P Enrollment Carousel Image 004.png` |

*Source location for copy step*: `c:\users\migodfrey\ZTK Images\`

---

## Validation Rules

| Rule | Check | Spec Reference |
|---|---|---|
| Image count | Exactly 4 images in array | FR-006 |
| Index bounds | `currentIndex` always 0–3 via modulo | FR-005 |
| Transition lock | `isAnimating` set before DOM changes, cleared at 350ms | FR-007, SC-005 |
| No left navigation | No decrement of `currentIndex` anywhere | FR-003, Assumptions |
