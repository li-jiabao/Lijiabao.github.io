
# Lesson: Concurrency in Swing

This lesson discusses concurrency as it applies to Swing programming. It assumes that you are already familiar with the content of the 
[Concurrency](../../essential/concurrency/index.html) lesson in the 
[Essential Classes](../../essential/index.html) trail.

Careful use of concurrency is particularly important to the Swing programmer. A well-written Swing program uses concurrency to create a user interface that never "freezes" &#8212; the program is always responsive to user interaction, no matter what it's doing. To create a responsive program, the programmer must learn how the Swing framework employs threads.

A Swing programmer deals with the following kinds of threads:

- *Initial threads*, the threads that execute initial application code.
- The *event dispatch thread*, where all event-handling code is executed. Most code that interacts with the Swing framework must also execute on this thread.
- *Worker threads*, also known as *background threads*, where time-consuming background tasks are executed.

The programmer does not need to provide code that explicitly creates these threads: they are provided by the runtime or the Swing framework. The programmer's job is to utilize these threads to create a responsive, maintainable Swing program.

Like any other program running on the Java platform, a Swing program can create additional threads and thread pools, using the tools described in the Concurrency lesson. But for basic Swing programs the threads described here are sufficient.

This lesson discusses each of the three kinds of threads in turn. Worker threads require the most discussion because tasks that run on them are created using `javax.swing.SwingWorker`. This class has many useful features, including communication and coordination between worker thread tasks and the tasks on other threads.
