
# Setting Package Version Information

You may need to include package version information in a JAR file's manifest. You provide this information with the following headers in the manifest:
<th id="h1">Header</th><th id="h2">Definition</th>
<td headers="h1"><tt>Name</tt></td><td headers="h2">The name of the specification.</td>
<td headers="h1"><tt>Specification-Title</tt></td><td headers="h2">The title of the specification.</td>
<td headers="h1"><tt>Specification-Version</tt></td><td headers="h2">The version of the specification.</td>
<td headers="h1"><tt>Specification-Vendor</tt></td><td headers="h2">The vendor of the specification.</td>
<td headers="h1"><tt>Implementation-Title</tt></td><td headers="h2">The title of the implementation.</td>
<td headers="h1"><tt>Implementation-Version</tt></td><td headers="h2">The build number of the implementation.</td>
<td headers="h1"><tt>Implementation-Vendor</tt></td><td headers="h2">The vendor of the implementation.</td>

One set of such headers can be assigned to each package. The versioning headers should appear directly beneath the <tt>Name</tt> header for the package. This example shows all the versioning headers:

```

Name: java/util/
Specification-Title: Java Utility Classes
Specification-Version: 1.2
Specification-Vendor: Example Tech, Inc.
Implementation-Title: java.util
Implementation-Version: build57
Implementation-Vendor: Example Tech, Inc.

```

For more information about package version headers, see the 
[Package Versioning specification ](https://docs.oracle.com/javase/8/docs/technotes/guides/versioning/spec/versioning2.html#wp89936).

## An Example

We want to include the headers in the example above in the manifest of MyJar.jar.

We first create a text file named <tt>Manifest.txt</tt> with the following contents:

```

Name: java/util/
Specification-Title: Java Utility Classes
Specification-Version: 1.2
Specification-Vendor: Example Tech, Inc.
Implementation-Title: java.util 
Implementation-Version: build57
Implementation-Vendor: Example Tech, Inc.

```

We then create a JAR file named <tt>MyJar.jar</tt> by entering the following command:

```

jar cfm MyJar.jar Manifest.txt MyPackage/*.class

```

This creates the JAR file with a manifest with the following contents:

```

Manifest-Version: 1.0
Created-By: 1.7.0_06 (Oracle Corporation)
Name: java/util/
Specification-Title: Java Utility Classes
Specification-Version: 1.2
Specification-Vendor: Example Tech, Inc.
Implementation-Title: java.util 
Implementation-Version: build57
Implementation-Vendor: Example Tech, Inc.

```
