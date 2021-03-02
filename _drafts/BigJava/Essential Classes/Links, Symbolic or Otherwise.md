
# Links, Symbolic or Otherwise

As mentioned previously, the `java.nio.file` package, and the `Path` class in particular, is "link aware." Every `Path` method either detects what to do when a symbolic link is encountered, or it provides an option enabling you to configure the behavior when a symbolic link is encountered.

The discussion so far has been about 
[symbolic or **soft** links](path.html#symlink), but some file systems also support hard links. **Hard links** are more restrictive than symbolic links, as follows:

- The target of the link must exist.
- Hard links are generally not allowed on directories.
- Hard links are not allowed to cross partitions or volumes. Therefore, they cannot exist across file systems.
- A hard link looks, and behaves, like a regular file, so they can be hard to find.
- A hard link is, for all intents and purposes, the same entity as the original file. They have the same file permissions, time stamps, and so on. All attributes are identical.

Because of these restrictions, hard links are not used as often as symbolic links, but the `Path` methods work seamlessly with hard links.

Several methods deal specifically with links and are covered in the following sections:

- [Creating a Symbolic Link](#symLink)
- [Creating a Hard Link](#hardLink)
- [Detecting a Symbolic Link](#detect)
- [Finding the Target of a Link](#read)

## <a name="symLink" id="symLink">Creating a Symbolic Link</a>

If your file system supports it, you can create a symbolic link by using the 
[`createSymbolicLink(Path, Path, FileAttribute&lt;?&gt;)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#createSymbolicLink-java.nio.file.Path-java.nio.file.Path-java.nio.file.attribute.FileAttribute...-) method. The second `Path` argument represents the target file or directory and might or might not exist. The following code snippet creates a symbolic link with default permissions:

```

Path newLink = ...;
Path target = ...;
try {
    Files.createSymbolicLink(newLink, target);
} catch (IOException x) {
    System.err.println(x);
} catch (UnsupportedOperationException x) {
    // Some file systems do not support symbolic links.
    System.err.println(x);
}

```

The `FileAttributes` vararg enables you to specify initial file attributes that are set atomically when the link is created. However, this argument is intended for future use and is not currently implemented.

## <a name="hardLink" id="hardLink">Creating a Hard Link</a>

You can create a hard (or **regular**) link to an existing file by using the 
[`createLink(Path, Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#createLink-java.nio.file.Path-java.nio.file.Path-) method. The second `Path` argument locates the existing file, and it must exist or a `NoSuchFileException` is thrown. The following code snippet shows how to create a link:

```

Path newLink = ...;
Path existingFile = ...;
try {
    Files.createLink(newLink, existingFile);
} catch (IOException x) {
    System.err.println(x);
} catch (UnsupportedOperationException x) {
    // Some file systems do not
    // support adding an existing
    // file to a directory.
    System.err.println(x);
}

```

## <a name="detect" id="detect">Detecting a Symbolic Link</a>

To determine whether a `Path` instance is a symbolic link, you can use the 
[`isSymbolicLink(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isSymbolicLink-java.nio.file.Path-) method. The following code snippet shows how:

```

Path file = ...;
boolean isSymbolicLink =
    Files.isSymbolicLink(file);

```

For more information, see 
[Managing Metadata](fileAttr.html).

## <a name="read" id="read">Finding the Target of a Link</a>

You can obtain the target of a symbolic link by using the 
[`readSymbolicLink(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#readSymbolicLink-java.nio.file.Path-) method, as follows:

```

Path link = ...;
try {
    System.out.format("Target of link" +
        " '%s' is '%s'%n", link,
        Files.readSymbolicLink(link));
} catch (IOException x) {
    System.err.println(x);
}

```

If the `Path` is not a symbolic link, this method throws a `NotLinkException`.
