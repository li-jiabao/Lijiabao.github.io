
# Obtaining Names of Method Parameters

You can obtain the names of the formal parameters of any method or constructor with the method
[`java.lang.reflect.Executable.getParameters`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Executable.html#getParameters--). (The classes
[`Method`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html) and 
[`Constructor`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Executable.html) extend the class 
[`Executable`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Executable.html) and therefore inherit the method `Executable.getParameters`.) However, `.class` files do not store formal parameter names by default. This is because many tools that produce and consume class files may not expect the larger static and dynamic footprint of `.class` files that contain parameter names. In particular, these tools would have to handle larger `.class` files, and the Java Virtual Machine (JVM) would use more memory. In addition, some parameter names, such as `secret` or `password`, may expose information about security-sensitive methods.

To store formal parameter names in a particular `.class` file, and thus enable the Reflection API to retrieve formal parameter names, compile the source file with the `-parameters` option to the `javac` compiler.

The 
[`<code>MethodParameterSpy`</code>](example/MethodParameterSpy.java) example illustrates how to retrieve the names of the formal parameters of all constructors and methods of a given class. The example also prints other information about each parameter.

The following command prints the formal parameter names of the constructors and methods of the class
[`<code>ExampleMethods`</code>](example/ExampleMethods.java). **Note**: Remember to compile the example `ExampleMethods` with the `-parameters` compiler option:

```
Parameter</code></a> class:</p>

<ul>

  <li><p>
[`getType`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Parameter.html#getType--): Returns a
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) object that identifies the declared type for the parameter.</p></li>
  
  <li><p>
[`getName`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Parameter.html#getName--): Returns the name of the parameter. If the parameter's name is present, then this method returns the name provided by the `.class` file. Otherwise, this method synthesizes a name of the form `arg**N**`, where `**N**` is the index of the parameter in the descriptor of the method that declares the parameter.</p>
  
  For example, suppose you compiled the class `ExampleMethods` without specifying the `-parameters` compiler option. The example `MethodParameterSpy` would print the following for the method `ExampleMethods.simpleMethod`:
  
  <pre class="codeblock">public boolean ExampleMethods.simpleMethod(java.lang.String,int)
             Return type: boolean
     Generic return type: boolean
         Parameter class: class java.lang.String
          Parameter name: arg0
               Modifiers: 0
            Is implicit?: false
        Is name present?: false
           Is synthetic?: false
         Parameter class: int
          Parameter name: arg1
               Modifiers: 0
            Is implicit?: false
        Is name present?: false
           Is synthetic?: false
```


  <li><p>
[`getType`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Parameter.html#getType--): Returns a
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) object that identifies the declared type for the parameter.</p></li>
  
  <li><p>
