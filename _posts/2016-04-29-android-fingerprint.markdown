---
layout:     post
title:      "android Fingerprint Authentication"
subtitle:   "Fingerprint Authentication"
date:        2016-04-28 09:00:00
author:     ""
header-img: "img/post-bg-2015.jpg"
tags:
   - android
---


 To authenticate users via fingerprint scan, get an instance of the new FingerprintManager class and call the authenticate() method. Your app must be running on a compatible device with a fingerprint sensor. You must implement the user interface for the fingerprint authentication flow on your app, and use the standard Android fingerprint icon in your UI. The Android fingerprint icon (c_fp_40px.png) is included in the Fingerprint Dialog sample. If you are developing multiple apps that use fingerprint authentication, note that each app must authenticate the user’s fingerprint independently.

To use this feature in your app, first add the USE_FINGERPRINT permission in your manifest.

    <uses-permission
            android:name="android.permission.USE_FINGERPRINT" />

To see an app implementation of fingerprint authentication, refer to the Fingerprint Dialog sample. For a demonstration of how you can use these authentication APIs in conjunction with other Android APIs, see the video Fingerprint and Payment APIs.

If you are testing this feature, follow these steps:

Install Android SDK Tools Revision 24.3, if you have not done so.
Enroll a new fingerprint in the emulator by going to Settings > Security > Fingerprint, then follow the enrollment instructions.
Use an emulator to emulate fingerprint touch events with the following command. Use the same command to emulate fingerprint touch events on the lockscreen or in your app.

    adb -e emu finger touch <finger_id>

 On Windows, you may have to run telnet 127.0.0.1 <emulator-id> followed by finger touch <finger_id>.

![](/img/fingerprint.jpg)


Confirm Credential
Your app can authenticate users based on how recently they last unlocked their device. This feature frees users from having to remember additional app-specific passwords, and avoids the need for you to implement your own authentication user interface. Your app should use this feature in conjunction with a public or secret key implementation for user authentication.

To set the timeout duration for which the same key can be re-used after a user is successfully authenticated, call the new setUserAuthenticationValidityDurationSeconds() method when you set up a KeyGenerator or KeyPairGenerator.

Avoid showing the re-authentication dialog excessively -- your apps should try using the cryptographic object first and if the the timeout expires, use the createConfirmDeviceCredentialIntent() method to re-authenticate the user within your app.

To see an app implementation of this feature, refer to the Confirm Credential sample.

