
# Questions and Exercises: Inheritance

## Questions

1. Consider the following two classes:

```

public class ClassA {
    public void methodOne(int i) {
    }
    public void methodTwo(int i) {
    }
    public static void methodThree(int i) {
    }
    public static void methodFour(int i) {
    }
}

public class ClassB extends ClassA {
    public static void methodOne(int i) {
    }
    public void methodTwo(int i) {
    }
    public void methodThree(int i) {
    }
    public static void methodFour(int i) {
    }
}

```

a. Which method overrides a method in the superclass?<br />
b. Which method hides a method in the superclass?<br />
c. What do the other methods do?<br />
<br />
2. Consider the 
[`Card`](../examples/Card.java), 
[`Deck`](../examples/Deck.java), and 
[`DisplayDeck`](../examples/DisplayDeck.java) classes you wrote in 

[Questions and Exercises: Classes](../../javaOO/QandE/creating-questions.html ). What `Object` methods should each of these classes override?

## Exercises

1. Write the implementations for the methods that you answered in question 2.

<br />


[Check your answers.](inherit-answers.html)
