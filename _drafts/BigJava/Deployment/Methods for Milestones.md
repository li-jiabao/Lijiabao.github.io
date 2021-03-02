
# Methods for Milestones

The 
[`Applet`](https://docs.oracle.com/javase/8/docs/api/java/applet/Applet.html) class provides a framework for applet execution, defining methods that the system calls when milestones occur. Milestones are major events in an applet's life cycle. Most applets override some or all of these methods to respond appropriately to milestones.

## `init` Method

The `init` method is useful for one-time initialization that doesn't take very long. The `init` method typically contains the code that you would normally put into a constructor. The reason applets don't usually have constructors is that they aren't guaranteed to have a full environment until their `init` method is called. Keep the `init` method short so that your applet can load quickly.

## `start` Method

Every applet that performs tasks after initialization (except in direct response to user actions) must override the `start` method. The `start` method starts the execution of the applet. It is good practice to return quickly from the `start` method. If you need to perform computationally intensive operations it might be better to start a new thread for this purpose.

## `stop` Method

Most applets that override the `start` should also override the `stop` method. The `stop` method should suspend the applet's execution, so that it doesn't take up system resources when the user isn't viewing the applet's page. For example, an applet that displays an animation should stop trying to draw the animation when the user isn't viewing it.

## `destroy` Method

Many applets don't need to override the `destroy` method because their `stop` method (which is called before `destroy`) will perform all tasks necessary to shut down the applet's execution. However, the `destroy` method is available for applets that need to release additional resources.
