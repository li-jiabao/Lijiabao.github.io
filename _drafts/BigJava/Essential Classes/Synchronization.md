
# Synchronization

Threads communicate primarily by sharing access to fields and the objects reference fields refer to. This form of communication is extremely efficient, but makes two kinds of errors possible: *thread interference* and *memory consistency errors*. The tool needed to prevent these errors is *synchronization*.

However, synchronization can introduce <a name="thread_contention" id="thread_contention">**thread contention**</a>, which occurs when two or more threads try to access the same resource simultaneously **and** cause the Java runtime to execute one or more threads more slowly, or even suspend their execution.
[Starvation and livelock](../../essential/concurrency/starvelive.html) are forms of thread contention. See the section
[Liveness](../../essential/concurrency/liveness.html) for more information.

This section covers the following topics:

- [Thread Interference](interfere.html) describes how errors are introduced when multiple threads access shared data.
- [Memory Consistency Errors](memconsist.html) describes errors that result from inconsistent views of shared memory.
- [Synchronized Methods](syncmeth.html) describes a simple idiom that can effectively prevent thread interference and memory consistency errors.
- [Implicit Locks and Synchronization](locksync.html) describes a more general synchronization idiom, and describes how synchronization is based on implicit locks.
- [Atomic Access](atomic.html) talks about the general idea of operations that can't be interfered with by other threads.
