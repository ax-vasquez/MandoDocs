# [RxJava2 for Android](https://techbeacon.com/missing-rxjava-2-guide-supercharge-your-android-development)
First of all **_RxJava2 is very different from RxJava1_** - when looking through reference material, make sure the material is for the correct version of RxJava. (NOTE: You will also need to use [RxAndroid](https://github.com/ReactiveX/RxAndroid))

## Asynchronous World of Android
- Android is asynchronous in many ways
  - If you need to read/write data to the disk, you must wait some time for the operation to complete
    - The situation is similar when making network requests
    - Network call _is not synchronous_
- Interestingly, _the whole user input mechanism is asynchronous in nature_
  - The device is continually waiting for some kind of input

## Thinking Reactively
- Almost everything in the Android world is asynchronous
  - Database transactions
  - File operations
  - Network calls
  - User input
  - Notifications
  - etc.
- Apps are better when designed in a _reactive_ manner rather than a synchronous manner
  - Think of app as a collection of observers that independently react to specific events in concert to create the global app experience
- Reactive coding _is not difficult_, just very different from traditional coding
  - If it still seems difficult, then you still haven't gotten it down - code is supposed to make life _easier_
- **Focus on understanding the core concepts and fundamentals first**
  - When you get stuck, remind yourself that you might need to change the way you are thinking or approaching the problem

## Benefits of Reactive
1. _Exceptional Multi-threaded Performance_
    - Reactive makes multi-threaded development significantly easier to do than it is in a traditional Android app environment
2. _Avoids "Callback Hell"_
    - Since reactive components _are not dependent on the output of the asynchronous operations of other components_, you avoid getting into the dreaded "callback hell"
    - Reactive Extensions prevents callback hell by forcing a design pattern that ensures components operate independently of one another - "independently reactive"
3. _Error Handling is Easier_
    - RxJava2 offers a variety of [robust error-handling methods](https://github.com/ReactiveX/RxJava/wiki/Error-Handling-Operators)
4. _Powerful Operators_
    - RxJava2 offers a lot of well-tested and powerful [operators](https://github.com/ReactiveX/RxJava/wiki/Alphabetical-List-of-Observable-Operators)
5. _Uses Less Code_
    - Code is more readable in RxJava2 and there is significantly less code in reactive applications
    - Helps make much fewer mistakes and leads to more stable code
6. _Cross-Platform_
    - Reactive programming is simply a paradigm that is available across multiple languages:
      - [Java](https://github.com/ReactiveX/RxJava)
      - [JavaScript](https://github.com/Reactive-Extensions/RxJS)
      - [Python](https://github.com/Reactive-Extensions/RxPy)
      - [Swift](https://github.com/ReactiveX/RxSwift)
      - [Kotlin](https://github.com/ReactiveX/RxKotlin)
      - [C++](https://github.com/Reactive-Extensions/RxCpp)
      - And more

# RxJava
In simplest terms, reactive programming is a paradigm where the _consumer_ **reacts** to data as it comes in. It's billed as "the observer pattern _done right_". As a reminder, this guide is written using RxJava2.

## Core Concepts (The "3 O's")
In the reactive world, _consider everything as a stream_. This is not new - it's just the [observer design pattern](https://en.wikipedia.org/wiki/Observer_pattern) in action, but much more powerful. The following are the core concepts of Reactive Extensions:
1. **Observables**
    - Source of some data stream (arbitrary data type)
2. **Observers**
    - Anything that is interested in an observable and subscribes to it can be called an _observer_
    - When the source observable emits some item, _all subscribed observers are notified about it_
    - There are three types of events that an observer can receive:
      - `onNext(T)`
        - Invoked when the source observable emits a particular item of type `T` (arbitrary type)
        - It will be called multiple times if the source observable has multiple items to emit
      - `onCompleted()`
        - Invoked when the source observable completes emitting all of its items
        - _This is a **terminal event**_
          - A _**terminal event**_ is one that, once it occurs, there can be no more items emitted from the source observable
        - This simply indicates that the observable has completed successfully
      - `onError( Throwable )`
        - Invoked when the source observable encounters an error while emitting items
        - _This is also a terminal event_
        - This indicates the observable has failed
          - It provides a `Throwable` which contains details of the error
3. **Operators**
    - This is where the real power of RxJava comes from
    - Operators are pure functions that transform or modify observable streams
    - In the real-world example below
      - Suppose a new speaker comes up and starts speaking in German, but attendees only speak English
      - There would need to be a translator to translate the German into English for the audience
        - The translation task would be the operator in this example

#### Real-World Example
Suppose you attend a conference where the speaker is someone you are interested in hearing. This speaker is an "observable" who produces streams of information for you to consume.

## Hot vs. Cold Observables
- Cold Observables can emit items _only when someone subscribes to them_
  - They only begin emitting once they have been subscribed to
  - In Android, a cold observable can be a network call that doesn't fetch (or emit) data if no one is subscribed to it
    - The observable makes the network call, fetches the data and emits _only if someone is interested in this event and subscribes to it_
- Hot Observables emit items regardless of whether someone is subscribed to them
  - Going back to the real-world example, this would be like the speaker producing information streams with or without anyone in the audience
  - In Android, a hot observable can be the location update event stream
    - Location updates will continue to happen even if no one is interested in them
    - If a subscriber eventually _is_ interested in these updates, it can subscribe and start getting the updates _from the point at which it subscribed onward, or until the subscriber unsubscribes_
    - Subscribers subscribing to a hot observable _do not affect the stream_

## Hands On
Consider this example observable:
```java
Observable<Movie> movieObservable = remoteService.getMovies();
```
- Here, the observable can emit `Movie` objects retrieved from some remote data source
- There can be:
  - Single item
  - Multiple items
  - Or no items emitted
- If the observable is unable to fetch data, it will signal an error

### Going Further
Consider the following example:
```java
DisposableObserver<Movie> disposable = movieObservable.subscribeWith( new DisposableObserver<Movie>() {
    
    @Override
    public void onNext( Movie movie ) {
      // Access your Movie object here
    }
    
    @Override
    public void onError( Throwable e ) {
      // Show the user that an error has occurred
    }
    
    @Override
    public void onComplete() {
      // Show the user that the operation is complete
    }
    
});
```
- In the snippet above, we use `subscribeWith()` method to subscribe to the movie observable
- Uses `DisposableObservable`
  - This has the three methods discussed above in Observers
  - `onNext()` will be called every time a `Movie` object is fetched (emitted) by the observable
    - It can be fetched from any source (network, database, files, and more)
- Suppose there are 10 movies in your data store
  - `onNext()` will be called 10 times with 10 movie objects
  - If the operation fails, `onError()` will be called with the appropriate error object

## The Disposable Observer
A [`DisposableObserver`](http://reactivex.io/RxJava/javadoc/io/reactivex/observers/DisposableObserver.html) is nothing but an observer that implements the [`Disposable`](http://reactivex.io/RxJava/javadoc/io/reactivex/disposables/Disposable.html) interface.
- Whenever an observer subscribes to an observable, _a connection is formed_
  - This connection needs to be cleared (or terminated) when not needed, otherwise it leads to resource leaks
- **A listener should not be listening eternally**
  - This is similar to needing to close a database cursor or a file input/output stream when you are done using them
- _Observers are nothing more than garbage after they are done being used_

You should always dispose of your observables when they are no longer needed:
```java
  //...
  
  @Override
  public void onDestroy() {
    
    if ( disposable != null && !disposable.isDisposed() ) {
      disposable.dispose();
    }
    
    super.onDestroy();
    
  }
  
  //...
```
- You can also use a [`CompositeDisposable`](http://reactivex.io/RxJava/2.x/javadoc/io/reactivex/disposables/CompositeDisposable.html) to add up all your disposables at once in the same manner

## Multi-Threading
RxJava2 features two powerful methods to easily-control multi-threading:
1. `observeOn()`
    - Specifies which thread the observer will observe the emissions
2. `subscribeOn()`
    - Specifies which thread the source observable will emit its data
    - _This operator **only works on the source observable**_
    - This operator can only be used once on a source observable
      - **If you do use it multiple times, _this will not cause an error_**
      - Instead, only the one defined closest to source will take effect

# Why You Should Use RxJava 2
This is a list of articles all suggesting RxJava2 be utilized in Android development:
1. [Why Should We Use RxJava on Android](https://medium.com/@lpereira/why-should-we-use-rxjava-on-android-c9066087c56c)
2. [Why You Should Use RxJava in Android - a Short Introduction to RxJava](http://blog.feedpresso.com/2016/01/25/why-you-should-use-rxjava-in-android-a-short-introduction-to-rxjava.html)
