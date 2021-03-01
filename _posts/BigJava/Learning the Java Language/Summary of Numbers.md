
# Summary of Numbers

You use one of the wrapper classes &#8211; `Byte`, `Double`, `Float`, `Integer`, `Long`, or `Short` &#8211; to wrap a number of primitive type in an object. The Java compiler automatically wraps (boxes) primitives for you when necessary and unboxes them, again when necessary.

The `Number` classes include constants and useful class methods. The `MIN_VALUE` and `MAX_VALUE` constants contain the smallest and largest values that can be contained by an object of that type. The `byteValue`, `shortValue`, and similar methods convert one numeric type to another. The `valueOf` method converts a string to a number, and the `toString` method converts a number to a string.

To format a string containing numbers for output, you can use the `printf()` or `format()` methods in the `PrintStream` class. Alternatively, you can use the `NumberFormat` class to customize numerical formats using patterns.

The `Math` class contains a variety of class methods for performing mathematical functions, including exponential, logarithmic, and trigonometric methods. `Math` also includes basic arithmetic functions, such as absolute value and rounding, and a method, `random()`, for generating random numbers.
