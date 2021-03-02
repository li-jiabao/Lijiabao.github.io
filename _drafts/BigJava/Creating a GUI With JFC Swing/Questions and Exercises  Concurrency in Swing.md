
# Questions and Exercises: Concurrency in Swing

## Questions

<li>For each of the following tasks, specify which thread it should be executed in and why.
<ul>
1. Initializing the GUI.
1. Loading a large file.
<li>Invoking 
[`javax.swing.JComponent.setFont`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setFont-java.awt.Font-) to change the font of a component.</li>
<li>Invoking 
[`javax.swing.text.JTextComponent.setText`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setText-java.lang.String-) to change the text of a component.</li>
</ul>
</li>
1. One set of threads is not used for any of the tasks mentioned in the previous question. Name this thread and explain why its applications are so limited.
1. `SwingWorker` has two type parameters. Explain how these type parameters are used, and why it often doesn't matter what they are.

## Exercises

<li>Modify the 
[`<code>Flipper`</code>](../examples/concurrency/FlipperProject/src/concurrency/Flipper.java) example so that it pauses 5 seconds between "coin flips." If the user clicks the "Cancel", the coin-flipping loop terminates immediately.</li>


[Check your answers.](answers-concurrency.html)
