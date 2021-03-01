
# Lesson: Creating and Using Extensions

Any set of packages or classes can easily be made to play the role of an extension. The first step in turning a set of classes into an extension is to bundle them in a JAR file. Once that's done, you can turn the software into an extension in two ways:

- by placing the JAR file in a special location in the directory structure of the Java Runtime Environment, in which case it's called an **installed** extension.
- by referencing the JAR file in a specified way from the manifest of the another JAR file, in which case it's called a **download** extension.

This lesson shows you how the extension mechanism works by using a simple "toy" extension as an example.

## 
[Installed Extensions](install.html)

In this section, you'll create a simple installed extension and see how extension software is treated as part of the platform by the runtime environment.

## 
[Download Extensions](download.html)

This section will show you how modify a JAR file's manifest so that the JAR-bundled software can make use of download extensions.

## 
[Understanding Extension Class Loading](load.html)

This section is a short detour that summarizes the Java platform's delegation model for loading classes, and shows how it relates to loading classes in extensions.

## 
[Creating Extensible Applications](spi.html)

This section discusses the mechanism used to extend an application, via plug-ins or modules, without modifying its original code base.

The next lesson, 
[Making Extensions Secure](../security/index.html) uses the same extension to show how the Java platform controls the security permissions that are granted to extensions.
