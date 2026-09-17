# Quickstart: PayCenter ZTK Interactive Demo

**Feature**: PayCenter ZTK Interactive Demo  
**Date**: 2026-05-15

---

## Prerequisites

- A modern desktop web browser (Chrome, Edge, Firefox, or Safari — latest stable)
- No Node.js, no npm, no build tools required

---

## Running the Demo

1. Open a terminal (or File Explorer) and navigate to the `ZTK Demo Project/` folder.
2. Open `index.html` directly in your browser:

   **Option A — Double-click** `index.html` in File Explorer (opens as `file://`)

   **Option B — VS Code Live Server** (recommended for smooth asset loading):
   - Install the [Live Server extension](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) if not already installed
   - Right-click `index.html` → **Open with Live Server**
   - Browser opens at `http://127.0.0.1:5500/`

3. The demo loads immediately — no install, no compile step.

---

## Using the Demo

| Action | Result |
|--------|--------|
| Click the **right half** of the phone screen | Slides to the next enrollment screen (wraps 004 → 001) |
| Click the **left half** of the phone screen | Slides to the previous enrollment screen (wraps 001 → 004) |
| Click the **bottom area** of any carousel screen (GET STARTED button) | Displays the Terms and Conditions screen |
| Click the **Accept & Continue button** on the Terms and Conditions screen | Displays the Choose How to Receive Money P2P screen |
| Click **anywhere else** on the Terms and Conditions screen | Nothing happens |
| The Choose How to Receive Money P2P screen | Terminal state — no interactions. Reload the page to restart. |

---

## File Structure

```
ZTK Demo Project/
├── index.html          ← Single file: all HTML, CSS, and JS
└── images/
    ├── Zelle P2P Enrollment Carousel Image 001.png
    ├── Zelle P2P Enrollment Carousel Image 002.png
    ├── Zelle P2P Enrollment Carousel Image 003.png
    ├── Zelle P2P Enrollment Carousel Image 004.png
    └── Terms and Conditions.png
```

---

## Modifying the Demo

**Change the animation speed**: In `index.html`, search for `300ms` — two occurrences in the JS `advance()` and `retreat()` functions control the slide duration. Update both. The CSS `.slide` transition duration must also match.

**Change the Accept & Continue hit area**: The `#accept-hotspot` CSS rule (`top: X%; height: Y%`) controls the T&C button tap region. Open `Terms and Conditions.png` (828×1792px) and measure the vertical position of the Accept & Continue button as a percentage of the image height, then set `top` and `height` accordingly. Note that `object-fit: contain` will letterbox the image inside the phone viewport, so account for any blank bars above/below the image when calculating the percentage relative to the viewport.

**Add a new screen**: Add a new entry to the `IMAGES` array in the `<script>` block. The carousel logic handles any array length automatically.
