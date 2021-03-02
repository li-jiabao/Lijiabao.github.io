
# Miscellaneous Methods in System

This section describes some of the methods in `System` that aren't covered in the previous sections.

The `arrayCopy` method efficiently copies data between arrays. For more information, refer to 
[Arrays](../../java/nutsandbolts/arrays.html) in the 
[Language Basics](../../java/nutsandbolts/index.html) lesson.

The 
[`currentTimeMillis`](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#currentTimeMillis--) and 
[`nanoTime`](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#nanoTime--) methods are useful for measuring time intervals during execution of an application. To measure a time interval in milliseconds, invoke `currentTimeMillis` twice, at the beginning and end of the interval, and subtract the first value returned from the second. Similarly, invoking `nanoTime` twice measures an interval in nanoseconds.

The 
[`exit`](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#exit-int-) method causes the Java virtual machine to shut down, with an integer exit status specified by the argument. The exit status is available to the process that launched the application. By convention, an exit status of `0` indicates normal termination of the application, while any other value is an error code.
