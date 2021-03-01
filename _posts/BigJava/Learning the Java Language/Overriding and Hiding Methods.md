
# Overriding and Hiding Methods

## Instance Methods

An instance method in a subclass with the same signature (name, plus the number and the type of its parameters) and return type as an instance method in the superclass *overrides* the superclass's method.

The ability of a subclass to override a method allows a class to inherit from a superclass whose behavior is "close enough" and then to modify behavior as needed. The overriding method has the same name, number and type of parameters, and return type as the method that it overrides. An overriding method can also return a subtype of the type returned by the overridden method. This subtype is called a *covariant return type*.

When overriding a method, you might want to use the `@Override` annotation that instructs the compiler that you intend to override a method in the superclass. If, for some reason, the compiler detects that the method does not exist in one of the superclasses, then it will generate an error. For more information on `@Override`, see 
[`Annotations`](../annotations/index.html).

## Static Methods

If a subclass defines a static method with the same signature as a static method in the superclass, then the method in the subclass *hides* the one in the superclass.

The distinction between hiding a static method and overriding an instance method has important implications:

  - The version of the overridden instance method that gets invoked is the one in the subclass.
  - The version of the hidden static method that gets invoked depends on whether it is invoked from the superclass or the subclass.

Consider an example that contains two classes. The first is `Animal`, which contains one instance method and one static method:

```

public class Animal {
    public static void testClassMethod() {
        System.out.println("The static method in Animal");
    }
    public void testInstanceMethod() {
        System.out.println("The instance method in Animal");
    }
}

```

The second class, a subclass of `Animal`, is called `Cat`:

```

public class Cat extends Animal {
    public static void testClassMethod() {
        System.out.println("The static method in Cat");
    }
    public void testInstanceMethod() {
        System.out.println("The instance method in Cat");
    }

    public static void main(String[] args) {
        Cat myCat = new Cat();
        Animal myAnimal = myCat;
        Animal.testClassMethod();
        myAnimal.testInstanceMethod();
    }
}

```

The `Cat` class overrides the instance method in `Animal` and hides the static method in `Animal`. The `main` method in this class creates an instance of `Cat` and invokes `testClassMethod()` on the class and `testInstanceMethod()` on the instance.

The output from this program is as follows:

```

The static method in Animal
The instance method in Cat

```

As promised, the version of the hidden static method that gets invoked is the one in the superclass, and the version of the overridden instance method that gets invoked is the one in the subclass.

## <a name="default" id="default">Interface Methods</a>

 
[Default methods](../../java/IandI/defaultmethods.html) and 
[abstract methods](../../java/IandI/abstract.html) in interfaces are inherited like instance methods. However, when the supertypes of a class or interface provide multiple default methods with the same signature, the Java compiler follows inheritance rules to resolve the name conflict. These rules are driven by the following two principles:

  <li>
    Instance methods are preferred over interface default methods.
  
    Consider the following classes and interfaces:

<pre class="codeblock">public class Horse {
    public String identifyMyself() {
        return "I am a horse.";
    }
}</code></pre>

<pre class="codeblock">public interface Flyer {
    default public String identifyMyself() {
        return "I am able to fly.";
    }
}</code></pre>

<pre class="codeblock">public interface Mythical {
    default public String identifyMyself() {
        return "I am a mythical creature.";
    }
}</code></pre>

<pre class="codeblock">public class Pegasus extends Horse implements Flyer, Mythical {
    public static void main(String... args) {
        Pegasus myApp = new Pegasus();
        System.out.println(myApp.identifyMyself());
    }
}</code></pre>

    The method `Pegasus.identifyMyself` returns the string `I am a horse.`
  </li>
  
  <li>Methods that are already overridden by other candidates are ignored. This circumstance can arise when supertypes share a common ancestor.
  
  Consider the following interfaces and classes:

<pre class="codeblock">public interface Animal {
    default public String identifyMyself() {
        return "I am an animal.";
    }
}</code></pre>

<pre class="codeblock">public interface EggLayer extends Animal {
    default public String identifyMyself() {
        return "I am able to lay eggs.";
    }
}</code></pre>

<pre class="codeblock">public interface FireBreather extends Animal { }</code></pre>

<pre class="codeblock">public class Dragon implements EggLayer, FireBreather {
    public static void main (String... args) {
        Dragon myApp = new Dragon();
        System.out.println(myApp.identifyMyself());
    }
}</code></pre>

The method `Dragon.identifyMyself` returns the string `I am able to lay eggs.`
</li>

```

public interface Mammal {
    String identifyMyself();
}

```

```

public class Horse {
    public String identifyMyself() {
        return "I am a horse.";
    }
}

```

```

public class Mustang extends Horse implements Mammal {
    public static void main(String... args) {
        Mustang myApp = new Mustang();
        System.out.println(myApp.identifyMyself());
    }
}

```

The method `Mustang.identifyMyself` returns the string `I am a horse.` The class `Mustang` inherits the method `identifyMyself` from the class `Horse`, which overrides the abstract method of the same name in the interface `Mammal`.

**Note**: Static methods in interfaces are never inherited.

## Modifiers

The access specifier for an overriding method can allow more, but not less, access than the overridden method. For example, a protected instance method in the superclass can be made public, but not private, in the subclass.

You will get a compile-time error if you attempt to change an instance method in the superclass to a static method in the subclass, and vice versa.

## Summary


The following table summarizes what happens when you define a method with the same signature as a method in a superclass.
<th id="h1">&#160;</th><th id="h2">Superclass Instance Method</th><th id="h3">Superclass Static Method</th>
<th id="h4">Subclass Instance Method</th><td headers="h1">Overrides</td><td headers="h2">Generates a compile-time error</td>
<th id="h5">Subclass Static Method</th><td headers="h1">Generates a compile-time error</td><td headers="h2">Hides</td>
