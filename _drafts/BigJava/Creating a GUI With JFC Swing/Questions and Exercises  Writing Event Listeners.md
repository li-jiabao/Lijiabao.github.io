
# Questions and Exercises: Writing Event Listeners

Use this lesson&#226;&#128;&#153;s tables, the [component how-to sections](../components/index.html) and the [event listeners how-to sections](../events/handling.html) to complete these questions and exercises.

## Questions

1. What listener would you implement to be notified when a particular component has appeared on screen? What method tells you this information?

2. What listener would you implement to be notified when the user has finished editing a text field by pressing Enter? What listener would you implement to be notified as each character is typed into a text field? Note that you should not implement a general- purpose key listener, but a listener specific to text.

3. What listener would you implement to be notified when a spinner&#226;&#128;&#153;s value has changed? How would you get the spinner&#226;&#128;&#153;s new value?

4. The default behavior for the focus subsystem is to consume the focus traversal keys, such as Tab and Shift Tab. Say you want to prevent this from happening in one of your application&#226;&#128;&#153;s components. How would you accomplish this?

## Exercises

1. Take the [`Beeper.java`](../examples/events/BeeperProject/src/events/Beeper.java)[<img src="../../images/sourceIcon.gif" width="11" height="11" border="0" align="middle" alt="(in a .java source file)" />](../examples/events/BeeperProject/src/events/Beeper.java) example and add a text field. Implement it so that when the user has finishing entering data, the system beeps.

2. Take the [`Beeper.java`](../examples/events/BeeperProject/src/events/Beeper.java)[<img src="../../images/sourceIcon.gif" width="11" height="11" border="0" align="middle" alt="(in a .java source file)" />](../examples/events/BeeperProject/src/events/Beeper.java) example and add a selectable component that allows the user to enter a number from 1 to 10. For example, you can use a combo box, a set of radio buttons, or a spinner. Implement it so that when the user has selected the number, the system beeps that many times.


[Check your answers.](answers-ch5.html)
