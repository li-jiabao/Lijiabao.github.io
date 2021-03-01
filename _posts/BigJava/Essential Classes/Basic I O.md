
# Lesson: Basic I/O

This lesson covers the Java platform classes used for basic I/O. It first focuses on **I/O Streams**, a powerful concept that greatly simplifies I/O operations. The lesson also looks at serialization, which lets a program write whole objects out to streams and read them back again. Then the lesson looks at file I/O and file system operations, including random access files.

Most of the classes covered in the `I/O Streams` section are in the `java.io` package. Most of the classes covered in the `File I/O` section are in the `java.nio.file` package.

## [I/O Streams](streams.html)

- [Byte Streams](bytestreams.html) handle I/O of raw binary data.
- [Character Streams](charstreams.html) handle I/O of character data, automatically handling translation to and from the local character set.
- [Buffered Streams](buffers.html) optimize input and output by reducing the number of calls to the native API.
- [Scanning and Formatting](scanfor.html) allows a program to read and write formatted text.
- [I/O from the Command Line](cl.html) describes the Standard Streams and the Console object.
- [Data Streams](datastreams.html) handle binary I/O of primitive data type and `String` values.
- [Object Streams](objectstreams.html) handle binary I/O of objects.

## [File I/O (Featuring NIO.2)](fileio.html)

- [What is a Path?](path.html) examines the concept of a path on a file system.
- [The Path Class](pathClass.html) introduces the cornerstone class of the `java.nio.file` package.
- [Path Operations](pathOps.html) looks at methods in the `Path` class that deal with syntactic operations.
- [File Operations](fileOps.html) introduces concepts common to many of the file I/O methods.
- [Checking a File or Directory](check.html) shows how to check a file's existence and its level of accessibility.
- [Deleting a File or Directory](delete.html).
- [Copying a File or Directory](copy.html).
- [Moving a File or Directory](move.html).
- [Managing Metadata](fileAttr.html) explains how to read and set file attributes.
- [Reading, Writing and Creating Files](file.html) shows the stream and channel methods for reading and writing files.
- [Random Access Files](rafs.html) shows how to read or write files in a non-sequentially manner.
- [Creating and Reading Directories](dirs.html) covers API specific to directories, such as how to list a directory's contents.
- [Links, Symbolic or Otherwise](links.html) covers issues specific to symbolic and hard links.
- [Walking the File Tree](walk.html) demonstrates how to recursively visit each file and directory in a file tree.
- [Finding Files](find.html) shows how to search for files using pattern matching.
- [Watching a Directory for Changes](notification.html) shows how to use the watch service to detect files that are added, removed or updated in one or more directories.
- [Other Useful Methods](misc.html) covers important API that didn't fit elsewhere in the lesson.
- [Legacy File I/O Code](legacy.html) shows how to leverage `Path` functionality if you have older code using the `java.io.File` class. A table mapping `java.io.File` API to `java.nio.file` API is provided.

## [Summary](summary.html)

A summary of the key points covered in this trail.

## [Questions and Exercises](QandE/questions.html)

Test what you've learned in this trail by trying these questions and exercises.

## The I/O Classes in Action

Many of the examples in the next trail, 
[Custom Networking](../../networking/index.html) use the I/O streams described in this lesson to read from and write to network connections.
