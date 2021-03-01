
# Other Useful Methods

A few useful methods did not fit elsewhere in this lesson and are covered here. This section covers the following:

- [Determining MIME Type](#mime)
- [Default File System](#default)
- [Path String Separator](#separator)
- [File System's File Stores](#stores)

## <a name="mime" id="mime">Determining MIME Type</a>

To determine the MIME type of a file, you might find the 
[`probeContentType(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#probeContentType-java.nio.file.Path-) method useful. For example:

```

try {
    String type = Files.probeContentType(filename);
    if (type == null) {
        System.err.format("'%s' has an" + " unknown filetype.%n", filename);
    } else if (!type.equals("text/plain") {
        System.err.format("'%s' is not" + " a plain text file.%n", filename);
        continue;
    }
} catch (IOException x) {
    System.err.println(x);
}

```

Note that `probeContentType` returns null if the content type cannot be determined.

The implementation of this method is highly platform specific and is not infallible. The content type is determind by the platform's default file type detector. For example, if the detector determines a file's content type to be `application/x-java` based on the `.class` extension, it might be fooled.

You can provide a custom 
[`FileTypeDetector`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/spi/FileTypeDetector.html) if the default is not sufficient for your needs.

The 
[`<code>Email`</code>](examples/Email.java) example uses the `probeContentType` method.

## <a name="default" id="default">Default File System</a>

To retrieve the default file system, use the 
[`getDefault`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystems.html#getDefault--) method. Typically, this `FileSystems` method (note the plural) is chained to one of the `FileSystem` methods (note the singular), as follows:

```

PathMatcher matcher =
    FileSystems.getDefault().getPathMatcher("glob:*.*");

```

## <a name="separator" id="separator">Path String Separator</a>

The path separator for POSIX file systems is the forward slash, `/`, and for Microsoft Windows is the backslash, `\`. Other file systems might use other delimiters. To retrieve the `Path` separator for the default file system, you can use one of the following approaches:

```

String separator = File.separator;
String separator = FileSystems.getDefault().getSeparator();

```

The 
[`getSeparator`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystem.html#getSeparator--) method is also used to retrieve the path separator for any available file system.

## <a name="stores" id="stores">File System's File Stores</a>

A file system has one or more file stores to hold its files and directories. The **file store** represents the underlying storage device. In UNIX operating systems, each mounted file system is represented by a file store. In Microsoft Windows, each volume is represented by a file store: `C:`, `D:`, and so on.

To retrieve a list of all the file stores for the file system, you can use the 
[`getFileStores`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystem.html#getFileStores--) method. This method returns an `Iterable`, which allows you to use the 
[enhanced for](../../java/nutsandbolts/for.html) statement to iterate over all the root directories.

```

for (FileStore store: FileSystems.getDefault().getFileStores()) {
   ...
}

```

If you want to retrive the file store where a particular file is located, use the 
[`getFileStore`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#getFileStore-java.nio.file.Path-) method in the `Files` class, as follows:

```

Path file = ...;
FileStore store= Files.getFileStore(file);

```

The 
[`DiskUsage`](examples/DiskUsage.java) example uses the `getFileStores` method.
