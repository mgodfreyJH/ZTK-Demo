# Data Model: PayCenter ZTK Interactive Demo

**Phase**: 1 — Design  
**Date**: 2026-05-15  
**Feature**: [spec.md](./spec.md)

---

## Entities

### CarouselSlide

Represents one of the four Zelle P2P Enrollment onboarding screens.

| Field | Type | Description |
|-------|------|-------------|
| `index` | integer (0–3) | Zero-based position in the carousel sequence |
| `src` | string (URL) | Relative path to the PNG image asset |
| `alt` | string | Accessible description for the image |

**Sequence**: 0 → 1 → 2 → 3 → 0 (circular, both directions)

**Instances** (compile-time constant, no dynamic data):

| index | src | alt |
|-------|-----|-----|
| 0 | `images/Zelle P2P Enrollment Carousel Image 001.png` | Enrollment screen 1 |
| 1 | `images/Zelle P2P Enrollment Carousel Image 002.png` | Enrollment screen 2 |
| 2 | `images/Zelle P2P Enrollment Carousel Image 003.png` | Enrollment screen 3 |
| 3 | `images/Zelle P2P Enrollment Carousel Image 004.png` | Enrollment screen 4 |

---

### TermsScreen

Represents the Terms and Conditions screen. Intermediate — one interaction (Accept & Continue) advances to ChooseScreen.

| Field | Type | Description |
|-------|------|-------------|
| `src` | string (URL) | `images/Terms and Conditions.png` |
| `alt` | string | `"Terms and Conditions"` |
| `dimensions` | 828×1792px | Source PNG dimensions (for hotspot calibration) |

---

### ChooseScreen

Represents the Choose How to Receive Money P2P screen. Terminal — no interactions.

| Field | Type | Description |
|-------|------|-------------|
| `src` | string (URL) | `images/Choose How to Receive Money P2P.png` |
| `alt` | string | `"Choose How to Receive Money"` |

---

### DemoState (runtime, held in JS variables)

The complete mutable state of the running demo. No persistence — resets on page reload.

| Field | Type | Initial Value | Description |
|-------|------|---------------|-------------|
| `currentIndex` | integer | `0` | Index into `IMAGES[]` for the currently visible slide |
| `isAnimating` | boolean | `false` | True while a CSS slide transition is in progress; blocks new taps |
| `isTermsScreen` | boolean | `false` | True when the Terms and Conditions screen is displayed |
| `isChooseScreen` | boolean | `false` | True when the Choose How to Receive Money screen is displayed |

---

## State Transitions

```
                    ┌──────────────────────────────────────────────────
                    ▼                                                  
[INIT] ──► CarouselScreen(0)                                          
               │         │                                             
          tap-right   tap-left                                         
               │         │                                             
               ▼         ▼                                             
        CarouselScreen(next)   CarouselScreen(prev)                    
               │                      │                                
          ─────┴──────────────────────┘                               
                         │                                             
                    tap-GET_STARTED (bottom 20%)                       
                         │                                             
                         ▼                                             
                   TermsScreen ──── tap-Accept&Continue ──► ChooseScreen (terminal)
```

**Guards**:
- All transitions blocked when `isAnimating = true`
- `tap-right` and `tap-left` blocked when `isTermsScreen = true` or `isChooseScreen = true`
- `tap-GET_STARTED` blocked when `isTermsScreen = true` or `isChooseScreen = true`
- `tap-Accept&Continue` only fires when `isTermsScreen = true`
- No interactions defined on ChooseScreen (`isChooseScreen = true` blocks everything)

---

## DOM Elements

| Element ID | Role | z-index |
|------------|------|---------|
| `#slide-current` | Visible image (`<img>`) | 1 (implicit) |
| `#slide-incoming` | Off-screen staging image (`<img>`) | 1 (implicit) |
| `#left-zone` | Left 50% tap zone (`<div>`) | 10 |
| `#click-zone` | Right 50% tap zone (`<div>`) | 10 |
| `#cta-hotspot` | Bottom 20% GET STARTED zone (`<div>`) | 11 |
| `#accept-hotspot` | Accept & Continue zone on T&C screen (`<div>`) | 12 |

---

## Image Asset Inventory

| Filename | Used | Role |
|----------|------|------|
| `Zelle P2P Enrollment Carousel Image 001.png` | ✅ | Carousel slide 0 |
| `Zelle P2P Enrollment Carousel Image 002.png` | ✅ | Carousel slide 1 |
| `Zelle P2P Enrollment Carousel Image 003.png` | ✅ | Carousel slide 2 |
| `Zelle P2P Enrollment Carousel Image 004.png` | ✅ | Carousel slide 3 |
| `Terms and Conditions.png` | ✅ | Terms screen (828×1792px) |
| `Choose How to Receive Money P2P.png` | ✅ | Choose screen (terminal) |
| `Zelle Access Contacts Permission.png` | ❌ | Out of scope |
