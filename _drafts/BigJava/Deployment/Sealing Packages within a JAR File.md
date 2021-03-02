
# Sealing Packages within a JAR File

Packages within JAR files can be optionally sealed, which means that all classes defined in that package must be archived in the same JAR file. You might want to seal a package, for example, to ensure version consistency among the classes in your software.

You seal a package in a JAR file by adding the <tt>Sealed</tt> header in the manifest, which has the general form:

```

Name: myCompany/myPackage/
Sealed: true

```

The value <tt>myCompany/myPackage/</tt> is the name of the package to seal.

Note that the package name must end with a "/".

## An Example

We want to seal two packages <tt>firstPackage</tt> and <tt>secondPackage</tt> in the JAR file <tt>MyJar.jar</tt>.

We first create a text file named <tt>Manifest.txt</tt> with the following contents:

```

Name: myCompany/firstPackage/
Sealed: true

Name: myCompany/secondPackage/
Sealed: true

```

We then create a JAR file named <tt>MyJar.jar</tt> by entering the following command:

```

jar cfm MyJar.jar Manifest.txt MyPackage/*.class

```

This creates the JAR file with a manifest with the following contents:

```

Manifest-Version: 1.0
Created-By: 1.7.0_06 (Oracle Corporation)
Name: myCompany/firstPackage/
Sealed: true
Name: myCompany/secondPackage/
Sealed: true

```

## Sealing JAR Files

If you want to guarantee that all classes in a package come from the same code source, use JAR sealing. A sealed JAR specifies that all packages defined by that JAR are sealed unless overridden on a per-package basis.

To seal a JAR file, use the <tt>Sealed</tt> manifest header with the value true. For example,

```

Sealed: true

```

specifies that all packages in this archive are sealed unless explicitly overridden for particular packages with the <tt>Sealed</tt> attribute in a manifest entry.
