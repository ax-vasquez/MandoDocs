# Android AsyncTask
AsyncTask enables proper and easy use of the UI thread. It allows you to perform background operations and publish results on the UI thread without having to manipulate threads and/or handlers.
- AsyncTask is designed to be a helper class around `Thread` and `Handler`
  - It _does not_ constitute a generic threading framework

## The 3 Types That Define Asynchronous Tasks
An "asynchronous task" is defined by a computation that runs on a background thread and whose result is published on the UI thread. An Asynchronous task is defined by 3 generic types:
1. `Params`
    - The types of _parameters sent_ to the task upon execution
2. `Progress`
    - The type of the _progress units published_ during the background computation
3. `Result`
    - The type of the _result_ of the background computation

_Not all types are always used by an asynchronous task_. To mark a type as unused, simpuly use the type `Void`:
```java
private class MyTask extends AsyncTask<Void, Void, Void> { ... }
```

## The 4 Steps That Define Asynchronous Tasks
In addition to being defined by the three types stated above, an asynchronous task is further defined by the following 4 steps:
1. `onPreExecute()`
    - Invoked on theUI thread before the task is executed
    - This step is normally used to setup the task
      - For instance, by showing a progress bar in the user interface
2. `doInBackground( Params ... )`
    - Invoked on the background thread immediately after `onPreExecute()` finishes executing
    - This step is used to perform background computation that can take a long time
    - The parameters _of the asynchronous task_ are passed to this step
    - The result of this computation must be returned by this step
      - The result will be passed back to the last step
    - This step can also use `publishProgress( Progress...)` to publish one or more units of progress
      - These values are published on the UI thread, in the `onProgressUpdate( Progress ... )` step
3. `onProgressUpdate( Progress ... )`
    - Invoked on the UI thread after a call to `publishProgress( Progress ... )`
      - _The timing of the execution is undefined_
    - This method is used to display any form of progress in the user interface while the background computation is still executing
      - For instance, it can be used to animate a progress bar or show logs in a text field
4. `onPostExecute( Result )`
    - Invoked on the UI thread after the background computation finishes
    - The result of the background computation is _passed to this step_ as a parameter

## Cancelling a Task
- A task can be cancelled at any time by invoking `cancel( boolean )`
  - Invoking this method will cause subsequent calls to `isCancelled()` to return true
- After invoking this method, `onCancelled( Object )`, instead of `onPostExecute( Object )` will be invoked ater `doInBackground( Object[] )` returns
- To ensure that a task is cancelled as quickly as possible:
  - Always check the return value of `isCancelled()` periodically from `doInBackground( Object[] )`, if possible
    - For instance, inside a loop

## Threading Rules
In order for AsyncTask to work properly, you must follow these threading rules:
1. _The **`AsyncTask` class** must be **loaded onto the UI thread**_
    - This is done automatically as of `JELLY_BEAN` ([Android 4.1 - API Level 16](https://developer.android.com/about/versions/android-4.1.html))
2. _The **task instance** must be **created on the UI thread**_
3. _**`execute( Params... )`** must be **invoked on the UI thread**_
4. _**Do not** call any of the following **manually**_:
    - `onPreExecute()`
    - `onPostExecute( Result )`
    - `doInBackground( Params... )`
    - `onProgressUpdate( Progress... )`
5. _The **task represented by a given `AsyncTask`** can be executed **only once**_
    - An exception will be thrown if a second execution is attempted

## Memory Observability
`AsyncTask` guarantees that all callback calls are synchronized in such a way that the following operations are sage without explicit synchronizations:
- Set member fields in the constructor or `onPreExecute()` and refer to them in `doInBackground( Params... )`
- Set member fields `doInBackground( Params... )` and refer to them in `onProgressUpdate( Progress... )` and `onPostExecute( Result )`

