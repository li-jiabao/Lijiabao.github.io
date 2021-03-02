
# Canceling Background Tasks

To cancel a running background task, invoke 
[`SwingWorker.cancel`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#cancel-boolean-) The task must cooperate with its own cancellation. There are two ways it can do this:

<li>By terminating when it receives an interrupt. This procedures is described in 
[Interrupts](../../essential/concurrency/interrupt.html) in 
[Concurrency](../../essential/concurrency/index.html).</li>
<li>By invoking 
[`SwingWorker.isCancelled`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingWorker.html#isCancelled--) at short intervals. This method returns `true` if `cancel` has been invoked for this `SwingWorker`.</li>

The `cancel` method takes a single `boolean` argument. If the argument is `true`, `cancel` sends the background task an interrupt. Whether the argument is `true` or `false`, invoking `cancel` changes the cancellation status of the object to `true`. This is the value returned by `isCancelled`. Once changed, the cancellation status cannot be changed back.

The `Flipper` example from the previous section uses the status-only idiom. The main loop in `doInBackground` exits when `isCancelled` returns `true`. This will occur when the user clicks the "Cancel" button, triggering code that invokes `cancel` with an argument of `false`.

The status-only approach makes sense for `Flipper` because its implementation of `SwingWorker.doInBackground` does not include any code that might throw `InterruptedException`. To respond to an interrupt, the background task would have to invoke `Thread.isInterrupted` at short intervals. It's just as easy to use `SwingWorker.isCancelled` for the same purpose
