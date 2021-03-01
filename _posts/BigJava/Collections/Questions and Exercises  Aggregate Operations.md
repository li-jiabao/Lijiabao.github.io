
# Questions and Exercises: Aggregate Operations

## Questions

  1. A sequence of aggregate operations is known as a ___ .
  1. Each pipeline contains zero or more ___ operations.
  1. Each pipeline ends with a ___ operation.
  1. What kind of operation produces another stream as its output?
  <li>Describe one way in which the `forEach` aggregate operation differs from the enhanced 
`for` statement
 or iterators.</li>
  <li>True or False: A stream is similar to a collection in that it 
 is a data structure that stores elements.</li>
  <li>Identify the intermediate and terminal operations in this code:
<pre><code>
double average = roster
    .stream()
    .filter(p -&gt; p.getGender() == Person.Sex.MALE)
    .mapToInt(Person::getAge)
    .average()
    .getAsDouble();
</code></pre>
</li>
<li>The code 
<code>
p -&gt; p.getGender() == Person.Sex.MALE
</code>
 is an example of what?</li>
<li>
The code
<code>
Person::getAge
</code>
is an example of what?
</li>
<li>
Terminal operations that combine the contents of a stream and return one value
are known as what?
</li>

<li>
Name one important difference between the `Stream.reduce` method 
and the `Stream.collect` method.
</li>

<li>
If you wanted to process a stream of names, extract the male names, and 
store them in a new `List`, would `Stream.reduce` or 
`Stream.collect` be the 
most appropriate operation to use?
</li>

<li>
True or False: Aggregate operations make it possible to
implement parallelism with non-thread-safe collections.
</li>

<li>
Streams are always serial unless otherwise specified. How 
 do you request that a stream be processed in parallel?
</li>


## Exercises

 <li>
Write the following enhanced `for` statement as a 
pipeline with lambda expressions. Hint: Use the
`filter` intermediate operation and the `forEach` terminal 
operation.
<br />
<pre><code>
for (Person p : roster) {
    if (p.getGender() == Person.Sex.MALE) {
        System.out.println(p.getName());
    }
}
</code></pre>



 </li>

<li>Convert the following code into a new implementation that 
uses lambda expressions and aggregate operations instead of nested
`for` loops. Hint: Make a pipeline that invokes the `filter`, `sorted`, and 
`collect`
operations, in that order. 

<pre><code>
List&lt;Album&gt; favs = new ArrayList&lt;&gt;();
for (Album a : albums) {
    boolean hasFavorite = false;
    for (Track t : a.tracks) {
        if (t.rating &gt;= 4) {
            hasFavorite = true;
            break;
        }
    }
    if (hasFavorite)
        favs.add(a);
}
Collections.sort(favs, new Comparator&lt;Album&gt;() {
                           public int compare(Album a1, Album a2) {
                               return a1.name.compareTo(a2.name);
                           }});
</code></pre>
</li>


[Check your answers.](answers.html)
