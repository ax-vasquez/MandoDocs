# [Android Handler](https://developer.android.com/reference/android/os/Handler.html)
- A Handler allows you to send and process `Message` and Runnable objects associated with a thread's `MessageQueue`.
- Each Handler instance is associated with a single thread and that thread's message queue
  - _When you create a new Handler, **it is bound to the thread/message queue of the thread that is creating it**_
  - From that point on, it will deliver messages and runnables to that message queue and execute them as they come out of the message queue

## The Two Main Uses of a Handler
1. _To schedule messages and runnables to be executed at some point in the future_
2. _To enqueue an action to be performed on a different thread than your own_

## Scheduling Messages
You can schedule messages with the following methods:
1. `post( Runnable )`
2. `postAtTime( Runnable, long )`
3. `postDelayed( Runnable, Object, long )`
4. `sendEmptyMessage( int )`
5. `sendMessage( Message )`
6. `sendMessageAtTime( Message, long )`
7. `sendMessageDelayed( Message, long )`

**Notes**
- The _post_ versions of these methods allow you to enqueue Runnable objects to be called by the message queue when they are received
- The _sendMessage_ versions allow you to enqueue a `Message` object containing a bundle of data that will be processed by the Handler's `handleMessage( Message )` method
  - This requires that you implement a subclass of Handler
- When posting or sending to a Handler, you can either:
  - _Allow the item to be processed as soon as the message queue is ready to do so, or..._
  - _Specify a delay before it gets processed (or absolute time for it to be processed)_
- When a process is created for your application, its main thread is dedicated to running a message queue that takes care of managing the top-level application objects
  - Activities
  - Broadcast receivers
  - etc.
- You can create your own threads, and communicate back with the main application through a Handler
  - This is done by calling the same _post_ or _sendMessage_ methods as before, but from your new thread
  - The given Runnable or Message will then be scheduled in the Handler's message queue and processed when appropriate