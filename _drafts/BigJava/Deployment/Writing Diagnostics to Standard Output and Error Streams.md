
# Writing Diagnostics to Standard Output and Error Streams

A Java applet can write messages to the standard output and standard error streams. Writing diagnostics to standard output can be an invaluable tool when you are debugging a Java applet.

The following code snippet writes messages to the standard output stream and the standard error stream.

```

**// Where instance variables are declared:**
boolean DEBUG = true;
// ...
**// Later, when we want to print some status:**
if (DEBUG) {
    try {
        // ...
        //some code that throws an exception
        System.out.
            println("Called someMethod(" + x + "," + y + ")");
    } catch (Exception e) {
        e.printStackTrace()
    }
}

```

Check the Java Console log for messages written to the standard output stream or standard error stream. To store messages in a log file, enable logging in the Java Control Panel. Messages will be written to a log file in the user's home directory (for example, on Windows, the log file might be in `C:\Documents and Settings\someuser\Application Data\Sun\Java\Deployment\log`).
