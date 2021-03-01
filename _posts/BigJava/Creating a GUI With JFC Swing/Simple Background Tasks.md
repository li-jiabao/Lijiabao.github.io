
# Simple Background Tasks

Let's start with a task that is very simple, but potentially time-consuming. The 
[`<code>TumbleItem`</code>](../examples/components/TumbleItemProject/src/components/TumbleItem.java) applet loads a set of graphic files used in an animation. If the graphic files are loaded from an initial thread, there may be a delay before the GUI appears. If the graphic files are loaded from the event dispatch thread, the GUI may be temporarily unresponsive.

To avoid these problems, `TumbleItem` creates and executes an instance of `SwingWorker` from its initial threads. The object's `doInBackground` method, executing in a worker thread, loads the images into an `ImageIcon` array, and returns a reference to it. Then the `done` method, executing in the event dispatch thread, invokes `get` to retrieve this reference, which it assigns to an applet class field named `imgs`. This allows `TumbleItem` to construct the GUI immediately, without waiting for the images to finish loading.

Here is the code that defines and executes the `SwingWorker` object.

```

SwingWorker worker = new SwingWorker&lt;ImageIcon[], Void&gt;() {
    @Override
    public ImageIcon[] doInBackground() {
        final ImageIcon[] innerImgs = new ImageIcon[nimgs];
        for (int i = 0; i &lt; nimgs; i++) {
            innerImgs[i] = loadImage(i+1);
        }
        return innerImgs;
    }

    @Override
    public void done() {
        //Remove the "Loading images" label.
        animator.removeAll();
        loopslot = -1;
        try {
            imgs = get();
        } catch (InterruptedException ignore) {}
        catch (java.util.concurrent.ExecutionException e) {
            String why = null;
            Throwable cause = e.getCause();
            if (cause != null) {
                why = cause.getMessage();
            } else {
                why = e.getMessage();
            }
            System.err.println("Error retrieving file: " + why);
        }
    }
};


```

All concrete subclasses of `SwingWorker` implement `doInBackground`; implementation of `done` is optional.

Notice that `SwingWorker` is a generic class, with two type parameters. The first type parameter specifies a return type for `doInBackground`, and also for the `get` method, which is invoked by other threads to retrieve the object returned by `doInBackground`. `SwingWorker`'s second type parameter specifies a type for interim results returned while the background task is still active. Since this example doesn't return interim results, 
[`Void` ](https://docs.oracle.com/javase/8/docs/api/java/lang/Void.html) is used as a placeholder.

You may wonder if the code that sets `imgs` is unnecessarily complicated. Why make `doInBackground` return an object and use `done` to retrieve it? Why not just have `doInBackground` set `imgs` directly? The problem is that the object `imgs` refers to is created in the worker thread and used in the event dispatch thread. When objects are shared between threads in this way, you must make sure that changes made in one thread are visible to the other. Using `get` guarantees this, because using `get` creates a *happens before* relationship between the code that creates `imgs` and the code that uses it. For more on the happens before relationship, refer to 
[Memory Consistency Errors](../../essential/concurrency/memconsist.html) in the 
[Concurrency](../../essential/concurrency/index.html) lesson.

There are actually two ways to retrieve the object returned by `doInBackground`.

<li>Invoke 
[`SwingWorker.get` with no arguments](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#get--). If the background task is not finished, `get` blocks until it is.</li>
<li>Invoke 
[`SwingWorker.get` with arguments indicating a timeout](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#get-long-java.util.concurrent.TimeUnit-). If the background task is not finished, `get` blocks until it is &#8212; unless the timeout expires first, in which case `get` throws `java.util.concurrent.TimeoutException`.</li>

Be careful when invoking either overload of `get` from the event dispatch thread; until `get` returns, no GUI events are being processed, and the GUI is "frozen". Don't invoke `get` without arguments unless you are confident that the background task is complete or close to completion.

For more on the `TumbleItem` example, refer to 
[How to Use Swing Timers](../misc/timer.html) in the lesson 
[Using Other Swing Features](../misc/index.html).