[`getName`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Parameter.html#getName--): Returns the name of the parameter. If the parameter's name is present, then this method returns the name provided by the `.class` file. Otherwise, this method synthesizes a name of the form `arg**N**`, where `**N**` is the index of the parameter in the descriptor of the method that declares the parameter.</p>
  
  For example, suppose you compiled the class `ExampleMethods` without specifying the `-parameters` compiler option. The example `MethodParameterSpy` would print the following for the method `ExampleMethods.simpleMethod`:
  
  <pre class="codeblock">public boolean ExampleMethods.simpleMethod(java.lang.String,int)
             Return type: boolean
     Generic return type: boolean
         Parameter class: class java.lang.String
          Parameter name: arg0
               Modifiers: 0
            Is implicit?: false
        Is name present?: false
           Is synthetic?: false
         Parameter class: int
          Parameter name: arg1
               Modifiers: 0
            Is implicit?: false
        Is name present?: false
           Is synthetic?: false</code></pre></li>

  <li><p>
[`getModifiers`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Parameter.html#getModifiers--)  : Returns an integer that represents various characteristics that the formal parameter possesses. This value is the sum of the following values, if applicable to the formal parameter:</p> 
  
  <table border="1" summary="Possible values of the getModifiers method">
      <tr>
          <th id="h1">Value (in decimal)</th>
          <th id="h2">Value (in hexadecimal</th>
          <th id="h3">Description</th>
      </tr>
      <tr>
          <td headers="h1">16</td>
          <td headers="h2">0x0010</td>
          <td headers="h3">The formal parameter is declared `final`</td>
      </tr>
      <tr>
          <td headers="h1">4096</td>
          <td headers="h2">0x1000</td>
          <td headers="h3">The formal parameter is synthetic. Alternatively, you can invoke the method `isSynthetic`.</td>
      </tr>
      <tr>
          <td headers="h1">32768</td>
          <td headers="h2">0x8000</td>
          <td headers="h3">The parameter is implicitly declared in source code. Alternatively, you can invoke the method `isImplicit`</td>
      </tr>
  </table>
  </li>  
        
  <li><p>
[`isImplicit`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Parameter.html#isImplicit--): Returns `true` if this parameter is implicitly declared in source code. See the section [Implicit and Synthetic Parameters](#implcit_and_synthetic) for more information.</p></li>
  
  <li><p>
[`isNamePresent`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Parameter.html#isNamePresent--): Returns `true` if the parameter has a name according to the `.class` file.</p></li>
  
  <li><p>
[`isSynthetic`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Parameter.html#isSynthetic--): Returns `true` if this parameter is neither implicitly nor explicitly declared in source code. See the section [Implicit and Synthetic Parameters](#implcit_and_synthetic) for more information.</p></li>

## <a name="implcit_and_synthetic" id="implcit_and_synthetic">Implicit and Synthetic Parameters</a>

Certain constructs are implicitly declared in the source code if they have not been written explicitly. For example, the
[`<code>ExampleMethods`</code>](example/ExampleMethods.java) example does not contain a constructor. A default constructor is implicitly declared for it. The `MethodParameterSpy` example prints information about the implicitly declared constructor of `ExampleMethods`:

```
`MethodParameterExamples`</code></a>:</p>

<pre class="codeblock">public class MethodParameterExamples {
    public class InnerClass { }
}
```

The class `InnerClass` is a non-static
[nested class](../../java/javaOO/nested.html) or inner class. A constructor for inner classes is also implicitly declared. However, this constructor will contain a parameter. When the Java compiler compiles `InnerClass`, it creates a `.class` file that represents code similar to the following:

```
`MethodParameterExamples`</code></a>:</p>

<pre class="codeblock">public class MethodParameterExamples {
    enum Colors {
        RED, WHITE;
    }
}
```

When the Java compiler encounters an `enum` construct, it creates several methods that are compatible with the `.class` file structure and provide the expected functionality of the `enum` construct. For example, the Java compiler would create a `.class` file for the `enum` construct `Colors` that represents code similar to the following:

The Java compiler creates three constructors and methods for this `enum` construct: `Colors(String name, int ordinal)`, `Colors[] values()`, and `Colors valueOf(String name)`. The methods `values` and `valueOf` are implicitly declared. Consequently, their formal parameter names are implicitly declared as well.

The `enum` constructor `Colors(String name, int ordinal)` is a default constructor and it is implicitly declared. However, the formal parameters of this constructor (`name` and `ordinal`) are **not** implicitly declared. Because these formal parameters are neither explicitly or implicitly declared, they are synthetic. (The formal parameters for the default constructor of an `enum` construct are not implicitly declared because different compilers need not agree on the form of this constructor; another Java compiler might specify different formal parameters for it. When compilers compile expressions that use `enum` constants, they rely only on the  public static fields of the `enum` construct, which are implicitly declared, and not on their constructors or how these constants are initialized.)

Consequently, the example `MethodParameterExample` prints the following about the `enum` construct `Colors`:

Refer to the [Java Language Specification](https://docs.oracle.com/javase/specs/) for more information about implicitly declared constructs, including parameters that appear as implicit in the Reflection API.
