
# Checking a File or Directory

You have a `Path` instance representing a file or directory, but does that file exist on the file system? Is it readable? Writable? Executable?

## <a name="verify" id="verify">Verifying the Existence of a File or Directory</a>

The methods in the `Path` class are syntactic, meaning that they operate on the `Path` instance. But eventually you must access the file system to verify that a particular `Path` exists, or does not exist. You can do so with the 
[`exists(Path, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#exists-java.nio.file.Path-java.nio.file.LinkOption...-) and the 
[`notExists(Path, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#notExists-java.nio.file.Path-java.nio.file.LinkOption...-) methods. Note that `!Files.exists(path)` is not equivalent to `Files.notExists(path)`. When you are testing a file's existence, three results are possible:

- The file is verified to exist.
- The file is verified to not exist.
- The file's status is unknown. This result can occur when the program does not have access to the file.

If both `exists` and `notExists` return `false`, the existence of the file cannot be verified.

## <a name="access" id="access">Checking File Accessibility</a>

To verify that the program can access a file as needed, you can use the 
[`isReadable(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isReadable-java.nio.file.Path-), 
[`isWritable(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isWritable-java.nio.file.Path-), and 
[`isExecutable(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isExecutable-java.nio.file.Path-) methods.

The following code snippet verifies that a particular file exists and that the program has the ability to execute the file.

```

Path file = ...;
boolean isRegularExecutableFile = Files.isRegularFile(file) &amp;
         Files.isReadable(file) &amp; Files.isExecutable(file);

```

## <a name="same" id="same">Checking Whether Two Paths Locate the Same File</a>

When you have a file system that uses symbolic links, it is possible to have two different paths that locate the same file. The 
[`isSameFile(Path, Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isSameFile-java.nio.file.Path-java.nio.file.Path-) method compares two paths to determine if they locate the same file on the file system. For example:

```

Path p1 = ...;
Path p2 = ...;

if (Files.isSameFile(p1, p2)) {
    // Logic when the paths locate the same file
}

```
