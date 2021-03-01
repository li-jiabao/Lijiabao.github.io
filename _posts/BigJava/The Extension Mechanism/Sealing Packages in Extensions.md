
# Trail: The Reflection API

## Uses of Reflection

Reflection is commonly used by programs which require the ability to examine or modify the runtime behavior of applications running in the Java virtual machine. This is a relatively advanced feature and should be used only by developers who have a strong grasp of the fundamentals of the language. With that caveat in mind, reflection is a powerful technique and can enable applications to perform operations which would otherwise be impossible.

## Drawbacks of Reflection

Reflection is powerful, but should not be used indiscriminately. If it is possible to perform an operation without using reflection, then it is preferable to avoid using it. The following concerns should be kept in mind when accessing code via reflection.

## Trail Lessons

This trail covers common uses of reflection for accessing and manipulating classes, fields, methods, and constructors. Each lesson contains code examples, tips, and troubleshooting information.

The examples in this trail are designed for experimenting with the Reflection APIs. The handling of exceptions therefore is not the same as would be used in production code. In particular, in production code it is not recommended to dump stack traces that are visible to the user.
