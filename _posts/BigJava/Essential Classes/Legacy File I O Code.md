
# Legacy File I/O Code

## <a name="interop" id="interop">Interoperability With Legacy Code</a>

Prior to the Java SE 7 release, the `java.io.File` class was the mechanism used for file I/O, but it had several drawbacks.

- Many methods didn't throw exceptions when they failed, so it was impossible to obtain a useful error message. For example, if a file deletion failed, the program would receive a "delete fail" but wouldn't know if it was because the file didn't exist, the user didn't have permissions, or there was some other problem.
- The `rename` method didn't work consistently across platforms.
- There was no real support for symbolic links.
- More support for metadata was desired, such as file permissions, file owner, and other security attributes.
- Accessing file metadata was inefficient.
- Many of the `File` methods didn't scale. Requesting a large directory listing over a server could result in a hang. Large directories could also cause memory resource problems, resulting in a denial of service.
- It was not possible to write reliable code that could recursively walk a file tree and respond appropriately if there were circular symbolic links.

Perhaps you have legacy code that uses `java.io.File` and would like to take advantage of the `java.nio.file.Path` functionality with minimal impact to your code.

The `java.io.File` class provides the 
[`toPath`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#toPath--) method, which converts an old style `File` instance to a `java.nio.file.Path` instance, as follows:

```

Path input = file.toPath();

```

You can then take advantage of the rich feature set available to the `Path` class.

For example, assume you had some code that deleted a file:

```

file.delete();

```

You could modify this code to use the `Files.delete` method, as follows:

```

Path fp = file.toPath();
Files.delete(fp);

```

Conversely, the 
[`Path.toFile`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Path.html#toFile--) method constructs a `java.io.File` object for a `Path` object.

## <a name="mapping" id="mapping">Mapping java.io.File Functionality to java.nio.file</a>

Because the Java implementation of file I/O has been completely re-architected in the Java SE 7 release, you cannot swap one method for another method. If you want to use the rich functionality offered by the `java.nio.file` package, your easiest solution is to use the 
[`File.toPath`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#toPath--) method as suggested in the previous section. However, if you do not want to use that approach or it is not sufficient for your needs, you must rewrite your file I/O code.

There is no one-to-one correspondence between the two APIs, but the following table gives you a general idea of what functionality in the `java.io.File` API maps to in the `java.nio.file` API and tells you where you can obtain more information.
<th id="h1">java.io.File Functionality</th><th id="h2">java.nio.file Functionality</th><th id="h3">Tutorial Coverage</th>
<td headers="h1">`java.io.File`</td><td headers="h2">`java.nio.file.Path`</td><td headers="h3">[The Path Class](pathClass.html)</td>
<td headers="h1">`java.io.RandomAccessFile`</td><td headers="h2">The `SeekableByteChannel` functionality.</td><td headers="h3">[Random Access Files](rafs.html)</td>
<td headers="h1">`File.canRead`, `canWrite`, `canExecute`</td><td headers="h2">`Files.isReadable`, `Files.isWritable`, and `Files.isExecutable`.<br />On UNIX file systems, the [Managing Metadata (File and File Store Attributes)](fileAttr.html) package is used to check the nine file permissions.</td><td headers="h3">[Checking a File or Directory](check.html)<br />[Managing Metadata](fileAttr.html)</td>
<td headers="h1">`File.isDirectory()`, `File.isFile()`, and `File.length()`</td><td headers="h2">`Files.isDirectory(Path, LinkOption...)`, `Files.isRegularFile(Path, LinkOption...)`, and `Files.size(Path)`</td><td headers="h3">[Managing Metadata](fileAttr.html)</td>
<td headers="h1">`File.lastModified()` and `File.setLastModified(long)`</td><td headers="h2">`Files.getLastModifiedTime(Path, LinkOption...)` and `Files.setLastMOdifiedTime(Path, FileTime)`</td><td headers="h3">[Managing Metadata](fileAttr.html)</td>
<td headers="h1">The `File` methods that set various attributes: `setExecutable`, `setReadable`, `setReadOnly`, `setWritable`</td><td headers="h2">These methods are replaced by the `Files` method `setAttribute(Path, String, Object, LinkOption...)`.</td><td headers="h3">[Managing Metadata](fileAttr.html)</td>
<td headers="h1">`new File(parent, "newfile")`</td><td headers="h2">`parent.resolve("newfile")`</td><td headers="h3">[Path Operations](pathOps.html)</td>
<td headers="h1">`File.renameTo`</td><td headers="h2">`Files.move`</td><td headers="h3">[Moving a File or Directory](move.html)</td>
<td headers="h1">`File.delete`</td><td headers="h2">`Files.delete`</td><td headers="h3">[Deleting a File or Directory](delete.html)</td>
<td headers="h1">`File.createNewFile`</td><td headers="h2">`Files.createFile`</td><td headers="h3">[Creating Files](file.html#createFile)</td>
<td headers="h1">`File.deleteOnExit`</td><td headers="h2">Replaced by the `DELETE_ON_CLOSE` option specified in the `createFile` method.</td><td headers="h3">[Creating Files](file.html#createFile)</td>
<td headers="h1">`File.createTempFile`</td><td headers="h2">`Files.createTempFile(Path, String, FileAttributes&lt;?&gt;)`, `Files.createTempFile(Path, String, String, FileAttributes&lt;?&gt;)`</td><td headers="h3">[Creating Files](file.html#createFile)<br />[Creating and Writing a File by Using Stream I/O](file.html#createStream)<br />[Reading and Writing Files by Using Channel I/O](file.html#channelio)</td>
<td headers="h1">`File.exists`</td><td headers="h2">`Files.exists` and `Files.notExists`</td><td headers="h3">[Verifying the Existence of a File or Directory](check.html)</td>
<td headers="h1">`File.compareTo` and `equals`</td><td headers="h2">`Path.compareTo` and `equals`</td><td headers="h3">[Comparing Two Paths](pathOps.html#compare)</td>
<td headers="h1">`File.getAbsolutePath` and `getAbsoluteFile`</td><td headers="h2">`Path.toAbsolutePath`</td><td headers="h3">[Converting a Path](pathOps.html#convert)</td>
<td headers="h1">`File.getCanonicalPath` and `getCanonicalFile`</td><td headers="h2">`Path.toRealPath` or `normalize`</td><td headers="h3">[Converting a Path (`toRealPath`)](pathOps.html#convert)<br />[Removing Redundancies From a Path (`normalize`)](pathOps.html#normal)<br /></td>
<td headers="h1">`File.toURI`</td><td headers="h2">`Path.toURI`</td><td headers="h3">[Converting a Path](pathOps.html#convert)</td>
<td headers="h1">`File.isHidden`</td><td headers="h2">`Files.isHidden`</td><td headers="h3">[Retrieving Information About the Path](pathOps.html#info)</td>
<td headers="h1">`File.list` and `listFiles`</td><td headers="h2">`Path.newDirectoryStream`</td><td headers="h3">[Listing a Directory's Contents](dirs.html#listdir)</td>
<td headers="h1">`File.mkdir` and `mkdirs`</td><td headers="h2">`Files.createDirectory`</td><td headers="h3">[Creating a Directory](dirs.html#create)</td>
<td headers="h1">`File.listRoots`</td><td headers="h2">`FileSystem.getRootDirectories`</td><td headers="h3">[Listing a File System's Root Directories](dirs.html#listall)</td>
<td headers="h1">`File.getTotalSpace`, `File.getFreeSpace`, `File.getUsableSpace`</td><td headers="h2">`FileStore.getTotalSpace`, `FileStore.getUnallocatedSpace`, `FileStore.getUsableSpace`, `FileStore.getTotalSpace`</td><td headers="h3">[File Store Attributes](fileAttr.html#store)</td>
