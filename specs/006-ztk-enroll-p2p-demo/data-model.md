# Data Model: PayCenter ZTK Demo - Enroll P2P Flow (006)

**Phase**: 1 - Design
**Date**: 2026-07-30 (amended; original 2026-07-27)
**Branch**: `006-ztk-enroll-p2p-demo`

> Amendment note (2026-07-30): Adds the SMB flow states/transitions and the two-option use-case model; retires placeholder use cases.

---

## Core Entities

### DemoApplication

Represents the single-page runtime orchestrating layout and interactions.

Fields:
- `activeUseCase`: enum (`p2p`, `smb`)
- `flowState`: enum (defined below)
- `isAnimating`: boolean
- `currentCarouselIndex`: integer (0-3)
- `spinnerTimerId`: nullable timer reference
- `rotationTimerId`: nullable timer reference (SMB feature-intro alternation only)

Validation rules:
- `currentCarouselIndex` must stay within 0-3.
- `isAnimating=true` blocks transition-triggering events.
- Changing use case clears active timers (`spinnerTimerId`, `rotationTimerId`) and resets state.
- The use-case panel offers exactly two selectable values: `p2p` and `smb`. There is no longer a placeholder/no-flow use case.

### DeviceScreen

Display surface inside the phone mockup.

Fields:
- `currentImagePath`: string
- `overlayMode`: enum (`none`, `fade-50-spinner`)
- `enabledHotspots`: list of hotspot ids

Validation rules:
- Image path must exist in configured asset list.
- Terminal state enables no hotspots.

### Hotspot

Clickable interaction zone bound to a state.

Fields:
- `id`: string
- `state`: flow state enum
- `bounds`: `{ topPct, leftPct, widthPct, heightPct }`
- `event`: interaction event name
- `nextState`: flow state enum

Validation rules:
- Hotspot is active only when `flowState === state`.
- Hotspot transitions must no-op when `isAnimating=true`.

---

## Flow State Enum

```text
lock-screen
carousel-001
carousel-002
carousel-003
carousel-004
terms
choose-receive-01
choose-receive-02
select-account-01
select-account-02
processing
congratulations
zrc-feature-intro
allow-contacts-01
allow-contacts-02
how-share
select-and-continue
allow-access-15
landing-page
smb-carousel-001
smb-carousel-002
smb-carousel-003
smb-carousel-004
smb-choose-receive-01
smb-choose-receive-02
smb-otp-01
smb-otp-02
smb-processing
smb-create-tag-01
smb-create-tag-02
smb-create-tag-03
smb-create-tag-04
smb-create-tag-05
smb-select-primary-01
smb-select-primary-02
smb-congratulations
smb-zrc-feature-intro
smb-zelle-tag-feature-intro
```

Notes:
- `lock-screen` is the default state on load and on any use-case reset.
- `landing-page` is terminal (shared endpoint for both P2P and SMB flows).
- `terms` is shared between flows; the active `activeUseCase` value (`p2p`/`smb`) determines which `choose-receive-01` variant Accept & Continue routes to.
- `allow-contacts-01` through `landing-page` are shared between P2P and SMB (convergence point per FR-045).
- `smb-zrc-feature-intro` and `smb-zelle-tag-feature-intro` alternate under `rotationTimerId` until a qualifying click cancels the rotation.

---

## Transition Model (Canonical Forward Path)

| Step | From State | Event | To State | Transition Rule |
|---|---|---|---|---|
| 1 | `lock-screen` | select Enroll P2P | `carousel-001` | immediate |
| 2 | `carousel-*` | GET STARTED | `terms` | immediate start, rendered transition as configured |
| 3 | `terms` | Accept and Continue | `choose-receive-01` | immediate start |
| 4 | `choose-receive-01` | choose radio | `choose-receive-02` | instant, no slide |
| 5 | `choose-receive-02` | Continue | `select-account-01` | immediate start |
| 6 | `select-account-01` | choose radio | `select-account-02` | instant, no slide |
| 7 | `select-account-02` | Continue | `processing` -> `congratulations` | 1s spinner gate |
| 8 | `congratulations` | Send or Request Money | `zrc-feature-intro` | immediate start |
| 9 | `zrc-feature-intro` | Access Contacts | `allow-contacts-01` | immediate start |
| 10 | `allow-contacts-01` | Allow | `allow-contacts-02` | immediate start |
| 11 | `allow-contacts-02` | Continue | `how-share` | immediate start |
| 12 | `how-share` | Select Contacts | `select-and-continue` | immediate start |
| 13 | `select-and-continue` | Continue | `allow-access-15` | immediate start |
| 14 | `allow-access-15` | Allow Selected Contacts | `landing-page` | immediate start |

