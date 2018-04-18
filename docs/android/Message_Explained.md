# [Android Message](https://developer.android.com/reference/android/os/Message.html)
A `Message` contains a description and an arbitrary data object that can be sent to a `Handler`. This object contains two extra int fields and an extra objecct field that allow you to not do allocations in many cases.

_NOTE: While the constructor of `Message` is public, the best way to get one of these is to call `Message.obtain()` or one of the `Handler.obtainMessage()` methods, which will pull them from a pool of recycled objects_.