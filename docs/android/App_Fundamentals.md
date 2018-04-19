# [App Fundamentals](https://developer.android.com/guide/components/fundamentals.html)
- Android apps can be written using:
  - Kotlin
  - Java
  - C++
- The Android SDK tools compile your code (along with any data resource files) into an APK
  - An _Android Package_
  - This file is an archive file with a `.apk` suffix
- An APK contains all the contents of an Android app 
  - APKs are the files that Android-powered devices use to install the app

## App Security Features
Each Android app _lives in its own security sandbox_, protected by the following Android security features:
- The Android operating system is a multi-user Linux system in which _each app is a different user_
- By default, the system assigns each app a unique Linux user ID
  - The ID is used only by the system and is unknown to the app
  - The system sets permissions for all the files in an app so that only the user ID assigned to that app can access them
- Each process has its own virtual machine (VM)
  - This means an app's code always runs in isolation from other apps
- By default, every app runs in its own Linux process
  - The Android system starts the process when any of the app's components need to be executed
  - The process is shut down when it's no longer needed (_or when the system must recover memory for other apps_)

The Android system implements the _principle of least privilege_. Apps should only ever have access to the bare-minimum permissions needed to perform their operations.

### Data Sharing and System Service Access
The following are ways to share data with other apps and for an app to access system services:
- It's possible to arrange for two apps to share the same Linux user ID
  - This enables them to have access to each other's files
  - To conserve system resources, apps with the same user ID can also arrange to run in the same Linux process and share the same VM
  - _The apps must also be signed with the same certificate_
- An app can request permission to access device data such as the user's contacts, SMS messages, mountable storage, camera and Bluetooth
  - _The user has to explicitly grant these permissions_
  - For more information, see [Working with System Permissions](https://developer.android.com/training/permissions/index.html)

## App Components
App components are _the essential building blocks of an Android app_. Each component _is an entry point_ through which the system or a user can enter your app. Some components depend on others.

There are 4 different types of app components:
1. **Activities**
    - Definition:
      - _The entry point for interacting with the user_
    - Responsibilities:
      - Facilitates the following key interactions between system and app:
        - Keeping track of what the user currently cares about (what is on screen) to ensure the system keeps running the process that is hosting the activity
        - Knowing that previously-used processes contain things the user may return to (stopped activities), and thus more highly prioritize keeping those processes around
        - Helping the app handle having its process killed so the user can return to activities with their previous state restored
        - Providing a way for apps to implement user flows between each other, and for the system to coordinate these flows
    - Notes:
      - It represents a single screen with a user interface
      - Activities are independent of one another
        - They should be designed so that they can be "stacked" in any number of ways that the app allows
      - You implement an activity as a subclass of the [`Activity`](https://developer.android.com/reference/android/app/Activity.html) class
        - For more information on the `Activity` class, see the [Activities](https://developer.android.com/guide/components/activities.html) developer
2. **Services**
    - Definition:
      - _A general-purpose entry point for keeping an app running in the background for all kinds of reasons_
    - Responsibilities:
      - It is a component that runs in the background to perform long-running operations
        - Or to perform work for remote processes
    - Notes:
      - A service does not provide a user interface
      - There are two very distinct semantics services tell the system about how to manage an app:
        - Started services _tell the system to keep them running until their work is completed_
      - Syncing data in the background or playing music represent two different types of started services that modify how the system handles them:
        - Music playback is something the user is directly aware of
          - The app tells the system this by saying it wants to be foreground with a notification to tell the user about it
          - This lets the system know that it should try really hard to keep this process running since the user will be unhappy if it goes away
        - A background service is one the user is not directly aware of
          - This gives the system more freedom in managing its process
3. **Broadcast Receivers**
4. **Content Providers**