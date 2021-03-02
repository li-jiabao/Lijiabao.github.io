
# Managing Metadata (File and File Store Attributes)

The definition of **metadata** is "data about other data." With a file system, the data is contained in its files and directories, and the metadata tracks information about each of these objects: Is it a regular file, a directory, or a link? What is its size, creation date, last modified date, file owner, group owner, and access permissions?

A file system's metadata is typically referred to as its **file attributes**. The `Files` class includes methods that can be used to obtain a single attribute of a file, or to set an attribute.
<th id="h1">Methods</th><th id="h2">Comment</th>
<td headers="h1">[`size(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#size-java.nio.file.Path-)</td><td headers="h2">Returns the size of the specified file in bytes.</td>
<td headers="h1">[`isDirectory(Path, LinkOption)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isDirectory-java.nio.file.Path-java.nio.file.LinkOption...-)</td><td headers="h2">Returns true if the specified `Path` locates a file that is a directory.</td>
<td headers="h1">[`isRegularFile(Path, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isRegularFile-java.nio.file.Path-java.nio.file.LinkOption...-)</td><td headers="h2">Returns true if the specified `Path` locates a file that is a regular file.</td>
<td headers="h1">[`isSymbolicLink(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isSymbolicLink-java.nio.file.Path-)</td><td headers="h2">Returns true if the specified `Path` locates a file that is a symbolic link.</td>
<td headers="h1">[`isHidden(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isHidden-java.nio.file.Path-)</td><td headers="h2">Returns true if the specified `Path` locates a file that is considered hidden by the file system.</td>
<td headers="h1">[`getLastModifiedTime(Path, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#getLastModifiedTime-java.nio.file.Path-java.nio.file.LinkOption...-)<br />[`setLastModifiedTime(Path, FileTime)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#setLastModifiedTime-java.nio.file.Path-java.nio.file.attribute.FileTime-)</td><td headers="h2">Returns or sets the specified file's last modified time.</td>
<td headers="h1">[`getOwner(Path, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#getOwner-java.nio.file.Path-java.nio.file.LinkOption...-)<br />[`setOwner(Path, UserPrincipal)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#setOwner-java.nio.file.Path-java.nio.file.attribute.UserPrincipal-)</td><td headers="h2">Returns or sets the owner of the file.</td>
<td headers="h1">[`getPosixFilePermissions(Path, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#getPosixFilePermissions-java.nio.file.Path-java.nio.file.LinkOption...-)<br />[`setPosixFilePermissions(Path, Set&lt;PosixFilePermission&gt;)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#setPosixFilePermissions-java.nio.file.Path-java.util.Set-)</td><td headers="h2">Returns or sets a file's POSIX file permissions.</td>
<td headers="h1">[`getAttribute(Path, String, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#getAttribute-java.nio.file.Path-java.lang.String-java.nio.file.LinkOption...-)<br />[`setAttribute(Path, String, Object, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#setAttribute-java.nio.file.Path-java.lang.String-java.lang.Object-java.nio.file.LinkOption...-)</td><td headers="h2">Returns or sets the value of a file attribute.</td>

If a program needs multiple file attributes around the same time, it can be inefficient to use methods that retrieve a single attribute. Repeatedly accessing the file system to retrieve a single attribute can adversely affect performance. For this reason, the `Files` class provides two `readAttributes` methods to fetch a file's attributes in one bulk operation.
<th id="h101">Method</th><th id="h102">Comment</th>
<td headers="h101">[`readAttributes(Path, String, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#readAttributes-java.nio.file.Path-java.lang.String-java.nio.file.LinkOption...-)</td><td headers="h102">Reads a file's attributes as a bulk operation. The `String` parameter identifies the attributes to be read.</td>
<td headers="h101">[`readAttributes(Path, Class&lt;A&gt;, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#readAttributes-java.nio.file.Path-java.lang.Class-java.nio.file.LinkOption...-)</td><td headers="h102">Reads a file's attributes as a bulk operation. The `Class&lt;A&gt;` parameter is the type of attributes requested and the method returns an object of that class.</td>

Before showing examples of the `readAttributes` methods, it should be mentioned that different file systems have different notions about which attributes should be tracked. For this reason, related file attributes are grouped together into views. A **view** maps to a particular file system implementation, such as POSIX or DOS, or to a common functionality, such as file ownership.

The supported views are as follows:

<li>
[`BasicFileAttributeView`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/BasicFileAttributeView.html) &#8211; Provides a view of basic attributes that are required to be supported by all file system implementations.</li>
<li>
[`DosFileAttributeView`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/DosFileAttributeView.html) &#8211; Extends the basic attribute view with the standard four bits supported on file systems that support the DOS attributes.</li>
<li>
[`PosixFileAttributeView`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/PosixFileAttributeView.html) &#8211; Extends the basic attribute view with attributes supported on file systems that support the POSIX family of standards, such as UNIX. These attributes include file owner, group owner, and the nine related access permissions.</li>
<li>
[`FileOwnerAttributeView`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/FileOwnerAttributeView.html) &#8211; Supported by any file system implementation that supports the concept of a file owner.</li>
<li>
[`AclFileAttributeView`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/AclFileAttributeView.html) &#8211; Supports reading or updating a file's Access Control Lists (ACL). The NFSv4 ACL model is supported. Any ACL model, such as the Windows ACL model, that has a well-defined mapping to the NFSv4 model might also be supported.</li>
<li>
[`UserDefinedFileAttributeView`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/UserDefinedFileAttributeView.html) &#8211; Enables support of metadata that is user defined. This view can be mapped to any extension mechanisms that a system supports. In the Solaris OS, for example, you can use this view to store the MIME type of a file.</li>

A specific file system implementation might support only the basic file attribute view, or it may support several of these file attribute views. A file system implementation might support other attribute views not included in this API.

In most instances, you should not have to deal directly with any of the `FileAttributeView` interfaces. (If you do need to work directly with the `FileAttributeView`, you can access it via the 
[`getFileAttributeView(Path, Class&lt;V&gt;, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#getFileAttributeView-java.nio.file.Path-java.lang.Class-java.nio.file.LinkOption...-) method.)

The `readAttributes` methods use generics and can be used to read the attributes for any of the file attributes views. The examples in the rest of this page use the `readAttributes` methods.

The remainder of this section covers the following topics:

- [Basic File Attributes](#basic)
- [Setting Time Stamps](#time)
- [DOS File Attributes](#dos)
- [POSIX File Permissions](#posix)
- [Setting a File or Group Owner](#lookup)
- [User-Defined File Attributes](#user)
- [File Store Attributes](#store)

## <a name="basic" id="basic">Basic File Attributes</a>

As mentioned previously, to read the basic attributes of a file, you can use one of the `Files.readAttributes` methods, which reads all the basic attributes in one bulk operation. This is far more efficient than accessing the file system separately to read each individual attribute. The varargs argument currently supports the 
[`LinkOption`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/LinkOption.html) enum, `NOFOLLOW_LINKS`. Use this option when you do not want symbolic links to be followed.

The following code snippet reads and prints the basic file attributes for a given file and uses the methods in the 
[`BasicFileAttributes`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/BasicFileAttributes.html) class.

```

Path file = ...;
BasicFileAttributes attr = Files.readAttributes(file, BasicFileAttributes.class);

System.out.println("creationTime: " + attr.creationTime());
System.out.println("lastAccessTime: " + attr.lastAccessTime());
System.out.println("lastModifiedTime: " + attr.lastModifiedTime());

System.out.println("isDirectory: " + attr.isDirectory());
System.out.println("isOther: " + attr.isOther());
System.out.println("isRegularFile: " + attr.isRegularFile());
System.out.println("isSymbolicLink: " + attr.isSymbolicLink());
System.out.println("size: " + attr.size());

```

In addition to the accessor methods shown in this example, there is a `fileKey` method that returns either an object that uniquely identifies the file or `null` if no file key is available.

## <a name="time" id="time">Setting Time Stamps</a>

The following code snippet sets the last modified time in milliseconds:

```

Path file = ...;
BasicFileAttributes attr =
    Files.readAttributes(file, BasicFileAttributes.class);
long currentTime = System.currentTimeMillis();
FileTime ft = FileTime.fromMillis(currentTime);
Files.setLastModifiedTime(file, ft);
}

```

## <a name="dos" id="dos">DOS File Attributes</a>

DOS file attributes are also supported on file systems other than DOS, such as Samba. The following snippet uses the methods of the 
[`DosFileAttributes`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/DosFileAttributes.html) class.

```

Path file = ...;
try {
    DosFileAttributes attr =
        Files.readAttributes(file, DosFileAttributes.class);
    System.out.println("isReadOnly is " + attr.isReadOnly());
    System.out.println("isHidden is " + attr.isHidden());
    System.out.println("isArchive is " + attr.isArchive());
    System.out.println("isSystem is " + attr.isSystem());
} catch (UnsupportedOperationException x) {
    System.err.println("DOS file" +
        " attributes not supported:" + x);
}

```

However, you can set a DOS attribute using the 
[`setAttribute(Path, String, Object, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#setAttribute-java.nio.file.Path-java.lang.String-java.lang.Object-java.nio.file.LinkOption...-) method, as follows:

```

Path file = ...;
Files.setAttribute(file, "dos:hidden", true);

```

## <a name="posix" id="posix">POSIX File Permissions</a>

**POSIX** is an acronym for Portable Operating System Interface for UNIX and is a set of IEEE and ISO standards designed to ensure interoperability among different flavors of UNIX. If a program conforms to these POSIX standards, it should be easily ported to other POSIX-compliant operating systems.

Besides file owner and group owner, POSIX supports nine file permissions: read, write, and execute permissions for the file owner, members of the same group, and "everyone else."

The following code snippet reads the POSIX file attributes for a given file and prints them to standard output. The code uses the methods in the 
[`PosixFileAttributes`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/PosixFileAttributes.html) class.

```

Path file = ...;
PosixFileAttributes attr =
    Files.readAttributes(file, PosixFileAttributes.class);
System.out.format("%s %s %s%n",
    attr.owner().getName(),
    attr.group().getName(),
    PosixFilePermissions.toString(attr.permissions()));

```

The 
[`PosixFilePermissions`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/PosixFilePermissions.html) helper class provides several useful methods, as follows:

- The `toString` method, used in the previous code snippet, converts the file permissions to a string (for example, `rw-r--r--`).
- The `fromString` method accepts a string representing the file permissions and constructs a `Set` of file permissions.
- The `asFileAttribute` method accepts a `Set` of file permissions and constructs a file attribute that can be passed to the `Path.createFile` or `Path.createDirectory` method.

The following code snippet reads the attributes from one file and creates a new file, assigning the attributes from the original file to the new file:

```

Path sourceFile = ...;
Path newFile = ...;
PosixFileAttributes attrs =
    Files.readAttributes(sourceFile, PosixFileAttributes.class);
FileAttribute&lt;Set&lt;PosixFilePermission&gt;&gt; attr =
    PosixFilePermissions.asFileAttribute(attrs.permissions());
Files.createFile(file, attr);

```

The `asFileAttribute` method wraps the permissions as a `FileAttribute`. The code then attempts to create a new file with those permissions. Note that the `umask` also applies, so the new file might be more secure than the permissions that were requested.

To set a file's permissions to values represented as a hard-coded string, you can use the following code:

```

Path file = ...;
Set&lt;PosixFilePermission&gt; perms =
    PosixFilePermissions.fromString("rw-------");
FileAttribute&lt;Set&lt;PosixFilePermission&gt;&gt; attr =
    PosixFilePermissions.asFileAttribute(perms);
Files.setPosixFilePermissions(file, perms);

```

The 
[`<code>Chmod`</code>](examples/Chmod.java) example recursively changes the permissions of files in a manner similar to the `chmod` utility.

## <a name="lookup" id="lookup">Setting a File or Group Owner</a>

To translate a name into an object you can store as a file owner or a group owner, you can use the 
[`UserPrincipalLookupService`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/UserPrincipalLookupService.html) service. This service looks up a name or group name as a string and returns a `UserPrincipal` object representing that string. You can obtain the user principal look-up service for the default file system by using the 
[`FileSystem.getUserPrincipalLookupService`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystem.html#getUserPrincipalLookupService--) method.

The following code snippet shows how to set the file owner by using the `setOwner` method:

```

Path file = ...;
UserPrincipal owner = file.GetFileSystem().getUserPrincipalLookupService()
        .lookupPrincipalByName("sally");
Files.setOwner(file, owner);

```

There is no special-purpose method in the `Files` class for setting a group owner. However, a safe way to do so directly is through the POSIX file attribute view, as follows:

```

Path file = ...;
GroupPrincipal group =
    file.getFileSystem().getUserPrincipalLookupService()
        .lookupPrincipalByGroupName("green");
Files.getFileAttributeView(file, PosixFileAttributeView.class)
     .setGroup(group);

```

## <a name="user" id="user">User-Defined File Attributes</a>

If the file attributes supported by your file system implementation aren't sufficient for your needs, you can use the `UserDefinedAttributeView` to create and track your own file attributes.

Some implementations map this concept to features like NTFS Alternative Data Streams and extended attributes on file systems such as ext3 and ZFS. Most implementations impose restrictions on the size of the value, for example, ext3 limits the size to 4 kilobytes.

A file's MIME type can be stored as a user-defined attribute by using this code snippet:

```

Path file = ...;
UserDefinedFileAttributeView view = Files
    .getFileAttributeView(file, UserDefinedFileAttributeView.class);
view.write("user.mimetype",
           Charset.defaultCharset().encode("text/html");

```

To read the MIME type attribute, you would use this code snippet:

```

Path file = ...;
UserDefinedFileAttributeView view = Files
.getFileAttributeView(file,UserDefinedFileAttributeView.class);
String name = "user.mimetype";
ByteBuffer buf = ByteBuffer.allocate(view.size(name));
view.read(name, buf);
buf.flip();
String value = Charset.defaultCharset().decode(buf).toString();

```

The 
[`<code>Xdd`</code>](examples/Xdd.java) example shows how to get, set, and delete a user-defined attribute.

```

$ sudo mount -o remount,user_xattr /

```

If you want to make the change permanent, add an entry to `/etc/fstab`.

## <a name="store" id="store">File Store Attributes</a>

You can use the 
[`FileStore`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileStore.html) class to learn information about a file store, such as how much space is available. The 
[`getFileStore(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#getFileStore-java.nio.file.Path-) method fetches the file store for the specified file.

The following code snippet prints the space usage for the file store where a particular file resides:

```

Path file = ...;
FileStore store = Files.getFileStore(file);

long total = store.getTotalSpace() / 1024;
long used = (store.getTotalSpace() -
             store.getUnallocatedSpace()) / 1024;
long avail = store.getUsableSpace() / 1024;

```

The 
[`DiskUsage`](examples/DiskUsage.java) example uses this API to print disk space information for all the stores in the default file system. This example uses the 
[`getFileStores`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystem.html#getFileStores--) method in the `FileSystem` class to fetch all the file stores for the the file system.
