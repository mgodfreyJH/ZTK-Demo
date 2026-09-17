# PayCenter ZTK Demo — Screen Flow Table

## Implemented Flow (Enroll P2P)

| Step | Current Screen | User Action / Trigger | Next Screen | Transition |
|------|---------------|----------------------|-------------|------------|
| 0 | Lock Screen (default) | Select **P2P** radio button | Carousel Image 001 | Instant |
| 1 | Carousel 001 | Click right side of screen | Carousel 002 | Slide left 300ms |
| 2 | Carousel 002 | Click right side of screen | Carousel 003 | Slide left 300ms |
| 3 | Carousel 003 | Click right side of screen | Carousel 004 | Slide left 300ms |
| 4 | Carousel 004 | Click right side of screen | Carousel 001 | Slide left 300ms (wraps) |
| 5 | Any Carousel (001-004) | Click left side of screen | Previous Carousel image | Slide right 300ms (wraps) |
| 6 | Any Carousel (001-004) | Click **GET STARTED** CTA | Terms and Conditions | Instant |
| 7 | Terms and Conditions | Click **Accept & Continue** | Choose How to Receive Money 01 P2P | Instant |
| 8 | Choose How to Receive Money 01 P2P | Select either radio button | Choose How to Receive Money 02 P2P | Instant |
| 9 | Choose How to Receive Money 02 P2P | Click **Continue** | Authenticate OTP 01 SMB *(shared screen)* | Instant |
| 10 | Authenticate OTP 01 SMB (P2P context) | Click leftmost of six OTP boxes | Authenticate OTP 02 SMB | Instant |
| 11 | Authenticate OTP 02 SMB (P2P context) | Click **Verify** | Processing (overlay) | Fade to 50% opacity + spinner 1 second |
| 12 | Processing (overlay) | *(automatic after 1 second)* | Select Account 01 P2P | Instant |
| 13 | Select Account 01 P2P | Select either radio button | Select Account 02 P2P | Instant |
| 14 | Select Account 02 P2P | Click **Continue** | Processing (overlay) | Fade to 50% opacity + spinner 1 second |
| 15 | Processing (overlay) | *(automatic after 1 second)* | Congratulations P2P | Instant |
| 16 | Congratulations P2P | Click **Send or Request Money** | ZRC Feature Intro | Instant |
| 17 | ZRC Feature Intro | Click **Access Contacts** | Allow Access To Contacts 01 | Instant |
| 18 | Allow Access To Contacts 01 | Click **Allow** | Allow Access To Contacts 02 | Instant |
| 19 | Allow Access To Contacts 02 | Click **Continue** | How Do You Want to Share | Instant |
| 20 | How Do You Want to Share | Click **Select Contacts** | Select And Continue | Instant |
| 21 | Select And Continue | Click **Continue** | Allow Access to 15 | Instant |
| 22 | Allow Access to 15 | Click **Allow Selected Contacts** | Landing Page | Instant |
| 23 | Landing Page | *(terminal state)* | — | Presenter resets by selecting either use case |

---

## Implemented Flow (Enroll SMB)

| Step | Current Screen | User Action / Trigger | Next Screen | Transition |
|------|---------------|----------------------|-------------|------------|
| 0 | Lock Screen (default) | Select **SMB** radio button | SMB Carousel Image 001 | Instant |
| 1 | SMB Carousel 001 | Click right side of screen | SMB Carousel 002 | Slide left 300ms |
| 2 | SMB Carousel 002 | Click right side of screen | SMB Carousel 003 | Slide left 300ms |
| 3 | SMB Carousel 003 | Click right side of screen | SMB Carousel 004 | Slide left 300ms |
| 4 | SMB Carousel 004 | Click right side of screen | SMB Carousel 001 | Slide left 300ms (wraps) |
| 5 | Any SMB Carousel (001-004) | Click left side of screen | Previous SMB Carousel image | Slide right 300ms (wraps) |
| 6 | Any SMB Carousel (001-004) | Click **GET STARTED** CTA | Terms and Conditions | Instant |
| 7 | Terms and Conditions | Click **Accept & Continue** (SMB context) | Choose How to Receive Money 01 SMB | Instant |
| 8 | Choose How to Receive Money 01 SMB | Select either radio button | Choose How to Receive Money 02 SMB | Instant |
| 9 | Choose How to Receive Money 02 SMB | Click **Continue** | Authenticate OTP 03 SMB | Instant |
| 10 | Authenticate OTP 03 SMB | Select radio button | Authenticate OTP 04 SMB | Instant |
| 11 | Authenticate OTP 04 SMB | Click **Continue** | Authenticate OTP 01 SMB | Instant |
| 12 | Authenticate OTP 01 SMB | Click leftmost of six OTP boxes | Authenticate OTP 02 SMB | Instant |
| 13 | Authenticate OTP 02 SMB | Click **Verify** | Create Zelle Tag 01 SMB | Fade to 50% opacity + spinner 1 second |
| 14 | Create Zelle Tag 01 SMB | Click "Enter your Zelle tag" area | Create Zelle Tag 02 SMB (Mikes Lawn Care Entered) | Instant |
| 15 | Create Zelle Tag 02 SMB | Click **Check Availability** | Create Zelle Tag 03 SMB (Mikes Lawn Care Not Available) | Fade to 50% opacity + spinner 1 second |
| 16 | Create Zelle Tag 03 SMB | Click "MikesLawnCare" suggestion | Create Zelle Tag 04 SMB (Mikes Dog Walking Entered) | Instant |
| 17 | Create Zelle Tag 04 SMB | Click **Check Availability** | Create Zelle Tag 05 SMB (Mikes Dog Walking Available) | Fade to 50% opacity + spinner 1 second |
| 18 | Create Zelle Tag 05 SMB | Click **Save and Continue** | Select Primary Account 01 SMB | Instant |
| 19 | Select Primary Account 01 SMB | Select either radio button | Select Primary Account 02 SMB | Instant |
| 20 | Select Primary Account 02 SMB | Click **Continue** | Congratulations SMB | Fade to 50% opacity + spinner 1 second |
| 21 | Congratulations SMB | Click **Send or Request Money** | ZRC Feature Intro | Instant |
| 22 | ZRC Feature Intro | No click for 5 seconds | Zelle Tag Feature Introduction SMB | Slide in from right, 300ms; repeats alternating every 5s until clicked |
| 23 | ZRC Feature Intro | Click **Access Contacts** | Allow Access To Contacts 01 *(shared with P2P)* | Instant |
| 24 | Zelle Tag Feature Introduction SMB | Click **Skip For Now** | Allow Access To Contacts 01 *(shared with P2P)* | Instant |

From Allow Access To Contacts 01, the SMB flow continues through the identical shared screens used by P2P (Allow Access To Contacts 02 → How Do You Want to Share → Select And Continue → Allow Access to 15 → Landing Page); see steps 15-19 of the Enroll P2P table above.

---

*Last updated: 2026-07-31 — Inserted the shared Authenticate OTP 01/02 SMB screens into the Enroll P2P flow (Choose How to Receive Money 02 P2P → OTP → Select Account 01 P2P), and added fade + spinner (1s) loading state to the Create Zelle Tag 02→03, Create Zelle Tag 04→05, and Select Primary Account 02→Congratulations SMB transitions.*
