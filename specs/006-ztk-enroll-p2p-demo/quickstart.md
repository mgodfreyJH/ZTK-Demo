# Quickstart: PayCenter ZTK Demo - Enroll P2P Flow (006)

**Branch**: `006-ztk-enroll-p2p-demo`
**Date**: 2026-07-30 (amended; original 2026-07-27)

> Amendment note: Adds SMB flow validation steps and updates the use-case panel to two options (P2P, SMB).

---

## Prerequisites

- Local project checkout with `ZTK Demo Project/index.html`
- `ZTK Demo Project/images/` contains all required PNG assets from the spec
- Desktop browser: Chrome, Edge, Firefox, or Safari
- No package install, build step, or backend service required

---

## Run

1. Open `ZTK Demo Project/index.html` directly in a browser.
2. Verify baseline layout:
- JH background fills page
- use-case panel appears left of device (exactly two options: **P2P**, **SMB**)
- device frame appears on right
- lock screen image appears by default
3. Select **P2P** and walk the canonical forward flow to **Landing Page** (19 transitions).
4. Select **SMB** and walk the canonical forward flow to **Landing Page** (24 transitions).

---

## Canonical Flow Validation — P2P (19 transitions)

Execute this sequence in order:

1. Carousel 001 -> GET STARTED -> Terms
2. Terms -> Accept and Continue -> Choose Receive 01
3. Choose Receive 01 -> radio selection -> Choose Receive 02
4. Choose Receive 02 -> Continue -> Authenticate OTP 01 SMB (shared screen)
5. Authenticate OTP 01 SMB -> click leftmost OTP box -> Authenticate OTP 02 SMB
6. Authenticate OTP 02 SMB -> Verify -> spinner 1s -> Select Account 01
7. Select Account 01 -> radio selection -> Select Account 02
8. Select Account 02 -> Continue -> spinner 1s -> Congratulations
9. Congratulations -> Send or Request Money -> ZRC Feature Intro
10. ZRC Feature Intro -> Access Contacts -> Allow Access 01
11. Allow Access 01 -> Allow -> Allow Access 02
12. Allow Access 02 -> Continue -> How Do You Want to Share
13. How Do You Want to Share -> Select Contacts -> Select and Continue
14. Select and Continue -> Continue -> Allow Access to 15
15. Allow Access to 15 -> Allow Selected Contacts -> Landing Page

Also validate carousel wrap behavior:
- left from 001 wraps to 004
- right from 004 wraps to 001

---

## Canonical Flow Validation — SMB (24 transitions)

Execute this sequence in order:

1. SMB Carousel 001 -> GET STARTED -> Terms
2. Terms -> Accept and Continue (SMB context) -> Choose Receive 01 SMB
3. Choose Receive 01 SMB -> radio selection -> Choose Receive 02 SMB
4. Choose Receive 02 SMB -> Continue -> Authenticate OTP 03 SMB
5. Authenticate OTP 03 SMB -> radio selection -> Authenticate OTP 04 SMB
6. Authenticate OTP 04 SMB -> Continue -> Authenticate OTP 01 SMB
7. Authenticate OTP 01 SMB -> click leftmost OTP box -> Authenticate OTP 02 SMB
8. Authenticate OTP 02 SMB -> Verify -> spinner 1s -> Create Zelle Tag 01 SMB
9. Create Zelle Tag 01 SMB -> click tag entry area -> Create Zelle Tag 02 SMB
10. Create Zelle Tag 02 SMB -> Check Availability -> spinner 1s -> Create Zelle Tag 03 SMB (not available)
11. Create Zelle Tag 03 SMB -> click "MikesLawnCare" suggestion -> Create Zelle Tag 04 SMB
12. Create Zelle Tag 04 SMB -> Check Availability -> spinner 1s -> Create Zelle Tag 05 SMB (available)
13. Create Zelle Tag 05 SMB -> Save and Continue -> Select Primary Account 01 SMB
14. Select Primary Account 01 SMB -> radio selection -> Select Primary Account 02 SMB
15. Select Primary Account 02 SMB -> Continue -> spinner 1s -> Congratulations SMB
16. Congratulations SMB -> Send or Request Money -> ZRC Feature Intro
17. Wait 5s with no click -> Zelle Tag Feature Introduction SMB slides in; wait another 5s -> ZRC Feature Intro slides back in (verify indefinite alternation)
18. ZRC Feature Intro -> Access Contacts (or Zelle Tag Feature Introduction SMB -> Skip For Now) -> Allow Access 01 *(shared with P2P)*
19. Allow Access 01 -> Allow -> Allow Access 02
20. Allow Access 02 -> Continue -> How Do You Want to Share
21. How Do You Want to Share -> Select Contacts -> Select and Continue
22. Select and Continue -> Continue -> Allow Access to 15
23. Allow Access to 15 -> Allow Selected Contacts -> Landing Page

Also validate: clicking during the 5s SMB rotation cancels the timer and immediately proceeds via the clicked screen's CTA.

---

## Timing Checks

- Non-loading, non-carousel-animated transitions begin within 250ms of click.
- Carousel slide animations are 300ms +/- 50ms (both P2P and SMB).
- Processing state from Select Account 02 to Congratulations (P2P) and from Authenticate OTP 02 to Create Zelle Tag 01 (SMB) lasts 1 second.
- SMB ZRC/Zelle Tag feature-intro rotation alternates every 5 seconds (±0.5s).

---

## Reset Behavior Checks

- Selecting P2P always restarts at P2P Carousel 001.
- Selecting SMB always restarts at SMB Carousel 001.
- Switching use cases mid-flow clears prior state immediately and shows Lock Screen before the new flow's carousel appears.

---

## Troubleshooting

- Broken image in device frame: verify exact image file names in `ZTK Demo Project/images/`.
- CTA click not responding: recalibrate hotspot bounds in CSS for that screen.
- Unexpected transitions during animation: verify `isAnimating` guard logic in interaction handlers.
