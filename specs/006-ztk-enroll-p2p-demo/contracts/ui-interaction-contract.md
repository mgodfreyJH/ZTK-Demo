# UI Interaction Contract: PayCenter ZTK Demo — Enroll P2P & SMB Flows (006)

> Amended 2026-07-30: adds the Enroll SMB contract (previously implemented but undocumented), simplifies the use-case model to two options, and formalizes the SMB feature-intro rotation.

## Purpose

Define the observable UI behavior contract for the PayCenter ZTK demo so implementation and testing can validate the same interaction rules.

## Surface

- Single-page web UI rendered in `ZTK Demo Project/index.html`
- One device-screen viewport with state-driven image rendering
- Overlay hotspots for CTA and navigation interactions

## Contract Rules

### 1. Initial State

- On page load, device screen MUST show `JH50 Lock Screen.png`.
- No transition is in progress (`isAnimating=false`).

### 2. Use-Case Selection

- The use-case panel MUST offer exactly two options: `P2P` and `SMB`.
- Selecting `P2P` MUST reset flow and enter P2P Carousel 001.
- Selecting `SMB` MUST reset flow and enter SMB Carousel 001.
- Switching use case at any time MUST discard prior flow state immediately and show Lock Screen before the new flow's carousel is presented.

### 3. Carousel Interaction

- Device screen left half click: previous carousel image with slide-right animation.
- Device screen right half click: next carousel image with slide-left animation.
- Wrap behavior is mandatory:
  - from 001 left -> 004
  - from 004 right -> 001
- Carousel animation duration MUST be 300ms +/- 50ms.
- These rules apply identically to both the P2P carousel and the SMB carousel.

### 4. CTA/Hotspot Progression Contract — P2P

The following events MUST progress the P2P flow in order:

1. GET STARTED -> Terms
2. Accept and Continue -> Choose Receive 01 P2P
3. Radio select -> Choose Receive 02 P2P (instant, no slide)
4. Continue -> Authenticate OTP 01 SMB (shared screen)
5. Click leftmost OTP box -> Authenticate OTP 02 SMB
6. Verify -> Processing (fade/spinner) -> Select Account 01 P2P
7. Radio select -> Select Account 02 P2P (instant, no slide)
8. Continue -> Processing (fade/spinner) -> Congratulations P2P
9. Send or Request Money -> ZRC Feature Intro
10. Access Contacts -> Allow Access 01
11. Allow -> Allow Access 02
12. Continue -> How Do You Want to Share
13. Select Contacts -> Select and Continue
14. Continue -> Allow Access to 15
15. Allow Selected Contacts -> Landing Page

### 4b. CTA/Hotspot Progression Contract — SMB

The following events MUST progress the SMB flow in order:

1. GET STARTED -> Terms
2. Accept and Continue (SMB context) -> Choose Receive 01 SMB
3. Radio select -> Choose Receive 02 SMB (instant, no slide)
4. Continue -> Authenticate OTP 03 SMB
5. Radio select -> Authenticate OTP 04 SMB (instant, no slide)
6. Continue -> Authenticate OTP 01 SMB
7. Click leftmost OTP box -> Authenticate OTP 02 SMB
8. Verify -> Processing (fade/spinner) -> Create Zelle Tag 01 SMB
9. Click tag entry area -> Create Zelle Tag 02 SMB
10. Check Availability -> Processing (fade/spinner) -> Create Zelle Tag 03 SMB (name unavailable)
11. Click suggested name -> Create Zelle Tag 04 SMB
12. Check Availability -> Processing (fade/spinner) -> Create Zelle Tag 05 SMB (name available)
13. Save and Continue -> Select Primary Account 01 SMB
14. Radio select -> Select Primary Account 02 SMB (instant, no slide)
15. Continue -> Processing (fade/spinner) -> Congratulations SMB
16. Send or Request Money -> ZRC Feature Intro (SMB context)
17. 5s no click -> Zelle Tag Feature Introduction SMB (slide in from right); alternates every 5s until clicked
18. Access Contacts (ZRC) or Skip For Now (Zelle Tag Intro) -> Allow Access 01 *(shared with P2P; cancels rotation)*
19. Allow Access 01 -> ... -> Landing Page *(identical shared tail as P2P steps 10-15 above)*

### 5. Timing Contract

- Non-loading, non-carousel-animated transitions MUST begin within 250ms of user click.
- Processing transition (Authenticate OTP 02 SMB -> Select Account 01 P2P; Select Account 02 P2P -> Congratulations P2P; Authenticate OTP 02 SMB -> Create Zelle Tag 01 SMB; Create Zelle Tag 02 SMB -> Create Zelle Tag 03 SMB; Create Zelle Tag 04 SMB -> Create Zelle Tag 05 SMB; Select Primary Account 02 SMB -> Congratulations SMB) MUST display spinner for exactly 1 second before advancing.
- The SMB ZRC Feature Intro / Zelle Tag Feature Introduction SMB rotation MUST alternate every 5 seconds (±0.5s) until cancelled by a qualifying click.

### 6. Terminal State Contract

- Landing Page is terminal (shared endpoint for both P2P and SMB flows).
- No additional in-flow navigation may occur from Landing Page.
- A new use-case selection is required to restart/reset.

### 7. Safety/Guard Contract

- Clicks outside active hotspots MUST cause no state change.
- Triggering actions during animation MUST NOT corrupt flow; implementation may ignore or guard duplicate clicks while `isAnimating=true`.

## Verification Mapping

- Functional Requirements: FR-001 through FR-020, FR-022 through FR-051 (FR-021 retired)
- Success Criteria: SC-001 through SC-009
- Canonical end-to-end validation: 17-transition P2P forward-path test from Carousel 001 to Landing Page; 22-transition SMB forward-path test from SMB Carousel 001 to Landing Page (via shared Allow Access to Contacts 01 convergence)
