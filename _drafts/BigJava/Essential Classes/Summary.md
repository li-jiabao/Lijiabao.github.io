
# Summary

The `java.io` package contains many classes that your programs can use to read and write data. Most of the classes implement sequential access streams. The sequential access streams can be divided into two groups: those that read and write bytes and those that read and write Unicode characters. Each sequential access stream has a speciality, such as reading from or writing to a file, filtering data as its read or written, or serializing an object.

The `java.nio.file` package provides extensive support for file and file system I/O. This is a very comprehensive API, but the key entry points are as follows:

- The `Path` class has methods for manipulating a path.
- The `Files` class has methods for file operations, such as moving, copy, deleting, and also methods for retrieving and setting file attributes.
- The `FileSystem` class has a variety of methods for obtaining information about the file system.

More information on NIO.2 can be found on the 
[OpenJDK: NIO](http://openjdk.java.net/projects/nio/) project website. This site includes resources for features provided by NIO.2 that are beyond the scope of this tutorial, such as multicasting, asynchronous I/O, and creating your own file system implementation.