Additional behavioral rules:
- Carousel left/right navigation supports wrap-around (`001 <-> 004`) with 300ms slide behavior, for both P2P and SMB carousels.
- Selecting a use case always restarts that use case's flow from its own carousel-001 state, discarding any prior progress.

## Transition Model (SMB Canonical Forward Path, 22 transitions)

| Step | From State | Event | To State | Transition Rule |
|---|---|---|---|---|
| 1 | `lock-screen` | select SMB | `smb-carousel-001` | immediate |
| 2 | `smb-carousel-*` | GET STARTED | `terms` | immediate start |
| 3 | `terms` (smb context) | Accept and Continue | `smb-choose-receive-01` | immediate start |
| 4 | `smb-choose-receive-01` | choose radio | `smb-choose-receive-02` | instant, no slide |
| 5 | `smb-choose-receive-02` | Continue | `smb-otp-01` | immediate start |
| 6 | `smb-otp-01` | click leftmost OTP box | `smb-otp-02` | immediate start |
| 7 | `smb-otp-02` | Verify | `smb-processing` -> `smb-create-tag-01` | 1s spinner gate |
| 8 | `smb-create-tag-01` | click tag entry area | `smb-create-tag-02` | immediate start |
| 9 | `smb-create-tag-02` | Check Availability | `smb-create-tag-03` | immediate start |
| 10 | `smb-create-tag-03` | click suggested name | `smb-create-tag-04` | immediate start |
| 11 | `smb-create-tag-04` | Check Availability | `smb-create-tag-05` | immediate start |
| 12 | `smb-create-tag-05` | Save and Continue | `smb-select-primary-01` | immediate start |
| 13 | `smb-select-primary-01` | choose radio | `smb-select-primary-02` | instant, no slide |
| 14 | `smb-select-primary-02` | Continue | `smb-congratulations` | immediate start |
| 15 | `smb-congratulations` | Send or Request Money | `smb-zrc-feature-intro` | immediate start |
| 16 | `smb-zrc-feature-intro` | 5s no click | `smb-zelle-tag-feature-intro` | slide-in 300ms; repeats every 5s until clicked |
| 17 | `smb-zrc-feature-intro` | Access Contacts | `allow-contacts-01` | cancels rotation timer |
| 17b | `smb-zelle-tag-feature-intro` | Skip For Now | `allow-contacts-01` | cancels rotation timer |

From `allow-contacts-01`, SMB reuses the shared P2P steps 10-14 (`allow-contacts-02` → `how-share` → `select-and-continue` → `allow-access-15` → `landing-page`).

---

## Asset Model

Required asset groups:
- Global background: `JHBackground.png`
- Default/reset screen: `JH50 Lock Screen.png`
- Carousel: 4 images (`Zelle P2P Enrollment Carousel Image 001-004.png`)
- Terms and account setup sequence
- ZRC/contact permission sequence
- Terminal landing page

Validation rules:
- All listed assets must load locally from `ZTK Demo Project/images/`.
- Missing image must fail fast in test pass as broken-asset defect.

---

## Timing Constraints as Model Rules

- `transitionStartLatencyMs <= 250` for non-loading, non-carousel-animated transitions.
- `carouselAnimationMs = 300 +/- 50`.
- `processingDurationMs = 1000` before entering `congratulations`.

---

## Error/Edge Handling Rules

- Clicks outside active hotspots do not change state.
- Mid-animation clicks do not corrupt state; they are ignored or queued by implementation policy.
- Use-case switch always wins and resets state immediately to `lock-screen`.
- Reselecting Enroll P2P always re-enters `carousel-001` with clean state.
