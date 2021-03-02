
# How to Use Swing Timers

A Swing timer (an instance of 
[`javax.swing.Timer`](https://docs.oracle.com/javase/8/docs/api/javax/swing/Timer.html)) fires one or more action events after a specified delay. Do not confuse Swing timers with the general-purpose timer facility in the `java.util` package. This page describes only Swing timers.

In general, we recommend using Swing timers rather than general-purpose timers for GUI-related tasks because Swing timers all share the same, pre-existing timer thread and the GUI-related task automatically executes on the event-dispatch thread. However, you might use a general-purpose timer if you don't plan on touching the GUI from the timer, or need to perform lengthy processing.

You can use Swing timers in two ways:

<li>To perform a task once, after a delay.<br />
For example, the tool tip manager uses Swing timers to determine when to show a tool tip and when to hide it.</li>
<li>To perform a task repeatedly.<br />
For example, you might perform animation or update a component that displays progress toward a goal.</li>

Swing timers are very easy to use. When you create the timer, you specify an action listener to be notified when the timer "goes off". The `actionPerformed` method in this listener should contain the code for whatever task you need to be performed. When you create the timer, you also specify the number of milliseconds between timer firings. If you want the timer to go off only once, you can invoke `setRepeats(false)` on the timer. To start the timer, call its `start` method. To suspend it, call `stop`.

Note that the Swing timer's task is performed in the event dispatch thread. This means that the task can safely manipulate components, but it also means that the task should execute quickly. If the task might take a while to execute, then consider using a `SwingWorker` instead of or in addition to the timer. See 
[Concurrency in Swing](../concurrency/index.html) for instructions about using the `SwingWorker` class and information on using Swing components in multi-threaded programs.

Let's look at an example of using a timer to periodically update a component. The 
[`<code>TumbleItem`</code>](../examples/components/TumbleItemProject/src/components/TumbleItem.java) applet uses a timer to update its display at regular intervals. (To see this applet running, go to 
[How to Make Applets](../components/applet.html). This applet begins by creating and starting a timer:

```

timer = new Timer(speed, this);
timer.setInitialDelay(pause);
timer.start(); 

```

The `speed` and `pause` variables represent applet parameters; as configured on the other page, these are 100 and 1900 respectively, so that the first timer event will occur in approximately 1.9 seconds, and recur every 0.1 seconds. By specifying `this` as the second argument to the `Timer` constructor, `TumbleItem` specifies that it is the action listener for timer events.

After starting the timer, `TumbleItem` begins loading a series of images in a background thread. Meanwhile, the timer events begin to occur, causing the `actionPerformed` method to execute:

```

public void actionPerformed(ActionEvent e) {
    //If still loading, can't animate.
    if (!worker.isDone()) {
        return;
    }

    loopslot++;

    if (loopslot &gt;= nimgs) {
        loopslot = 0;
        off += offset;

        if (off &lt; 0) {
            off = width - maxWidth;
        } else if (off + maxWidth &gt; width) {
            off = 0;
        }
    }

    animator.repaint();

    if (loopslot == nimgs - 1) {
        timer.restart();
    }
}

```

Until the images are loaded, `worker.isDone` returns `false`, so timer events are effectively ignored. The first part of the event handling code simply sets values that are employed in the animation control's `paintComponent` method: `loopslot` (the index of the next graphic in the animation) and `off` (the horizontal offset of the next graphic).

Eventually, `loopslot` will reach the end of the image array and start over. When this happens, the code at the end of `actionPerformed` restarts the timer. Doing this causes a short delay before the animation sequence begins again.
