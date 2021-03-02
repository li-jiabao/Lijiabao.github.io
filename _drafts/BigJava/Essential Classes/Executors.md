
# Executors

In all of the previous examples, there's a close connection between the task being done by a new thread, as defined by its `Runnable` object, and the thread itself, as defined by a `Thread` object. This works well for small applications, but in large-scale applications, it makes sense to separate thread management and creation from the rest of the application. Objects that encapsulate these functions are known as *executors*. The following subsections describe executors in detail.

- [Executor Interfaces](exinter.html) define the three executor object types.
- [Thread Pools](pools.html) are the most common kind of executor implementation.
- [Fork/Join](forkjoin.html) is a framework (new in JDK 7) for taking advantage of multiple processors.
