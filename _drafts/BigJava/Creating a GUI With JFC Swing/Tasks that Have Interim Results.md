
# Tasks that Have Interim Results

It is often useful for a background task to provide interim results while it is still working. The task can do this by invoking 
[`SwingWorker.publish`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#publish-V...-). This method accepts a variable number of arguments. Each argument must be of the type specified by `SwingWorker`'s second type parameter.

To collect results provided by `publish`, override 
[`SwingWorker.process`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#process-java.util.List-) This method will be invoked from the event dispatch thread. Results from multiple invocations of `publish` are often accumulated for a single invocation of `process`.

Let's look at the way the 
[`<code>Flipper.java`</code>](../examples/concurrency/FlipperProject/src/concurrency/Flipper.java) example uses `publish` to provide interim results. Click the Launch button to run `Flipper` using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Or, to compile and run the example yourself, consult the [example index](../examples/concurrency/index.html#Flipper).

This program tests the fairness of 
[`java.util.Random`](https://docs.oracle.com/javase/8/docs/api/java/util/Random.html) by generating a series of random `boolean` values in a background task. This is equivalent to flipping a coin; hence the name `Flipper`. To report its results, the background task uses an object of type `FlipPair`

```

private static class FlipPair {
    private final long heads, total;
    FlipPair(long heads, long total) {
        this.heads = heads;
        this.total = total;
    }
}

```

The `heads` field is the number of times the random value has been `true`; the `total` field is the total number of random values.

The background task is represented by an instance of `FlipTask`:

```

private class FlipTask extends SwingWorker&lt;Void, FlipPair&gt; {

```

Since the task does not return a final result, it does not matter what the first type parameter is; `Void` is used as a placeholder. The task invokes `publish` after each "coin flip":

```

@Override
protected Void doInBackground() {
    long heads = 0;
    long total = 0;
    Random random = new Random();
    while (!isCancelled()) {
        total++;
        if (random.nextBoolean()) {
            heads++;
        }
        publish(new FlipPair(heads, total));
    }
    return null;
}

```

(The `isCancelled` method is discussed in the next section.) Because `publish` is invoked very frequently, a lot of `FlipPair` values will probably be accumulated before `process` is invoked in the event dispatch thread; `process` is only interested in the last value reported each time, using it to update the GUI:

```

protected void process(List&lt;FlipPair&gt; pairs) {
    FlipPair pair = pairs.get(pairs.size() - 1);
    headsText.setText(String.format("%d", pair.heads));
    totalText.setText(String.format("%d", pair.total));
    devText.setText(String.format("%.10g", 
            ((double) pair.heads)/((double) pair.total) - 0.5));
}

```

If `Random` is fair, the value displayed in `devText` should get closer and closer to 0 as `Flipper` runs.
