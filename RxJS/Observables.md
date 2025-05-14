#RxJS #Observables

> References: [Computer Baba's RxJS and Observables introduction](https://www.youtube.com/watch?v=v0DtWGoLRmI&list=PLqLR2H326bY6FofFwSTNq7nyrL_Y6fZAM&index=1)

[[Introduction to RxJS |RxJS]] extends the [[Introduction to RxJS#What is the Observer Pattern|Observer Pattern]] to support sequences of data and/or events and adds operators that allow you to compose sequences together declaratively while abstracting away concerns about things like low-level threading, synchronization, thread-safety, concurrent data structures, and non-blocking I/O. It does this using a concept known as Observables.

So what exactly are Observables? 

## What are Observables

To explain what observables are let's first set up the scene.

Consider we have a producer that emits certain values over time, these values can be anything from mouse positions to [[API]] calls, rendering the producer to be anything from a mouse to web server serving http requests. Now, also consider we have a consumer that wishes to listen to or capture the values emitted by the producer.

Here is an image to better represent the situation.

![[Scenario-1 Observables.png]]
Under normal circumstances the producer will keep emitting values but the consumer will have no way to track those values or capture them for his own use, this is because both the producer and consumer arecompletely separate entities with no connection whatsoever.

This is precisely the situation Observables rescue us from.

An Observable wraps around a producer and by doing so provides a mechanism to outside entities to *observe* the changes in the producer's emitted values.

Now, let's wrap our producer in an Observable, this will allow us to *observe* the values emitted by the producer through the Observable. Even though it is the producer that emits the values, because we get access to those values through an Observable, it appears that the values are emitted by the observable itself and due to this fact whenever we will talk about emitted values from here on, we will consider the values to be emitted by the Observable instance to simplify the discussion.

Okay, now we have an observable that can emit values from time to time, we still won't be able to capture the eventually emitted values however, the reason for this is that the Observable does not know that you, the consumer are interested in its values and therefore does not notify you when there are emitted, to fix this we need to tell the observable that we are interested in being notified whenever a new value is emitted, we do this using a special object known as a **Subscription**. When we tell the observer of our interest in its values using a subscription we are said to have subscribed to the observable, and now we will be notified of any future values allowing us to capture that value and react to it.

Again an image of the situation thus far.

![[Scenario-2 Observables.png]]

One very important property of Observables is that they are *lazy* by default, meaning they won't emit values unless someone has subscribed to it, this is behavior is not too different from a function for example, a function also only executes once it is called and not before that.

### Types of Observables

- Finite Observables: -
		These Observables eventually stop emitting values, they have an endpoint. e.g. HTTP request.
- Infinite Observables: -
		These Observables never stop emitting events, they don't have an endpoint. e.g. Mouse clicks, mouse position
- Unicast Observables: -
		These Observables create a new producer for every subscriber.
- Multicast Observables: -
		These observables share a producer with multiple subscribers.



## Diving Deeper

Now that we have a theoretical understanding of what Observables and Observers(Consumers) are let's dive into their specifics.

### What is an Observer really?

An observer is in reality an object. This object has three methods, `next`, `error` and `complete`

The observable calls these methods according to its state.

1. `next`: The `next` method is called when an event occurs the data to be emitted is passed in as a parameter to this method.
2. `complete`: This method is called when an observable stream completes, this method takes no parameters.
3. `error`: This method is called when an error occurs within the observable, it takes the error as its parameter.

### What is a Subscription really?

An observer subscribes to an observable using the `subscribe()` method this method returns an object called the **Subscription** object, this method represents disposable resources, it only contains a single method, the `unsubscribe()` method which is used to cancel observable executions and effectively stop the producer execution.