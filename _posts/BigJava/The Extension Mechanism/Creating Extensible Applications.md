
# Lesson: Making Extensions Secure

Now that you have seen how to use extensions, you may be wondering what security privileges extensions have. If you are developing an extension that does file I/O, for example, you will need to know how your extension is granted the appropriate permissions for reading and writing files. Conversely, if you are thinking about using an extension developed by someone else, you will want to understand clearly what security privileges the extension has and how to change those privileges should you desire to do so.

This lesson shows you how the Java&#8482; platform's security architecture treats extensions. You will see how to tell what privileges are granted to extension software, and you will learn how to modify extension privileges by following some simple steps. In addition, you will learn how to seal packages within your extensions to restrict access to specified parts of your code.

This lesson has two sections:

## 
[Setting Privileges for Extensions](policy.html)

This section contains some examples that show you what conditions must be met for extensions to be granted permissions to perform security-sensitive operations.

## 
[Sealing Packages in Extensions](sealing.html)

You can optionally seal packages in extension JAR files as an additional security measure. If a package is sealed, it means that all classes defined in that package must originate from a single JAR file. This section shows you how to modify an extension's manifest to seal extension packages.

## Additional Documentation

You will find links and references to relevant security documentation at appropriate places throughout this lesson.

For complete information on security, you can refer to the following:

<li>
[Security Features in Java SE](../../security/index.html) trail (in this tutorial)
</li>
<li>
[Security guide](https://docs.oracle.com/javase/8/docs/technotes/guides/security/)</li>
