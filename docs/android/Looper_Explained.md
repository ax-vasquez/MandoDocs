# [Android Looper](https://developer.android.com/reference/android/os/Looper.html)
Class used to run a message loop for a thread
- Threads by default do not have a message loop associated with them
  - To create one, called `prepare()` in the thread that is to run the loop
  - Then, `loop()` to have it process messages until the loop is stopped
- Most interaction with a message loop is through the [Handler](https://developer.android.com/reference/android/os/Handler.html) class