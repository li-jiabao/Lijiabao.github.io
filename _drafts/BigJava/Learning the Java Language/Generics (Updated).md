
# Lesson: Generics (Updated)


In any nontrivial software project, bugs are simply a fact of life.  Careful planning, programming, and testing can help reduce their pervasiveness, but somehow, somewhere, they'll always find a way to creep into your code. This becomes especially apparent as new features are introduced and your code base grows in size and complexity.


Fortunately, some bugs are easier to detect than others. Compile-time bugs, for example, can be detected early on; you can use the compiler's error messages to figure out what the problem is and fix it, right then and there. Runtime bugs, however, can be much more problematic; they don't always surface immediately, and when they do, it may be at a point in the program that is far removed from the actual cause of the problem.


Generics add stability to your code by making more of your bugs detectable at compile time. After completing this lesson, you may want to follow up with the 
[Generics](../../extra/generics/index.html) tutorial by Gilad Bracha.
