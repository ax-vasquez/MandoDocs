# [Android Processes and Threads](https://developer.android.com/guide/components/processes-and-threads.html)
By default, all components of the same application run in the same process and thread (called the "main" thread). If an application component starts and there already exists a process for that application, then the component is started within that process and uses the same thread of execution. _However, you can arrange for different components in your application to run in separate processes, and you can create additional threads for any process._

## Processes
By default, all components of the same application run in the same process, _and most applications should not change this_. However, if you find that you need to control which process a certain component belongs to, you can do so in the manifest file.

The manifest entry for each type of component element ( `<activity>`, `<service>`, `<receiver>` and `<provider>`) supports an `android::process` attribute that can specify a process in which that a component should run. 
- You can set this attribute so that each component runs in its own process or so that some components share a process while others do not

#### Some Notes on Processes
- When memory is required, Android may decide to shut down a process
  - Application components running in the process that's killed are consequently destroyed
  - A process is started again for those components when there is again work for those components
- The decision on when a process is killed depends on the importance of the process to the user
  - It's entirely dependent on the state of the components running in that process

You can learn more in [Processes and Application Lifecycle](https://developer.android.com/guide/topics/processes/process-lifecycle.html)

## Threads
When an application is launched, the syste, creates a thread of execution for the application, called "main"
- The "main" thread is a very important thread
  - It is in charge of dispatching events to the appropriate user interface widgets, including drawing events
  - _It is almost-always the thread in which your application interacts with components from the Android UI toolkit_
    - As such, the main thread is also sometimes called the UI thread
    - There are some special circumstances - for more information, see [Thread annotations]
- The system _does not_ create a separate thread for each instance of a component
  - All components that run in the same processes are instantiated in the UI thread
    - System calls to each component are dispatched from that thread

#### The 2 Rules of Android Single-Thread Model
1. **Do not block the UI thread**
2. **Do not access the Android UI toolkit from outside the UI thread**

## Worker Threads
Because of the single-threaded model of Android, it's imperative to your app performance that you _do not block the UI thread_. Non-instantaneous operations should be made to do their work in separate threads ("background" or "worker" threads). **However**, you cannot update the UI from any thread other than the UI thread ("main thread").
- To get around the issue of being unable to update the UI from any thread other than the UI thread:
  - Android offers several ways to access the UI thread from other threads:
    - [`Activity.runOnUiThread( Runnable )`](https://developer.android.com/reference/android/app/Activity.html#runOnUiThread(java.lang.Runnable))
    - [`View.post( Runnable )`](https://developer.android.com/reference/android/view/View.html#post(java.lang.Runnable))
    - [`View.postDelayed( Runnable, long )`](https://developer.android.com/reference/android/view/View.html#postDelayed(java.lang.Runnable,%20long))

#### Using AsyncTask
`AsyncTask` allows you to perform asynchronous work on your user interface. It _performs the blocking operations in a worker thread and then publishes the results on the UI thread, without requiring you to handle threads and/or handlers yourself_.

To learn more on how to use this class, refer to [AsyncTask](https://developer.android.com/reference/android/os/AsyncTask.html)

## Thread-Safe Methods
Some methods need to be called from more than one thread - such methods must be _thread-safe_. This is most important for methods that can be called remotely.

## Interprocess Communication
Android offers a mechanism for interprocess communication (IPC) using remote procedure calls (RPC), in which a method is called by an activity or other application component, but executed remotely (in another message), with any result returned back to the caller.