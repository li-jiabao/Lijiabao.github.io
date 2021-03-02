
# Lesson: Packaging Programs in JAR Files

The Java&#8482; Archive (JAR) file format enables you to bundle multiple files into a single archive file. Typically a JAR file contains the class files and auxiliary resources associated with applets and applications.

The JAR file format provides many benefits:

- **Security**: You can digitally sign the contents of a JAR file. Users who recognize your signature can then optionally grant your software security privileges it wouldn't otherwise have.
- **Decreased download time**: If your applet is bundled in a JAR file, the applet's class files and associated resources can be downloaded to a browser in a single HTTP transaction without the need for opening a new connection for each file.
- **Compression**: The JAR format allows you to compress your files for efficient storage.
- **Packaging for extensions**: The extensions framework provides a means by which you can add functionality to the Java core platform, and the JAR file format defines the packaging for extensions. By using the JAR file format, you can turn your software into extensions as well.
- **Package Sealing**: Packages stored in JAR files can be optionally sealed so that the package can enforce version consistency. Sealing a package within a JAR file means that all classes defined in that package must be found in the same JAR file.
- **Package Versioning**: A JAR file can hold data about the files it contains, such as vendor and version information.
- **Portability**: The mechanism for handling JAR files is a standard part of the Java platform's core API.

This lesson has four sections: <!--    Using JAR Files: The Basics    -->

## 
[Using JAR Files: The Basics](basicsindex.html)

This section shows you how to perform basic JAR-file operations, and how to run software that is bundled in JAR files. <!--    Working with Manifest Files: The Basics    -->

## 
[Working with Manifest Files: The Basics](manifestindex.html)

This section explains manifest files and how to customize them so you can do such things as seal packages and set an application's entry point. <!--    Signing JAR files    -->

## 
[Signing and Verifying JAR Files](signindex.html)

This section shows you how to digitally sign JAR files and verify the signatures of signed JAR files.

## 
[Using JAR-related APIs](apiindex.html)

This section introduces you to some of the JAR-handling features of the Java platform. The JAR file format is an important part of the Java platform's extension mechanism. You can learn more about that aspect of JAR files in the [The Extension Mechanism](../../ext/index.html) trail of this tutorial.

## 
[Questions and Exercises: JAR](QandE/questions.html)

Test what you've learned about JAR. <!--Additional References -->

## Additional References

The documentation for the Java Development Kit (JDK) includes information about the Jar tool:

<li>
[Java Archive (JAR) Files Guide](https://docs.oracle.com/javase/8/docs/technotes/guides/jar/index.html)</li>
<li>
[JAR File Specification](https://docs.oracle.com/javase/8/docs/technotes/guides/jar/jar.html)</li>
