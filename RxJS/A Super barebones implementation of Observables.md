#RxJS #Observables 

> References: [Computer Baba's Implementing the Observer Pattern video](https://www.youtube.com/watch?v=chqX6Va6JL8&list=PLqLR2H326bY6FofFwSTNq7nyrL_Y6fZAM&index=2)

In this section we will describe a super barebones implementation of [[Observables]], we will start with describing every thing using plain functions and them move onto using classes. Let's get started.

First we will implement Observables using plain functions

```javascript
function observable(observer) {

  let counter = 1;
  const intervalId = setInterval(() => {
    observer.next(counter++);
  }, 1000);
  return () => {
    clearInterval(intervalId);
  };
}

const closeFn = observable({
  next: (data) => {
    console.log(data);
  },
});

setTimeout(() => {
  closeFn();
}, 5000);
```

The function `observable` takes an observer and then calls the `observer`'s method to notify it of changes.

Next up let's refactor to use classes

```javascript
class ObserverGuard {
  constructor(observer) {
    this.observer = observer;
    this.isUnsubsribed = false;
  }
  next(data) {
    if (!this.observer.next || this.isUnsubsribed) {
      return;
    }
    
    try {
      this.observer.next(data);
    } catch (error) {
      this.observer.unsubscribe();
      this.isUnsubsribed = true;
    }
  }
  error(error) {
    if (!this.observer.error || this.isUnsubsribed) {
      return;
    }
    this.observer.error(error);
    this.unsubscribe();
  }
  complete() {
    if (!this.observer.complete || this.isUnsubsribed) {
      return;
    }
    this.observer.complete();
    this.unsubscribe();
  }
  unsubscribe() {
    this.isUnsubsribed = true;
    if (this.closeFn) {
      this.closeFn();
    }
  }
  get closed() {
    return this.isUnsubsribed;
  }
}
class Observable {
  constructor(observable) {
    this.observable = observable;
  }
  subscribe(observer) {
    const observerWithGuardInstance = new ObserverGuard(observer);
    const closeFn = this.observable(observerWithGuardInstance);
    observerWithGuardInstance.closeFn = closeFn;
    const subscription = this.subscriptionMetaData(observerWithGuardInstance);
    return subscription;
  }
  subscriptionMetaData(observerWithGuardInstance) {
    return {
      unsubscribe() {
        observerWithGuardInstance.unsubscribe();
      },
      get closed() {
        return observerWithGuardInstance.closed;
      },
    };
  }
}
function observableFunction(observer) {
  let counter = 0;
  let intervalId = setInterval(() => {
    observer.next(counter++);
  }, 1000);
  return () => {
    clearInterval(intervalId);
  };
}
const observerObject = {
  next: (data) => {
    console.log(data);
  },
  error: (error) => {
    console.log(error);
  },
  complete: () => {
    console.log("done");
  },
};

const observableInstance = new Observable(observableFunction);
const subscription = observableInstance.subscribe(observerObject);

setTimeout(() => {
  console.log("Unsubsribing");
  subscription.unsubscribe();
}, 5000);

setTimeout(() => {
  console.log(subscription.closed);
}, 7000);
```

This implementation user classes to define Observables, it also defines a class called `ObserverGuard` that takes an observer and makes sure its `next` method cannot be called after the `complete` method has been called or the observer has unsubscribed from the producer.