
# Bound Properties and Status Methods

`SwingWorker` supports 
[bound properties](../../javabeans/writing/properties.html#bound), which are useful for communicating with other threads. Two bound properties are predefined: `progress` and `state`. As with all bound properties, `progress` and `state` can be used to trigger event-handling tasks on the event dispatch thread.

By implementing a property change listener, a program can track changes to `progress`, `state`, and other bound properties. For more information, refer to 
[How to Write a Property Change Listener](../events/propertychangelistener.html) in 
[Writing Event Listeners](../events/index.html).

## The `progress` Bound Variable

The `progress` bound variable is an `int` value that can range from 0 to 100. It has a predefined setter method (the protected 
[`SwingWorker.setProgress`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#setProgress--)) and a predefined getter method (the public 
[`SwingWorker.getProgress`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#getProgress-int-)).

The 
[`<code>ProgressBarDemo`</code>](../examples/components/ProgressBarDemoProject/src/components/ProgressBarDemo.java) example uses `progress` to update a `ProgressBar` control from a background task. For a detailed discussion of this example, refer to 
[How to Use Progress Bars](../components/progress.html) in 
[Using Swing Components](../components/index.html).

## The `state` Bound Variable

The `state` bound variable indicates where the `SwingWorker` object is in its lifecycle. The bound variable contains an enumeration value of type `SwingWorker.StateValue`. Possible values are:

The current value of the `state` bound variable is returned by 
[`SwingWorker.getState`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#getState--).

## Status Methods

Two methods, part of the `Future` interface, also report on the status of the background task. As we saw in 
[Canceling Background Tasks](cancel.html), `isCancelled` returns `true` if the task has been canceled. In addition, `isDone` returns `true` if the task has finished, either normally, or by being cancelled.
