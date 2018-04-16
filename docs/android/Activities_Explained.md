# [Android Activities Explained](https://developer.android.com/reference/android/app/Activity.html)
An activity is a _single, focused thing that the user can do_.
- Almost all activities interact with the user
- There are two methods almost all subclasses of Activity will implement:
  - `onCreate( Bundle )`
  - `onPause()`

**_IMPORTANT: To be of use with `Context.startActivity()`, all activity classes must have a corresponding `<activity>` declaration in their package's `AndroidManifest.xml`_**

## Fragments
The `FragmentActivity` subclass can make use of the `Fragment` class to:
- Better modularize their code
- Build more sophisticated user interfaces for larger screens
- Help scale application between small and large screens

For more information about using fragments, read the [Fragments](https://developer.android.com/guide/components/fragments.html) developer guide.

## Activity Lifecycle
Activities in the system are managed as an _activity stack_
- When a new activity is started, it is placed on TOP of the stack and becomes the running activity
- The previous activity always remains below the new activity in the stack
  - The previous activity will not come to the foreground again until the new activity exits

#### The 4 States of an Activity
1. **_Active/Running_**
    - State when the activity is in the foreground (at the top of the stack)
2. **_Paused_**
    - State when the activity has _lost focus, but is still visible_
      - Occurs when a new non-full-sized or transparent activity has focus on top of your activity
    - Activity is still alive in this state
      - It _may_ be killed by the system in extreme low memory situations (Pre-Honeycomb)
3. **_Stopped_**
    - State when an activity is completely obscured by another activity
    - _It still retains all state and member information_
      - It is simply no longer visible to the user so its window is hidden
      - Stopped activities will often be killed by the system when memory is needed elsewhere
4. **_Killed/Terminated_**
    - An activity that has been paused or stopped may be dropped from memory by the system by either:
      - Asking the activity to finish
      - Simply killing the process
    - _When displayed again to the user, a killed/terminated activity must be completely restarted and restored to its previous state_

#### 3 Key Loops to Monitor Within Activities
1. **_Entire Lifetime_**
    - Happens between the _first call to `onCreate( Bundle )`_ to a _single final call to `onDestroy()`_
    - All setup of an Activity's "global" state happens in `onCreate()`
    - Release of all remaining resources happens in `onDestroy()`
2. **_Visible Lifetime_**
    - Happens between _a call to `onStart()`_ until a _corresponding call to `onStop()`_
    - _It is during this time that the user can see the activity on-screen_
      - Though, it may not be in the foreground and interacting with the user (e.g. "paused" state)
3. **_Foreground Lifetime_**
    - Happens between _a call to `onResume()`_ until a _corresponding call to `onPause()`_
    - _It is during this time that the activity is in front of ALL other activities and interacting with the user_
    - Activities can frequently go between the resumed and paused states

## Configuration Changes
If the configuration of a device changes (as defined by the `Resources.Configuration` class), then anything displaying a user interface will need to update to match that configuration.

_Unless you specify otherwise_, a configuration change (such as change in screen orientation, language, input devices, etc) **will cause your activity to be destroyed**, going through the normal activity lifecycle process of `onPause()`, `onStop()`, and `onDestroy()`.
- If the activity had been in the foreground or visible to the user, once `onDestroy()` is called in that instance, a new instance of the activity will be created
  - This is done because the only safe way to handle a configuration change is to re-retrieve all resources, including:
    - Layouts
    - Drawables
    - Strings

#### Bypassing Activity Restart
In some special cases, you may want to bypass restarting the activity based on one or more types of configuration changes.
- This is done with the `android::configChanges` attribute in its manifest
- _If a configuration change involves any that you do not handle, the activity will still be restarted_

## Starting Activities and Getting Results
`startActivity( Intent )` method is used to start a new activity, which will be placed at the top of the stack.
- Takes a single argument `Intent` object, which describes the activity to be executed

Sometimes you want to get a result back from an activity when it ends
- To do this, call the `startActivityForResult(Intent, int)` version with a second integer parameter identifying the call
- The result will come back through your `onActivityResult(int, int, Intent)` method

## Saving Persistent State
There are generally two types of persistent state that an activity will deal with:
1. _Shared, Document-Like Data_
    - Typically stored in a SQLite database using a [content provider](https://developer.android.com/reference/android/content/ContentProvider.html)
    - Google suggests an "edit in place" model for this kind of data 
      - In other words, edits are made immediately (without an additional confirmation step)
      - This means that the act of the user "backing out" of an activity _should persist all changes without any user interaction_
        - If you need "caneling" functionality, you must implement this explicitly via a "revert" or "undo" method
2. _Internal State_
    - Something like user preferences

Activity persistent state is managed with the method `getPreferences(int)`, allowing you to retrieve and modify a set of name/value pairs associated with the activity.
- To use preferences that are shared across 

## Permissions
The ability to start a particular Activity can be enforced when its declares in its manifest's `<activity>` tag. _By doing do, other applications will need to declare a corresponding `<uses-permission>` element in their own manifest to be able to start that activity_.

## Proccess Lifecycle
The Android system attempts to keep the application process around for as long as possible, _but will eventually need to remove old processes when memory runs low_. There are 4 states a process can be in based on the activities running in it, **listed in order of importance**
1. _**Foreground Activity**_
    - _Most-important activity_
    - Activity at the top of the stack that the user is currently interacting with
    - This process is only ever killed as a last resort (e.g. something bad must've happened to get to this point)
      - Generally at this point the device has reached a memory paging state
      - This is required to keep the user interface responsive
2. _**Visible Activity**_
    - An acticvity that is visible to the user, but is not in the foreground, such as one sitting behind a dialog
    - Considered extremely important to the app's task stack
      - As such, this will only be killed _if doing so is required to keep the foreground activity running_
3. _**Background Activity**_
    - An activity that _is not_ visible to the user and has been paused
    - The system may safely kill a background activity to reclaim memory for other foreground or visible processes
    - If a background activity was killed by the system and the user navigates back to said activity
      - That activity's `onCreate(Bundle)` method will be called with the savedInstanceState it had previously supplied in `onSaveInstanceState( Bundle )`
      - This allows the activity to restart itself in the same state as the user last left it
4. _**Empty Process**_
    - A process hosting no activities or other application components (such as `Service` or `BroadcastReceiver` classes)
    - These processes are _killed very quickly by the system as memory becomes low_
    - For this reason, _any background operation you do outside of an activity **must be executed in the context of an activity BroadcastReceiver or Service** to ensure that the system knows in needs to keep your process around_

#### Long-Running, Independent Tasks
Sometimes an Activity may need to do a long-running operation that exists independently of the activity lifecycle itself. To do this, your Activity should start a Service in which the process takes place. This allows the system to properly prioritize your process (considering it to be more important than other non-visible applications) for the duration of the process, _independent of whether the original activity is paused, stopped or finished_.