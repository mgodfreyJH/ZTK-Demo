# Quickstart: PayCenter ZTK Demo – Enroll P2P Flow

**Branch**: `004-paycenter-ztk-demo-flow`  
**Date**: 2026-05-18

---

## Prerequisites

- A modern desktop browser (Chrome, Edge, Firefox, Safari — 2022 or later)
- No install, no build step, no server required

---

## Running the Demo

1. Open `ZTK Demo Project/index.html` directly in a browser (double-click or drag-and-drop)
2. The page loads with the JH background, a mobile phone mockup on the right, and a use-case panel on the left

---

## Demo Script: Enroll P2P Flow

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Open the page | Phone screen is blank/black; no use case selected |
| 2 | Click **Enroll P2P** radio button | Carousel image 001 appears on the phone screen |
| 3 | Click the **right half** of the phone screen | Image slides left; image 002 slides in from right |
| 4 | Click the **right half** again | Image 003 appears |
| 5 | Click the **right half** again | Image 004 appears |
| 6 | Click the **right half** again | Wraps back to image 001 |
| 7 | Click the **left half** of the phone screen | Image 004 slides in from left (wraps backward) |
| 8 | Click the **GET STARTED** area (bottom of screen) | Terms and Conditions screen slides in |
| 9 | Click anywhere on the T&C screen **except** Accept & Continue | Nothing happens |
| 10 | Click **Accept & Continue** | "Choose How to Receive Money 01 P2P" screen appears |
| 11 | Screen is now static — reload the page to restart |

---

## Resetting the Demo

Reload the page (`F5` or `Cmd+R`) to return to the initial blank state.

Alternatively, selecting a different use case radio button then selecting **Enroll P2P** again also resets the carousel to image 001.

---

## File Structure

```text
ZTK Demo Project/
├── index.html          ← single-file app; open this in a browser
└── images/
    ├── JHBackground.png
    ├── Zelle P2P Enrollment Carousel Image 001.png
    ├── Zelle P2P Enrollment Carousel Image 002.png
    ├── Zelle P2P Enrollment Carousel Image 003.png
    ├── Zelle P2P Enrollment Carousel Image 004.png
    ├── Terms and Conditions.png
    └── Choose How to Receive Money 01 P2P.png
```

All paths are relative — do not move `index.html` out of its folder.
