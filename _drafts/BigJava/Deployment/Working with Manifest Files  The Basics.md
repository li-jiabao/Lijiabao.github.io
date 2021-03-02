
# Working with Manifest Files: The Basics

JAR files support a wide range of functionality, including electronic signing, version control, package sealing, and others. What gives a JAR file this versatility? The answer is the JAR file's **manifest**.

The manifest is a special file that can contain information about the files packaged in a JAR file. By tailoring this "meta" information that the manifest contains, you enable the JAR file to serve a variety of purposes.

This lesson will explain the contents of the manifest file and show you how to work with it, with examples for the basic features:

## 
[Understanding the Default Manifest](defman.html)

When you create a JAR file, a default manifest is created automatically. This section describes the default manifest.

## 
[Modifying a Manifest File](modman.html)

This section shows you the basic method of modifying a manifest file. The later sections demonstrate specific modifications you may want to make.

## 
[Setting an Application's Entry Point](appman.html)

This section describes how to use the <tt>Main-Class</tt> header in the manifest file to set an application's entry point.

## 
[Adding Classes to the JAR File's Classpath](downman.html)

This section describes how to use the <tt>Class-Path</tt> header in the manifest file to add classes in other JAR files to the classpath when running an applet or application.

## 
[Setting Package Version Information](packageman.html)

This section describes how to use the package version headers in the manifest file.

## 
[Sealing Packages within a JAR File](sealman.html)

This section describes how to seal packages within a JAR file by modifying the manifest file.

## 
[Enhancing Security with Manifest Attributes](secman.html)

This section describes how to use manifest attributes to increase the security of an applet or Java Web Start application.

## Additional Information

A 
[specification](https://docs.oracle.com/javase/8/docs/technotes/guides/jar/jar.html#JARManifest) of the manifest format is part of the on-line JDK documentation.
