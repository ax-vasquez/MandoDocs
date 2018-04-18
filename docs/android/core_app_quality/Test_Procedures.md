# Core App Quality Test Procedures
These test procedures are meant to help you discover various types of quality issues in your app.

## Core Suite
These are the tests contained within the Core Suite
1. `CR-0`
    - Navigate to all parts of the app - all screens, dialogs, settings, and all user flows
      - If the application allows for editing or content creation, game play, or media playback, make sure to enter those flows to create or modify content
      - While exercising the app, introduce transient changes in network connectivity, battery function, GPS or location availability, system load, and so on
        - _This is especially important given the dynamic nature of how the Android system kills processes_
2. `CR-1`
    - From each app screen, press the device's Home key, then relaunch the app from the _All Apps_ screen
3. `CR-2`
    - From each app screen, switch to another running app and then return to the app under test using Recents app switcher
4. `CR-3`
    - From each app screen (and dialogs), press the Back button
5. `CR-5`
    - From each app , rotate the device between landscape and portrait orientation at least 3 times
6. `CR-6`
    - Switch to another app to send the test app into the background
    - Go to Settings and check whether the test app has any services running while in the background
      - In Android 4.0 and higher, go to the Apps screen and find the app in the "Running" tab
7. `CR-7`
    - Press the power button to put the device to sleep, then press the power button again to wake the screen
8. `CR-8`
    - Set the device to lock when the power button is pressed
    - Press the power button to put the device to sleep, then press the power button again to wake the screen, then unlock the device
9. `CR-9`
    - For devices that have slide-out keyboards, slide the keyboard in and out at least once
    - For devices that have keyboard docks, attach the device to the keyboard dock
10. `CR-10`
    - For devices that have an external display port, plug in the external display
11. `CR-11`
    - Trigger and observe in the notifications drawer all types of notifications that the app can display
    - Expand notifications where applicable (Android 4.1 and higher), and tap all actions offered

## Install on SD Card
1. `SD-1`
    - Repeat _Core Suite_ with the app installed to a device's SD card (if supported by app)
    - To move the app to SD card, you can use Settings > App Info > Move to SD Card

## Hardware Acceleration
1. `HA-1`
    - Repeat _Core Suite_ with hardware acceleration enabled
    - To force-enable hardware acceleration (where supported by the device)
      - Add `hardware-accelerated="true"` to the `<application>` in the app manifest and recompile

## Performance and Stability
1. `SP-1`
    - Review the Android manifest file and build configuration to ensure that the application is built against the [latest available SDK](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels) (`targetSdk` and `compileSdk`)

## Battery
1. `BA-1`
    - Repeat _Core Suite_ across Doze and App Standby cycles
    - Pay close attention to alarms, timers, notifications, syncs and so on
    - See [Testing with Doze and App Standby for requirements and guidelines](https://developer.android.com/training/monitoring-device-state/doze-standby.html#testing_doze_and_app_standby)

## Security
1. `SC-1`
    - Review all data stored in external storage
2. `SC-2`
    - Review how data loaded from external storage is handled and processed
3. `SC-3`
    - Review all content providers defined in the Android manifest file for appropriate `protectionLevel`
4. `SC-4`
    - Review all permissions that your app requires, in the manifest file, at runtime and in the app settings (Settings > App Info) on the device
5. `SC-5`
    - Review all [application components](https://developer.android.com/guide/topics/manifest/application-element.html) defined in the Android manifest file for the appropriate export state
    - The export property must be set explicitly for all components
6. `SC-6`
    - Review the app's [Network Security configuration](https://developer.android.com/training/articles/security-config.html), ensuring that no lint checks on the configuration fail
7. `SC-7`
    - For each WebView, navigate to a page that requires JavaScript
8. `SC-8`
    - For each WebView, attempt to navigate to sites and content that are out of your control
9. `SC-9`
    - Declare a Network Security Configuration that [disables cleartext traffic](https://developer.android.com/training/articles/security-config.html#CleartextTrafficPermitted) then execute the app
10. `SC-10`
    - Run the application and exercise all core functionality, while [observing the device log](https://developer.android.com/studio/command-line/logcat.html)
    - _No private user information should ever be logged_

## Google Play
1. `GP-1`
    - Sign into the Google Play Developer Console to review your developer profile, app description, screenshots, feature graphic, content rating and user feedback
2. `GP-2`
    - Download your feature graphic and screenshots and scale them down to match the display sizes on the devices and form factors you are targeting
3. `GP-3`
    - Review all graphical assets, media, text, code libraries, and other content packaged in the app or expansion file download
4. `GP-4`
    - Review [Support for other use cases](https://developer.android.com/training/monitoring-device-state/doze-standby.html#other_use_cases) in Doze and App Standby

## Payments
1. `GP-4` (yes, really)
    - Navigate to all screens of your app and enter all in-app purchase flows