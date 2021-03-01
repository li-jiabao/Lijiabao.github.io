
# When to Use Nested Classes, Local Classes, Anonymous Classes, and Lambda Expressions 

As mentioned in the section 
[Nested Classes](nested.html), nested classes enable you to logically group classes
that are only used in one place, increase the use of
encapsulation, and create more readable and maintainable code.
Local classes, anonymous classes, and lambda expressions also
impart these advantages; however, they are intended to be used
for more specific situations:

  <li><p>
[Local class](localclasses.html): Use it if you need to create more than one
instance of a class, access its constructor, or introduce a new, named
type (because, for example, you need to invoke additional methods
later).</p></li>
  <li><p>
[Anonymous class](anonymousclasses.html): Use it if you need to declare fields or additional methods.</p></li>
  <li><p>
[Lambda expression](lambdaexpressions.html):</p>
    <ul>
      - Use it if you are encapsulating a single unit of behavior that you want to pass to other code. For example, you would use a lambda expression if you want a certain action performed on each element of a collection, when a process is completed, or when a process encounters an error.
      - Use it if you need a simple instance of a functional interface and none of the preceding criteria apply (for example, you do not need a constructor, a named type, fields, or additional methods).
    

[Nested class](nested.html): Use it if your requirements are similar to those
of a local class, you want to make the type more widely
available, and you don't require access to local variables or
method parameters.

      - Use a non-static nested class (or inner class) if you require access to an enclosing instance's non-public fields and methods. Use a static nested class if you don't require this access.
    