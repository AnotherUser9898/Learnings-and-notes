# The Node JS event loop

## Phase execution order

1. The nextTick queue is checked for any callbacks, if found all callbacks in the nextTick queue are executed before moving on to other phases.

2. The promise queue is checked next, if any callbacks are found they are executed. All callbacks in the promise queue are executed before moving on to other phases even if new callbacks are added to the nextTick queue.

3. The Timer queue is checked for callbacks after the two microtask queues. The event loop checks both the microtask queues for callbacks after each callback from the timer queue finishes execution.

4. The IO phase is next, if any callbacks are already present in the io queue they are immediately executed, after execution of each callback the microtask queues are checked and drained.

5. If there are no callbacks then node JS starts the polling phase where it checks for any already complete io operations and adds their callback to the io queue.

6. If there are no callbacks in the further phases node js waits for io events to complete.

7. A subtule detail to keep in mind is that node js only becomes aware of the completed io operations when it checks for them in the poll phase, when it does discover a completed operation, its callback is enqueued onto the io queue and node js moves to the next phase. The enqueued callback will only be executed in the next iteration.

8. Node JS moves onto the check phase if there are callbacks and executes each of them, checking the microtask queues after each callback execution finishes.
