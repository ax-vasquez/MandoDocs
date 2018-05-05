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
        - _Bound_ services run because some other app (or the system) has said that it wants to make use of the service
          - This is basically the service providing an API to another process
        - A service should be implemented as a subclass of `Service`
          - For more information on the `Service` class, see the [Services](https://developer.android.com/guide/components/services.html) developer guide
        - NOTE: If your app targets Android 5.0 or later...
          - Use the [`JobScheduler`](https://developer.android.com/reference/android/app/job/JobScheduler.html) class to scheduled actions
          - It has the advantage of conserving battery by optimally scheduling jobs to reduce power consumption
            - And by working with the [Doze](https://developer.android.com/training/monitoring-device-state/doze-standby.html) API
3. **Broadcast Receivers**
    - Definition:
      - _A component that enables the system to deliver events to the app outside of a regular user flow, allowing the app to respond to system-wide broadcast announcements_
    - Notes:
      - Broadcast receivers are a well-defined entry into the app
        - This means the system can deliver broadcasts even to apps that aren't currently running
      - Many broadcasts originate from the system:
        - Signal that the screen has turned off
        - Signal that the battery is low
        - Signal that a picture was captured
        - Etc.
      - Although Broadcast receivers don't display a user interface, they may [create a status bar notification](https://developer.android.com/guide/topics/ui/notifiers/notifications.html) to alert the user when a broadcast event occurs
4. **Content Providers**
    - Definition:
      - _A Content Provider manages a shared set of app data that you can store in the file system, in a SQLite database, on the web, or on any other persistent storage location that your app can access_
    - Notes:
      - Through the content provider, other apps can query or modify the data if the content provider allows it
      - To the system, a content provider is an entry point into an app for publishing named data items, identified by a URI scheme
        - Assigning a URI doesn't require the app to be running
          - So, URIs can persist after their owning apps have exited
          - The system only needs to make sure that an owning app is still running when it has to retrieve the app's data from the corresponding URI
        - These URIs also provide an important fine-grained security model
      - Content Providers are also useful for reading and writing data that is private to your app and not shared
      - A Content Provider is implemented as a subclass of `ContentProvider` and must implement a standard set of APIs that enable other apps to perform transactions
        - For more information, see the [Content Providers](https://developer.android.com/guide/topics/providers/content-providers.html) developer guide
      - A unique aspect to the Android system design is that _any app can start another app's component_
        - Instead of having to "do it all yourself", consider integrating existing functionality from other apps to facilitate development/functionality
        - To the user, any activity to attach to from within your app will appear to be part of your app (if implemented properly)
      - Unlike apps on most other systems, _Android apps don't have a single entry point_ (there is no `main()`)
      - Because the system runs each app in a separate process with file permissions that restrict access to other apps, your app _cannot directly activate a component from another app_
        - However, the Android system can
        - To activate a component in another app:
          - Deliver a message to the system that specifies your _intent_ to start a particular component
          - The system then activates the component for you

## Activating Components
- Three of the four component types are _activated by an asynchronous message called an intent_
  - _Activities_
  - _Services_
  - _Broadcast Receivers_
- Intents bind individual components to each other at runtime
  - You can think of them as the messengers that require an action from other components (regardless of whether the component belongs to your app)
- An intent is created with an [`Intent`](https://developer.android.com/reference/android/content/Intent.html) object, which _defines a message to activate either a specific component (explicit intent), or a specific type of component (implicit intent)_
  - For Activities and Services, an intent _defines the action to perform_
    - It may specify the URI of the data to act on, and anything else the component being started might need to know
  - For Broadcast Receivers, the intent _simply defines the announcement being broadcast_
    - For example, a broadcast to indicate the device battery is low includes only a known action string that indicates _battery is low_
- **Content Providers are not activated by intents**
  - Instead, they are activated when targeted by a request from a [`ContentResolver`](https://developer.android.com/reference/android/content/Intent.html)
  - The `ContentResolver` handles all direct transactions with the content provider so that the component that's performing transactions with the provider doesn't need to and instead calls methods on the `ContentResolver`
  - This leaves a layer of abstractions between the content provider and the component requesting information (for security)

There are separate methods for activating each type of component:
- You can start an activity or give it something new to do by passing an `Intent` to `startActivity()` or `startActivityForResult()`
- With Android 5.0 and later, you can use `JobScheduler` class to schedule actions
  - For earlier versions, you can start a service (or give new instructions to an ongoing service) by passing an `Intent` to `startService()`
  - You can bind to the service by passing an `Intent` to `bindService()`
- You can initiate a broadcast by passing an `Intent` to methods such as `sendBroadcast()`, `sendOrderedBroadcast()`, or `sendStickyBroadcast()`
- You can perform a query to content provider by calling `query()` on a `ContentResolver`

For more information about using intents, see the [Intents and Intent Filters](https://developer.android.com/guide/components/intents-filters.html) document

## The manifest File
Before the Android system can start an app component, the system must know that the component exists by reading the app's _manifest file_, `AndroidManifest.xml`.
- Your app _must declare all its components in this file_, which must be at the root of the app project directory
- The manifest does a number of things in addition to declaring the app's components:
  - Identifies any user permissions the app required, such as Internet access or read-access to the user's contacts
  - Declares the minimum [API Level](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels) required by the app
  - Declares hardware and software features used or required by the app, such as camera, bluetooth services, or a multitouch screen
  - Declares API libraries the app needs to be linked against (other than the Android framework APIs), such as the [Google Maps library](http://code.google.com/android/add-ons/google-apis/maps-overview.html)

### Declaring Components
The primary task of the manifest is to inform the system about the app's components. For example, a manifest file can declare an activity as follows:
```java
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <application android:icon="@drawable/app_icon.png" ... >
        <activity android:name="com.example.project.ExampleActivity"
                  android:label="@string/example_label" ... >
        </activity>
        ...
    </application>
</manifest>
```
- In the `<application>` element, the `android:icon` attribute points to resources for an icon that identifies the app
- In the `<activity>` element, the `android:name` attribute specifies the fully qualified class name of the `Activity` subclass and the `android:label` attribute specifies as string to use as the user-visible label for the activity
- **You must declare all app components using the following elements:**
  - _`<activity>`_
    - Elements for activities
  - _`<service>`_
    - Elements for services
  - _`<receiver>`_
    - Elements for broadcast receivers
  - _`<provider>`_
    - Elements for content providers

Activities, services and content providers that you include in your source, but do not declare in the manifest, _are not visible to the system and can never run_. Broadcast Receiver can be either declared in the manifest or created dynamically in code as `BroadcastReceiver` objects and registered with the system by calling `registerReceiver()`.

For more about how to structure the manifest file for your app, see [The AndroidManifest.xml File](https://developer.android.com/guide/topics/manifest/manifest-intro.html) documentation

### Declaring Component Capabilities
The system identifies components that can respond to an intent by comparing the intent received to the _intent filters_ provided in the manifest file of other apps on the device.
- When you declare an activity in your app's manifest file, you can optionally include intent filters that declare the capabilities of the activity so it can respond to intents from other apps
  - You declare an intent filter for your component by adding an `<intent-filter>` element as a child of the component's declaration element.

For example, if you build an email app with an activity for composing a new email, you can declare an intent filter to respond to "send" intents (in order to send a new email), as shown below:
```java
<manifest ... >
    ...
    <application ... >
        <activity android:name="com.example.project.ComposeEmailActivity">
            <intent-filter>
                <action android:name="android.intent.action.SEND" />
                <data android:type="*/*" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```

For more information about creating intent filters, see the [Intents and Intent Filters](https://developer.android.com/guide/components/intents-filters.html) document

### Declaring App Requirements
Not all Android powered devices are the same. To prevent your app from being installed on devices that lack features needed by your app, it's important that you _clearly-define a profile for the types of devices your app supports_.
- This is done by declaring device and software requirements in your manifest file
  - Most of these declarations are informational only and are not used by the system
  - They are, however, used by external services such as Google Play, in order to provide filtering for users when they search for apps from their device

For example, if your app requires a camera and uses APIs introduces in Android 2.1, you must declare these as requirements in your manifest file as shown below:
```java
<manifest ... >
    <uses-feature android:name="android.hardware.camera.any"
                  android:required="true" />
    <uses-sdk android:minSdkVersion="7" android:targetSdkVersion="19" />
    ...
</manifest>
```
- From the snippet above:
  - Devices that _do not_ have a camera cannot install the app
  - Devices that have an Android version lower than 2.1 also cannot install the app
- You can also declare that your app _uses_ a feature, but does not require it (by setting `required` attribute to `false`)
  - Devices that do not have a camera would be able to install this app, but would not be able to use the features related to the camera

For more information about managing app compatibility with different devices, see the [Device Compatibility](https://developer.android.com/guide/practices/compatibility.html) document

## App Resources
- Android apps require resources that are separate from the source code such as:
  - Images
  - Audio Files
  - And anything related to the visual presentation of your app
  - For example, you can define animations, menus, styles, colors and the layout of activity user interfaces with XML files
- Using app resources makes it easy to update various characteristics of your app _without modifying code_
  - Providing sets of alternative resources enables you to optimize your app for a variety of device configurations
    - Such as different languages and screen sizes

For every resource that you include in your Android project, the SDK build tools define a unique integer ID. This can be used to reference the resourcce from your app code, or from other resources defined in the XML.

One of the most important aspects of providing resources separate from your source code is the ability to provide alternative resources for different device configurations. For example, by defining UI strings in XML, you can translate the strings into other languages and save those strings in separate files. Then, Android applies the appropriate language strings to your UI based on a language _qualifier_ that you append to the resource directory's name and the user's language settings.

For more information on providing resources to your application, see the [Providing Resources](https://developer.android.com/guide/topics/resources/providing-resources.html) documentation.

To learn more about best practices and designing robust, production-quality apps, see [Guide to App Architecture](https://developer.android.com/topic/libraries/architecture/guide.html).