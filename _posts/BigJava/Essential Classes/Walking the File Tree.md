
# Walking the File Tree

Do you need to create an application that will recursively visit all the files in a file tree? Perhaps you need to delete every `.class` file in a tree, or find every file that hasn't been accessed in the last year. You can do so with the 
[`FileVisitor`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitor.html) interface.

This section covers the following:

- [The FileVisitor Interface](#filevisitor)
- [Kickstarting the Process](#invoke)
- [Considerations When Creating a FileVisitor](#order)
- [Controlling the Flow](#return)
- [Examples](#ex)

## <a name="filevisitor" id="filevisitor">The FileVisitor Interface</a>

To walk a file tree, you first need to implement a `FileVisitor`. A `FileVisitor` specifies the required behavior at key points in the traversal process: when a file is visited, before a directory is accessed, after a directory is accessed, or when a failure occurs. The interface has four methods that correspond to these situations:

<li>
[`preVisitDirectory`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitor.html#preVisitDirectory-T-java.nio.file.attribute.BasicFileAttributes-) &#8211; Invoked before a directory's entries are visited.</li>
<li>
[`postVisitDirectory`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitor.html#postVisitDirectory-T-java.io.IOException-) &#8211; Invoked after all the entries in a directory are visited. If any errors are encountered, the specific exception is passed to the method.</li>
<li>
[`visitFile`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitor.html#visitFile-T-java.nio.file.attribute.BasicFileAttributes-) &#8211; Invoked on the file being visited. The file's `BasicFileAttributes` is passed to the method, or you can use the 
[file attributes](fileAttr.html) package to read a specific set of attributes. For example, you can choose to read the file's `DosFileAttributeView` to determine if the file has the "hidden" bit set.</li>
<li>
[`visitFileFailed`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitor.html#visitFileFailedy-T-java.io.IOException-) &#8211; Invoked when the file cannot be accessed. The specific exception is passed to the method. You can choose whether to throw the exception, print it to the console or a log file, and so on.</li>

If you don't need to implement all four of the `FileVisitor` methods, instead of implementing the `FileVisitor` interface, you can extend the 
[`SimpleFileVisitor`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/SimpleFileVisitor.html) class. This class, which implements the `FileVisitor` interface, visits all files in a tree and throws an `IOError` when an error is encountered. You can extend this class and override only the methods that you require.

Here is an example that extends `SimpleFileVisitor` to print all entries in a file tree. It prints the entry whether the entry is a regular file, a symbolic link, a directory, or some other "unspecified" type of file. It also prints the size, in bytes, of each file. Any exception that is encountered is printed to the console.

The `FileVisitor` methods are shown in bold:

```

import static java.nio.file.FileVisitResult.*;

public static class PrintFiles
    extends SimpleFileVisitor&lt;Path&gt; {

    // Print information about
    // each type of file.
    @Override
    <strong>public FileVisitResult visitFile(Path file,
                                   BasicFileAttributes attr)</strong> {
        if (attr.isSymbolicLink()) {
            System.out.format("Symbolic link: %s ", file);
        } else if (attr.isRegularFile()) {
            System.out.format("Regular file: %s ", file);
        } else {
            System.out.format("Other: %s ", file);
        }
        System.out.println("(" + attr.size() + "bytes)");
        return CONTINUE;
    }

    // Print each directory visited.
    @Override
    <strong>public FileVisitResult postVisitDirectory(Path dir,
                                          IOException exc)</strong> {
        System.out.format("Directory: %s%n", dir);
        return CONTINUE;
    }

    // If there is some error accessing
    // the file, let the user know.
    // If you don't override this method
    // and an error occurs, an IOException 
    // is thrown.
    @Override
    <strong>public FileVisitResult visitFileFailed(Path file,
                                       IOException exc)</strong> {
        System.err.println(exc);
        return CONTINUE;
    }
}

```

## <a name="invoke" id="invoke">Kickstarting the Process</a>

Once you have implemented your `FileVisitor`, how do you initiate the file walk? There are two `walkFileTree` methods in the `Files` class.

<li>
[`walkFileTree(Path, FileVisitor)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#walkFileTree-java.nio.file.Path-java.nio.file.FileVisitor-)</li>
<li>
[`walkFileTree(Path, Set&lt;FileVisitOption&gt;, int, FileVisitor)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#walkFileTree-java.nio.file.Path-java.util.Set-int-java.nio.file.FileVisitor-)</li>

The first method requires only a starting point and an instance of your `FileVisitor`. You can invoke the `PrintFiles` file visitor as follows:

```

Path startingDir = ...;
PrintFiles pf = new PrintFiles();
Files.walkFileTree(startingDir, pf);

```

The second `walkFileTree` method enables you to additionally specify a limit on the number of levels visited and a set of 
[`FileVisitOption`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitOption.html) enums. If you want to ensure that this method walks the entire file tree, you can specify `Integer.MAX_VALUE` for the maximum depth argument.

You can specify the `FileVisitOption` enum, `FOLLOW_LINKS`, which indicates that symbolic links should be followed.

This code snippet shows how the four-argument method can be invoked:

```

import static java.nio.file.FileVisitResult.*;

Path startingDir = ...;

EnumSet&lt;FileVisitOption&gt; opts = EnumSet.of(FOLLOW_LINKS);

Finder finder = new Finder(pattern);
Files.walkFileTree(startingDir, opts, Integer.MAX_VALUE, finder);

```

## <a name="order" id="order">Considerations When Creating a FileVisitor</a>

A file tree is walked depth first, but you cannot make any assumptions about the iteration order that subdirectories are visited.

If your program will be changing the file system, you need to carefully consider how you implement your `FileVisitor`.

For example, if you are writing a recursive delete, you first delete the files in a directory before deleting the directory itself. In this case, you delete the directory in `postVisitDirectory`.

If you are writing a recursive copy, you create the new directory in `preVisitDirectory` before attempting to copy the files to it (in `visitFiles`). If you want to preserve the attributes of the source directory (similar to the UNIX `cp -p` command), you need to do that **after** the files have been copied, in `postVisitDirectory`. The 
[`<code>Copy`</code>](examples/Copy.java) example shows how to do this.

If you are writing a file search, you perform the comparison in the `visitFile` method. This method finds all the files that match your criteria, but it does not find the directories. If you want to find both files and directories, you must also perform the comparison in either the `preVisitDirectory` or `postVisitDirectory` method. The 
[`<code>Find`</code>](examples/Find.java) example shows how to do this.

You need to decide whether you want symbolic links to be followed. If you are deleting files, for example, following symbolic links might not be advisable. If you are copying a file tree, you might want to allow it. By default, `walkFileTree` does not follow symbolic links.

The `visitFile` method is invoked for files. If you have specified the `FOLLOW_LINKS` option and your file tree has a circular link to a parent directory, the looping directory is reported in the `visitFileFailed` method with the `FileSystemLoopException`. The following code snippet shows how to catch a circular link and is from the 
[`<code>Copy`</code>](examples/Copy.java) example:

```

@Override
public FileVisitResult
    visitFileFailed(Path file,
        IOException exc) {
    if (exc instanceof FileSystemLoopException) {
        System.err.println("cycle detected: " + file);
    } else {
        System.err.format("Unable to copy:" + " %s: %s%n", file, exc);
    }
    return CONTINUE;
}

```

This case can occur only when the program is following symbolic links.

## <a name="return" id="return">Controlling the Flow</a>

Perhaps you want to walk the file tree looking for a particular directory and, when found, you want the process to terminate. Perhaps you want to skip specific directories.

The `FileVisitor` methods return a 
[`FileVisitResult`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitResult.html) value. You can abort the file walking process or control whether a directory is visited by the values you return in the `FileVisitor` methods:

- `CONTINUE` &#8211; Indicates that the file walking should continue. If the `preVisitDirectory` method returns `CONTINUE`, the directory is visited.
- `TERMINATE` &#8211; Immediately aborts the file walking. No further file walking methods are invoked after this value is returned.
- `SKIP_SUBTREE` &#8211; When `preVisitDirectory` returns this value, the specified directory and its subdirectories are skipped. This branch is "pruned out" of the tree.
- `SKIP_SIBLINGS` &#8211; When `preVisitDirectory` returns this value, the specified directory is not visited, `postVisitDirectory` is not invoked, and no further unvisited siblings are visited. If returned from the `postVisitDirectory` method, no further siblings are visited. Essentially, nothing further happens in the specified directory.

In this code snippet, any directory named `SCCS` is skipped:

```

import static java.nio.file.FileVisitResult.*;

public FileVisitResult
     preVisitDirectory(Path dir,
         BasicFileAttributes attrs) {
    (if (dir.getFileName().toString().equals("SCCS")) {
         return SKIP_SUBTREE;
    }
    return CONTINUE;
}

```

In this code snippet, as soon as a particular file is located, the file name is printed to standard output, and the file walking terminates:

```

import static java.nio.file.FileVisitResult.*;

// The file we are looking for.
Path lookingFor = ...;

public FileVisitResult
    visitFile(Path file,
        BasicFileAttributes attr) {
    if (file.getFileName().equals(lookingFor)) {
        System.out.println("Located file: " + file);
        return TERMINATE;
    }
    return CONTINUE;
}

```

## <a name="ex" id="ex">Examples</a>

The following examples demonstrate the file walking mechanism:

<li>
[`<code>Find`</code>](examples/Find.java) &#8211; Recurses a file tree looking for files and directories that match a particular glob pattern. This example is discussed in 
[Finding Files](find.html).</li>
<li>
[`<code>Chmod`</code>](examples/Chmod.java) &#8211; Recursively changes permissions on a file tree (for POSIX systems only).</li>
<li>
[`<code>Copy`</code>](examples/Copy.java) &#8211; Recursively copies a file tree.</li>
<li>
[`<code>WatchDir`</code>](examples/WatchDir.java) &#8211; Demonstrates the mechanism that watches a directory for files that have been created, deleted or modified. Calling this program with the `-r` option watches an entire tree for changes. For more information about the file notification service, see 
[Watching a Directory for Changes](notification.html).</li>
