
# Questions and Exercises: Annotations

## Questions

<li>
What is wrong with the following interface?
<pre><code>
public interface House {
    @Deprecated
    void open();
    void openFrontDoor();
    void openBackDoor();
}
</code></pre>
</li>
<li>
Consider this implementation of the `House` interface, shown in Question 1.
<pre><code>
public class MyHouse implements House {
    public void open() {}
    public void openFrontDoor() {}
    public void openBackDoor() {}
}
</code></pre>
If you compile this program, the compiler produces a warning because `open` was deprecated (in the interface). What can you do to get rid of that warning?
</li>

<li>
Will the following code compile without error? Why or why not?
<pre><code>
public @interface Meal { ... }

@Meal("breakfast", mainDish="cereal")
@Meal("lunch", mainDish="pizza")
@Meal("dinner", mainDish="salad")
public void evaluateDiet() { ... }
</code></pre>
</li>

## Exercises

1. Define an annotation type for an enhancement request with elements `id`, `synopsis`, `engineer`, and `date`. Specify the default value as `unassigned` for engineer and `unknown` for date.


[Check your answers.](answers.html)
