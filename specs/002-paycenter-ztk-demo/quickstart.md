# Quickstart: PayCenter ZTK Demo — Zelle P2P Enrollment Carousel

**Branch**: `002-paycenter-ztk-demo`  
**Updated**: 2026-05-11

---

## Prerequisites

- A modern browser (Chrome, Edge, Firefox, or Safari)
- No server, no build step, no Node.js required

---

## Run the Demo

1. Open the `ZTK Demo Project/` folder in Windows Explorer (or VS Code Explorer).
2. Double-click `index.html` — it opens directly in your default browser.
3. A phone mockup appears with Zelle P2P Enrollment screen 1 displayed.

---

## Using the Demo

| Interaction | Result |
|-------------|--------|
| Click the **right half** of the phone screen | Advance to next screen (slides left) |
| Click the **left half** of the phone screen | Go back to previous screen (slides right) |
| Navigate past screen 4 (right click) | Wraps back to screen 1 |
| Navigate before screen 1 (left click) | Wraps forward to screen 4 |
| Click **GET STARTED** on screen 4 | Transitions to the Zelle Access Contacts Permission screen |
| While on the permission screen | No navigation — terminal state for now |

> The left/right zones and GET STARTED area are all invisible — the cursor changes to a pointer when hovering over them.

---

## Project Files

```
ZTK Demo Project/
├── index.html                                      ← single application file
└── images/
    ├── Zelle P2P Enrollment Carousel Image 001.png
    ├── Zelle P2P Enrollment Carousel Image 002.png
    ├── Zelle P2P Enrollment Carousel Image 003.png
    ├── Zelle P2P Enrollment Carousel Image 004.png
    └── Zelle Access Contacts Permission.png
```

---

## Sharing the Demo

Zip the entire `ZTK Demo Project/` folder. Recipients unzip and open `index.html` — no installation required.

**Branch**: `002-paycenter-ztk-demo`  
**Date**: 2026-05-11

---

## Prerequisites

- A modern browser (Chrome, Edge, Firefox, or Safari)
- No server, no build step, no Node.js required

---

## Run the Demo

1. Open the `ZTK Demo Project/` folder in Windows Explorer (or VS Code Explorer).
2. Double-click `index.html` — it opens directly in your default browser.
3. The phone mockup appears with Zelle P2P Enrollment screen 1 displayed.

> **VS Code tip**: Right-click `index.html` → **Open with Live Server** (if the extension is installed) for automatic reload on edits.

---

## Using the Demo

| Interaction | Result |
|-------------|--------|
| Click the **right half** of the phone screen | Advance to next screen (slides left) |
| Click the **left half** of the phone screen | Go back to previous screen (slides right) |
| Navigate past screen 4 (right click) | Wraps back to screen 1 |
| Navigate before screen 1 (left click) | Wraps forward to screen 4 |

> The left/right zones are invisible — the cursor changes to a pointer when hovering over either half of the screen to indicate interactivity.

---

## Project Files

```
ZTK Demo Project/
├── index.html                                      ← single application file
└── images/
    ├── Zelle P2P Enrollment Carousel Image 001.png
    ├── Zelle P2P Enrollment Carousel Image 002.png
    ├── Zelle P2P Enrollment Carousel Image 003.png
    └── Zelle P2P Enrollment Carousel Image 004.png
```

---

## Updating Images

Replace the PNG files in `images/` with new versions using the same filenames. No code change needed.

---

## Sharing the Demo

Zip the entire `ZTK Demo Project/` folder (including `images/`). Recipients unzip and open `index.html` — no installation required.
