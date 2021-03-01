
# Evolving Interfaces

Consider an interface that you have developed called `DoIt`:

```

public interface DoIt {
   void doSomething(int i, double x);
   int doSomethingElse(String s);
}

```

Suppose that, at a later time, you want to add a third method to `DoIt`, so that the interface now becomes:

```

public interface DoIt {

   void doSomething(int i, double x);
   int doSomethingElse(String s);
   boolean didItWork(int i, double x, String s);
   
}

```

If you make this change, then all classes that implement the old `DoIt` interface will break because they no longer implement the old  interface. Programmers relying on this interface will protest loudly.

Try to anticipate all uses for your interface and specify it completely from the beginning. If you want to add additional methods to an interface, you have several options. You could create a `DoItPlus` interface that extends `DoIt`:

```

public interface DoItPlus extends DoIt {

   boolean didItWork(int i, double x, String s);
   
}

```

Now users of your code can choose to continue to use the old interface or to upgrade to the new interface.

Alternatively, you can define your new methods as
[default methods](../../java/IandI/defaultmethods.html). The following example defines a default method named `didItWork`:

```

public interface DoIt {

   void doSomething(int i, double x);
   int doSomethingElse(String s);
   <strong>default boolean didItWork(int i, double x, String s) {
       // Method body 
   }</strong>
   
}

```

Note that you must provide an implementation for default methods. You could also define new
[static methods](../../java/IandI/defaultmethods.html#static) to existing interfaces. Users who have classes that implement interfaces enhanced with new default or static methods do not have to modify or recompile them to accommodate the additional methods.
