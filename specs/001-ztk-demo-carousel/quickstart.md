# Quickstart: PayCenter ZTK Demo Carousel

**Date**: 2026-05-07

---

## Prerequisites

- A modern desktop web browser (Chrome, Edge, or Firefox)
- The 4 image files from `c:\users\migodfrey\ZTK Images\`

No installs, no Node.js, no server. Nothing to build.

---

## Setup (One Time)

1. **Copy the images** into the `images/` folder next to `index.html`:

   ```
   Copy from:   c:\users\migodfrey\ZTK Images\Zelle P2P Enrollment Carousel Image 001.png
                c:\users\migodfrey\ZTK Images\Zelle P2P Enrollment Carousel Image 002.png
                c:\users\migodfrey\ZTK Images\Zelle P2P Enrollment Carousel Image 003.png
                c:\users\migodfrey\ZTK Images\Zelle P2P Enrollment Carousel Image 004.png

   Copy into:   ZTK Demo Project\images\
   ```

   After copying, the folder structure should look like:

   ```
   ZTK Demo Project\
   ├── index.html
   └── images\
       ├── Zelle P2P Enrollment Carousel Image 001.png
       ├── Zelle P2P Enrollment Carousel Image 002.png
       ├── Zelle P2P Enrollment Carousel Image 003.png
       └── Zelle P2P Enrollment Carousel Image 004.png
   ```

2. **Open the demo** by double-clicking `index.html` in Windows Explorer. It will open in your default browser.

---

## Using the Demo

- The demo opens with **Image 001** displayed on the phone screen.
- **Click the right half of the phone screen** to advance to the next image.
- After **Image 004**, clicking again returns to **Image 001** — the demo loops indefinitely.

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| Blank/broken phone screen | Images not in `images/` folder | Re-check the copy step above; verify filenames match exactly |
| Images don't advance on click | Clicking the left half of the screen | Click on the right half of the phone screen only |
| Demo looks unstyled | Very old browser | Switch to a modern browser (Chrome 90+, Edge 90+, Firefox 88+) |
