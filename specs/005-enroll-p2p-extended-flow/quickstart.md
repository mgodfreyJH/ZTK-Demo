# Quickstart: Enroll P2P Extended Flow

**Branch**: `005-enroll-p2p-extended-flow`  
**Date**: 2026-05-19  
**Extends**: Feature 004 (`004-paycenter-ztk-demo-flow`)

---

## Prerequisites

- A modern desktop browser (Chrome, Edge, Firefox, Safari — 2022 or later)
- No install, no build step, no server required

---

## Running the Demo

Open `ZTK Demo Project/index.html` directly in a browser (double-click or drag-and-drop).

---

## Demo Script: Full Enroll P2P Flow (004 + 005)

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Open the page | Phone screen is blank/black |
| 2 | Click **Enroll P2P** radio button | Carousel image 001 appears |
| 3 | Navigate carousel right/left | Images slide 001→004 with wrap |
| 4 | Tap the **GET STARTED** area (bottom ~20%) | Terms & Conditions slides in |
| 5 | Tap **Accept & Continue** | Choose How to Receive Money **01** appears |
| 6 | Tap either **radio button** on the screen | Choose How to Receive Money **02** appears instantly (no animation) |
| 7 | Tap the **Continue** CTA (bottom of screen) | Select Account **01** slides in |
| 8 | Tap either **radio button** on the screen | Select Account **02** appears instantly (no animation) |
| 9 | Tap the **Continue** CTA (bottom of screen) | Screen fades, spinner appears |
| 10 | Wait 1 second | Spinner disappears, **Congratulations P2P** appears |
| 11 | Screen is fully inert — reload page to restart |

---

## Resetting Mid-Flow

Clicking **any use-case radio button** (including switching back to Enroll P2P) from the panel at any point immediately resets the phone screen to black, even if an animation or spinner is running. This is the fastest way to restart the demo without reloading.

---

## File Structure

```text
ZTK Demo Project/
├── index.html          ← single-file app; open this in a browser
└── images/
    ├── JHBackground.png
    ├── Zelle P2P Enrollment Carousel Image 001–004.png
    ├── Terms and Conditions.png
    ├── Choose How to Receive Money 01 P2P.png
    ├── Choose How to Receive Money 02 P2P.png
    ├── Select Account 01 P2P.png
    ├── Select Account 02 P2P.png
    └── Congratulations P2P.png
```
