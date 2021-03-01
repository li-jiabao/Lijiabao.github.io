
# Controlling Access to Members of a Class

Access level modifiers determine whether other classes can use a particular field or invoke a particular method. There are two levels of access control:

- At the top level&#151;`public`, or *package-private* (no explicit modifier).
- At the member level&#151;`public`, `private`, `protected`, or *package-private* (no explicit modifier).

A class may be declared with the modifier `public`, in which case that class is visible to all classes everywhere. If a class has no modifier (the default, also known as *package-private*), it is visible only within its own package (packages are named groups of related classes &#151; you will learn about them in a later 
lesson.) 


At the member level, you can also use the `public` modifier or no modifier (*package-private*) just as with top-level classes, and with the same meaning. For members, there are two additional access modifiers: `private` and `protected`. The `private` modifier specifies that the member can only be accessed in its own class. The `protected` modifier specifies that the member can only be accessed within its own package (as with *package-private*) and, in addition, by a subclass of its class in another package.

The following table shows the access to members permitted by each modifier.
<th id="h1">Modifier</th><th id="h2">Class</th><th id="h3">Package</th><th id="h4">Subclass</th><th id="h5">World</th>
<td headers="h1">`public`</td><td headers="h2">Y</td><td headers="h3">Y</td><td headers="h4">Y</td><td headers="h5">Y</td>
<td headers="h1">`protected`</td><td headers="h2">Y</td><td headers="h3">Y</td><td headers="h4">Y</td><td headers="h5">N</td>
<td headers="h1" style="font-style: italic">no modifier</td><td headers="h2">Y</td><td headers="h3">Y</td><td headers="h4">N</td><td headers="h5">N</td>
<td headers="h1">`private`</td><td headers="h2">Y</td><td headers="h3">N</td><td headers="h4">N</td><td headers="h5">N</td>

The first data column indicates whether the class itself has access to the member defined by the access level. As you can see, a class always has access to its own members. The second column indicates whether classes in the same package as the class (regardless of their parentage) have access to the member. The third column indicates whether subclasses of the class declared outside this package have access to the member. The fourth column indicates whether all classes have access to the member.

Access levels affect you in two ways. First, when you use classes that come from another source, such as the classes in the Java platform, access levels determine which members of those classes your own classes can use. Second, when you write a class, you need to decide what access level every member variable and every method in your class should have.

Let's look at a collection of classes and see how access levels affect visibility. 
The following figure shows the four classes in this example and how they are related.

Classes and Packages of the Example Used to Illustrate Access Levels


The following table shows where the members of the Alpha class are visible for each of the access modifiers that can be applied to them.
<th id="h101">Modifier</th><th id="h102">Alpha</th><th id="h103">Beta</th><th id="h104">Alphasub</th><th id="h105">Gamma</th>
<td headers="h101">`public`</td><td headers="h102">Y</td><td headers="h103">Y</td><td headers="h104">Y</td><td headers="h105">Y</td>
<td headers="h101">`protected`</td><td headers="h102">Y</td><td headers="h103">Y</td><td headers="h104">Y</td><td headers="h105">N</td>
<td headers="h101" style="font-style: italic">no modifier</td><td headers="h102">Y</td><td headers="h103">Y</td><td headers="h104">N</td><td headers="h105">N</td>
<td headers="h101">`private`</td><td headers="h102">Y</td><td headers="h103">N</td><td headers="h104">N</td><td headers="h105">N</td>

If other programmers use your class, you want to ensure that errors from misuse cannot happen. Access levels can help you do this.

- Use the most restrictive access level that makes sense for a particular member. Use `private` unless you have a good reason not to.
<li>Avoid `public` fields except for constants. (Many of the examples in the 
tutorial 
use public fields. This may help to illustrate some points concisely, but is not recommended for production code.) Public fields tend to link you to a particular implementation and limit your flexibility in changing your code.</li>
