#RxJS 

>References: [refactoring.guru's Observer Pattern Guide](https://refactoring.guru/design-patterns/observer)

[RxJS](https://rxjs.dev/) is the JavaScript version of popular [ReactiveX](https://reactivex.io/) library. **ReactiveX** is a library that provides API's for easily and efficiently working with asynchronous data.

Before proceeding let's get a brief overview of terms like Reactive Programming and Observer Pattern that often get thrown around in discourse surrounding this topic.

## What is Reactive Programming

Reactive Programming is a programming paradigm that focuses on working with asynchronous data, propagation of change and reacting to changes in data or events. 

It is a style of programming just like other programming paradigms e.g.. **Object Oriented Programming** or **Functional Programming**. Loosely Speaking it is the concept that when a value `x` changes or updates in one location the things that depend on `x` in various other locations are recalculated and updated automatically in a non-blocking way.

## What is the Observer Pattern

The **Observer Pattern** is a [[Design Patterns |Design pattern]], characterized by two main concepts a **Subject** or **Publisher** which is the thing that produces a stream of events and a **Subscriber** or **Consumer** which subscribes to the publisher to automatically receive the stream of events, whenever any changes occur with the publisher all of its subscribers are notified.

In an actual implementation whenever the state of the publisher changes it calls the notification method of the subscriber, only classes which have subscribed to the publisher will have their notification method called.


