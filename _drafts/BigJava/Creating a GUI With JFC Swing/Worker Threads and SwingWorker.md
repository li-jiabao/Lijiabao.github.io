
# Worker Threads and SwingWorker

When a Swing program needs to execute a long-running task, it usually uses one of the *worker threads*, also known as the *background threads*. Each task running on a worker thread is represented by an instance of 
[`javax.swing.SwingWorker`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html). `SwingWorker` itself is an abstract class; you must define a subclass in order to create a `SwingWorker` object; anonymous inner classes are often useful for creating very simple `SwingWorker` objects.

`SwingWorker` provides a number of communication and control features:

- The `SwingWorker` subclass can define a method, `done`, which is automatically invoked on the event dispatch thread when the background task is finished.
<li>`SwingWorker` implements 
[`java.util.concurrent.Future`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Future.html). This interface allows the background task to provide a return value to the other thread. Other methods in this interface allow cancellation of the background task and discovering whether the background task has finished or been cancelled.</li>
- The background task can provide intermediate results by invoking `SwingWorker.publish`, causing `SwingWorker.process` to be invoked from the event dispatch thread.
- The background task can define bound properties. Changes to these properties trigger events, causing event-handling methods to be invoked on the event dispatch thread.

These features are discussed in the following subsections.

The `javax.swing.SwingWorker` class was added to the Java platform in Java SE 6. Prior to this, another class, also called `SwingWorker`, was widely used for some of the same purposes. The old `SwingWorker` was not part of the Java platform specification, and was not provided as part of the JDK.

The new `javax.swing.SwingWorker` is a completely new class. Its functionality is not a strict superset of the old `SwingWorker`. Methods in the two classes that have the same function do not have the same names. Also, instances of the old `SwingWorker` class were reusable, while a new instance of `javax.swing.SwingWorker` is needed for each new background task.

Throughout the Java Tutorials, any mention of `SwingWorker` now refers to `javax.swing.SwingWorker`.
