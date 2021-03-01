
# Questions and Exercises: Concurrency

## Questions

1. Can you pass a `Thread` object to `Executor.execute`? Would such an invocation make sense?

## Exercises

<li>Compile and run 
[`<code>BadThreads.java`</code>](BadThreads.java):
<pre><code>

public class BadThreads {

    static String message;

    private static class CorrectorThread
        extends Thread {

        public void run() {
            try {
                sleep(1000); 
            } catch (InterruptedException e) {}
            // Key statement 1:
            message = "Mares do eat oats."; 
        }
    }

    public static void main(String args[])
        throws InterruptedException {

        (new CorrectorThread()).start();
        message = "Mares do not eat oats.";
        Thread.sleep(2000);
        // Key statement 2:
        System.out.println(message);
    }
}
</code></pre>
The application should print out "Mares do eat oats." Is it guaranteed to always do this? If not, why not? Would it help to change the parameters of the two invocations of `Sleep`? How would you guarantee that all changes to `message` will be visible in the main thread?
</li>
1. Modify the producer-consumer example in [Guarded Blocks](../guardmeth.html) to use a standard library class instead of the `Drop` class.


[Check your answers.](answers.html)
