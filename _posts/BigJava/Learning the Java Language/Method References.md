
# Method References

You use
[lambda expressions](lambdaexpressions.html) to create anonymous methods. Sometimes, however, a lambda expression does nothing but call an existing method. In those cases, it's often clearer to refer to the existing method by name. Method references enable you to do this; they are compact, easy-to-read lambda expressions for methods that already have a name.

Consider again the
[`Person`](examples/Person.java) class discussed in the section
[Lambda Expressions](lambdaexpressions.html):

```
MethodReferencesTest</code></a>):</p>

<pre class="codeblock">Person[] rosterAsArray = roster.toArray(new Person[roster.size()]);

class PersonAgeComparator implements Comparator&lt;Person&gt; {
    public int compare(Person a, Person b) {
        return a.getBirthday().compareTo(b.getBirthday());
    }
}
        
Arrays.sort(rosterAsArray, new PersonAgeComparator());
```

The method signature of this invocation of `sort` is the following:

```
ContainingClass::staticMethodName</code></td>
  </tr>
  
  <tr>
    <td headers="h1">Reference to an instance method of a particular object</td>
    <td headers="h2">`containingObject::instanceMethodName`</td>
  </tr>
  
  <tr>
    <td headers="h1">Reference to an instance method of an arbitrary object of a particular type</td>
    <td headers="h2">`ContainingType::methodName`</td>
  </tr>
  
  <tr>
    <td headers="h1">Reference to a constructor</td>
    <td headers="h2">`ClassName::new`</td>
  </tr>
  
</table>

<h3><a name="static" id="static">Reference to a Static Method</a></h3>

The method reference `Person::compareByAge` is a reference to a static method.

<h3><a name="object" id="object">Reference to an Instance Method of a Particular Object</a></h3>

The following is an example of a reference to an instance method of a particular object:

<pre class="codeblock">class ComparisonProvider {
    public int compareByName(Person a, Person b) {
        return a.getName().compareTo(b.getName());
    }
        
    public int compareByAge(Person a, Person b) {
        return a.getBirthday().compareTo(b.getBirthday());
    }
}
ComparisonProvider myComparisonProvider = new ComparisonProvider();
Arrays.sort(rosterAsArray, **myComparisonProvider::compareByName**);
```

### <a name="static" id="static">Reference to a Static Method</a>

The method reference `myComparisonProvider::compareByName` invokes the method `compareByName` that is part of the object `myComparisonProvider`. The JRE infers the method type arguments, which in this case are `(Person, Person)`.

### <a name="type" id="type">Reference to an Instance Method of an Arbitrary Object of a Particular Type</a>

The following is an example of a reference to an instance method of an arbitrary object of a particular type:

The equivalent lambda expression for the method reference `String::compareToIgnoreCase` would have the formal parameter list `(String a, String b)`, where `a` and `b` are arbitrary names used to better describe this example. The method reference would invoke the method `a.compareToIgnoreCase(b)`.

### <a name="constructor" id="constructor">Reference to a Constructor</a>

You can reference a constructor in the same way as a static method by using the name `new`. The following method copies elements from one collection to another:

The functional interface `Supplier` contains one method `get` that takes no arguments and returns an object. Consequently, you can invoke the method `transferElements` with a lambda expression as follows:

You can use a constructor reference in place of the lambda expression as follows:

The Java compiler infers that you want to create a `HashSet` collection that contains elements of type `Person`. Alternatively, you can specify this as follows:
