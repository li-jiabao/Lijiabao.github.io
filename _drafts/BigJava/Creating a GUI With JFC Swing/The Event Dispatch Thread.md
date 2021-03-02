
# The Event Dispatch Thread

Swing event handling code runs on a special thread known as the event dispatch thread. Most code that invokes Swing methods also runs on this thread. This is necessary because most Swing object methods are not "thread safe": invoking them from multiple threads risks 
[thread interference](../../essential/concurrency/interfere.html) or 
[memory consistency errors](../../essential/concurrency/memconsist.html). Some Swing component methods are labelled "thread safe" in the API specification; these can be safely invoked from any thread. All other Swing component methods must be invoked from the event dispatch thread. Programs that ignore this rule may function correctly most of the time, but are subject to unpredictable errors that are difficult to reproduce.

It's useful to think of the code running on the event dispatch thread as a series of short tasks. Most tasks are invocations of event-handling methods, such as `ActionListener.actionPerformed`. Other tasks can be scheduled by application code, using `invokeLater` or `invokeAndWait`. Tasks on the event dispatch thread must finish quickly; if they don't, unhandled events back up and the user interface becomes unresponsive.

If you need to determine whether your code is running on the event dispatch thread, invoke 
[`javax.swing.SwingUtilities.isEventDispatchThread`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingUtilities.html#isEventDispatchThread--).
