
# File I/O (Featuring NIO.2)

**Note:** This tutorial reflects the file I/O mechanism introduced in the JDK 7 release. The Java SE 6 version of the File I/O tutorial was brief, but you can download the 
[Java SE Tutorial 2008-03-14](http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-tutorials-419421.html#tutorial-2008_03_14-oth-JPR) version of the tutorial which contains the earlier File I/O content.

The `java.nio.file` package and its related package, `java.nio.file.attribute`, provide comprehensive support for file I/O and for accessing the default file system. Though the API has many classes, you need to focus on only a few entry points. You will see that this API is very intuitive and easy to use.

The tutorial starts by asking 
[what is a path?](path.html) Then, the 
[Path class](pathClass.html), the primary entry point for the package, is introduced. Methods in the `Path` class relating to 
[syntactic operations](pathOps.html) are explained. The tutorial then moves on to the other primary class in the package, the `Files` class, which contains methods that deal with file operations. First, some concepts common to many 
[file operations](fileOps.html) are introduced. The tutorial then covers methods for 
[checking](check.html), 
[deleting](delete.html), 
[copying](copy.html), and 
[moving](move.html) files.

The tutorial shows how 
[metadata](fileAttr.html) is managed, before moving on to 
[file I/O](file.html) and 
[directory I/O](dirs.html). 
[Random access files](rafs.html) are explained and issues specific to 
[symbolic and hard links](links.html) are examined.

Next, some of the very powerful, but more advanced, topics are covered. First, the capability to 
[recursively walk the file tree](walk.html) is demonstrated, followed by information about how to 
[search for files using wild cards](find.html). Next, how to 
[watch a directory for changes](notification.html) is explained and demonstrated. Then, 
[methods that didn't fit elsewhere](misc.html) are given some attention.

Finally, if you have file I/O code written prior to the Java SE 7 release, there is a 
[map from the old API to the new API](legacy.html#mapping), as well as important information about the `File.toPath` method for developers who would like to 
[leverage the new API without rewriting existing code](legacy.html#interop).
