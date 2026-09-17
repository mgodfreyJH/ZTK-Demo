## ZTK Demo Prompt file

# This file will get huge over time

Reference the image files in images folder in the project.

We want to build a demo application of the PayCenter ZTK product. This demo will have limited functionality that will be specified here. 

Use the JHBackground image in the background of the demo.

Position the demo to the right so that it's further away from the background text.

Build a table that documents the order of the screen flow and triggers for next screens and save it in a separate document.

Build a mermaid diagram of the demo flow that matches the table you document and save it in a separate document.

Display the front view of a generic mobile device (mobile phone) with the JH50 Lock Screen image.

Display a list of use cases with radio buttons to the left of the generic mobile device.  The use cases are P2P, and SMB.

Carousel slide animations must be 300ms unless otherwise specified.

When the user selects the P2P radio button, do the following things:
    1. Populate the screen of the phone with Zelle P2P Enrollment Carousel Image 001.
    2. When the user clicks the right side of the screen, slide the current image to the left in the device screen while sliding the next image in from the right.
    3. When the user clicks the left side of the screen, slide the current image to the right in the device screen while sliding the next image in from the left.
    4. The order of the screens follows the number at the end of each image name (001, 002, 003, 004).  Then 004 is displaying and the user clicks the right side of the screen, start over with image 001.
    5. Each of those images, 001 thru 004, has a get started button area on them. When the user clicks anywhere within the "GET STARTED" CTA, display the Terms and Conditions image.
    6. When the user clicks the Accept & Continue button on the Terms and Conditions image display the Choose How to Receive Money 01 P2P image. 
    7. When the user selects one of the two radio buttons on the Choose How to Receive Money 01 P2P image, display the Choose How to Receive Money 02 P2P image immediately without a 300ms slide animation.
    8. When the user click anywhere within the Continue CTA on the Choose How to Receive Money 02 P2P image, display the Authenitacte OTP 01 SMB image.
    11. When the user clicks on the leftmost box of the six boxes on the Authenticate OTP 01 SMB image, display the Authenticate OTP 02 SMB image.
    12. When the user clicks on the Verify button on the Authenticate OTP 02 SMB image, fade the image 50% and display a working icon for 1 second, then display the Select Account 01 P2P image.
    9. When the user selects one of the two radio buttons on the Select Account 01 P2P image, display the Select Account 02 P2P image immediately without a 300ms slide animation.
    10. When the user clicks anywhere within the Counintue CTA on the Select Account 02 P2P imaage, fade the image 50% and display a spinning working icon for 1 second, then display the Congratulations P2P image.
    11. When the user clicks the Sed or Request Money CTA on the Congratulations P2P image, display the ZRC Feature Intro image.
    12. When the user clicks anywhere within the Access Contacts CTA on the ZRC Feature Intro image, display the Allow Access to Contacts 01 image.
    13. When the user clicks anywhere within the Allow CTA on the Allow Access to Contacts 01 image, display the Allow Access to Contacts 02 image.
    14. When the user clicks anywhere within the Continue CTA on the Allow Access to Contacts 02 image, display the How Do You Want to Share image.
    15. When the user clicks anywhere within the Select Contacts CTA on the How Do You Want to Share image, display the Select and Continue image.
    16. When the user clicks anywhere within the Continue CTA on the Select and Continue image, display the Allow Access to 15 image.
    17. When the user clicks anywhere within the Allow Selected Contacts CTA on the Allow Access to 15 image, display the Landing Page.

When the user selects the SMB radio button, do the following things:
    1. Populate the screen of the phone with Zelle SMB Enrollment Carousel Image 001.
    2. When the user clicks the right side of the screen, slide the current image to the left in the device screen while sliding the next image in from the right.
    3. When the user clicks the left side of the screen, slide the current image to the right in the device screen while sliding the next image in from the left.
    4. The order of the screens follows the number at the end of each image name (001, 002, 003, 004).  Then 004 is displaying and the user clicks the right side of the screen, start over with image 001.
    5. Each of those images, 001 thru 004, has a get started button area on them. When the user clicks anywhere within the "GET STARTED" CTA, display the Terms and Conditions image.
    6. When the user clicks the Accept & Continue button on the Terms and Conditions image, display the Choose How to Receive Money 01 SMB image. 
    7. When the user selects one of the two radio buttons on the Choose How to Receive Money 01 SMB image, display the Choose How to Receive Money 02 SMB image immediately without a 300ms slide animation.
    8. When the user clicks the Continue button on the Choose How to Receive Money 02 SMB image, display the Authenticate 03 SMB image.
    9. When the user selects the radio button on the Authenticate 03 SMB image, display the Authenticate 04 SMB image.
    10. When the user selects the Continue button on the Authenticate 04 SMB image, display the Authenticate OTP 01 SMB image.
    11. When the user clicks on the leftmost box of the six boxes on the Authenticate OTP 01 SMB image, display the Authenticate OTP 02 SMB image.
    12. When the user clicks on the Verify button on the Authenticate OTP 02 SMB image, fade the image 50% and display a working icon for 1 second, then display the Create Zelle Tag 01 SMB image.
    13. When the user clicks on the Enter you Zelle tag area on the Create Zelle Tag 01 SMB image, display Create Zelle Tag 02 SMB - Mikes Lawn Care Entered image.
    14. When the user clicks on the Check Availability button on the Create Zelle Tag 02 SMB - Mikes Lawn Care Entered image, fade the image 50% and display a working icon for 1 second, display the Create Zelle Tag 03 SMB - Mikes Lawn Care Not Available image.
    15. When the user clicks on MikesLawnCare on the Create Zelle Tag 03 SMB - Mikes Lawn Care Not Available image, display the Create Zelle Tag 04 SMB - Mikes Dog Walking Entered image.
    16. When the user clicks on the Check Availability button on the Create Zelle Tag 04 SMB - Mikes Dog Walking Entered image, fade the image 50% and display a working icon for 1 second, display the Create Zelle Tag 05 SMB - Mikes Dog Walking Available image.
    17. When the user clicks on the Save and Continue button on the Create Zelle Tag 05 SMB - Mikes Dog Walking Available image, display the Select Primary Account 01 SMB image.
    18. When the user clicks one of the two radio buttons on the Select Primary Account 01 SMB image, display the Select Primary Account 02 SMB image.
    19. When the user clicks Continue on the Select Primary Account 02 SMB image, fade the image 50% and display a working icon for 1 second, display the Congratulations SMB image.
    20. When the user clicks Send Or Request Money on the Congratulations SMB image, display the ZRC Feature Intro image.
    21. When after 5 seconds the user has not clicked the ZRC Feature Intro image, slide the Zelle Tag Feature Introduction SMB image in from the right.
    22. When after 5 seconds the user has not clicked the Zelle Tag Feature Intorduction SMB image, slide the ZRC Feature Intro image in from the right.
    23. Repeat the alternating display of the ZRC Feature Intro and Zelle Tag Feature Introduction SMB SMB images until the user clicks the designated areas on either image.
    24. When the user clicks on either the Skip For Now link on the Zelle Tag Feature Introduction SMB image, or clicks on the ACCESS CONTACTS CTA on the ZRC Feature Intro image, display the Allow Access to Contacts 01 image.   
    25. Continue from step 12. under the P2P workflow.
