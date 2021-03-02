
# Sealing Packages in Extensions

You can optionally **seal** packages in extension JAR files as an additional security measure. If a package is sealed, all classes defined in that package must originate from a single JAR file.

Without sealing, a "hostile" program could create a class and define it to be a member of one of your extension packages. The hostile software would then have free access to package-protected members of your extension package.

Sealing packages in extensions is no different than sealing any JAR-packaged classes. To seal your extension packages, you must add the <tt>Sealed</tt> header to the manifest of the JAR file containing your extension. You can seal individual packages by associating a <tt>Sealed</tt> header with the packages' <tt>Name</tt> headers. A <tt>Sealed</tt> header not associated with an individual package in the archive signals that all packages are sealed. Such a "global" <tt>Sealed</tt> header is overridden by any <tt>Sealed</tt> headers associated with individual packages. The value associated with the <tt>Sealed</tt> header is either <tt>true</tt> or <tt>false</tt>.

## Examples

Let's look at a few sample manifest files. For these examples suppose that the JAR file contains these packages:

```

com/myCompany/package_1/
com/myCompany/package_2/
com/myCompany/package_3/
com/myCompany/package_4/

```

Suppose that you want to seal all the packages. You could do so by simply adding an archive-level <tt>Sealed</tt> header to the manifest like this:

```

Manifest-Version: 1.0
Sealed: true

```

All packages in any JAR file having this manifest will be sealed.

If you wanted to seal only <tt>com.myCompany.package_3</tt>, you could do so with this manifest:

```

Manifest-Version: 1.0

Name: com/myCompany/package_3/
Sealed: true

```

In this example the only <tt>Sealed</tt> header is that associated with the <tt>Name</tt> header of package <tt>com.myCompany.package_3</tt>, so only that package is sealed. (The <tt>Sealed</tt> header is associated with the <tt>Name</tt> header because there are no blank lines between them.)

For a final example, suppose that you wanted to seal all packages **except for** <tt>com.myCompany.package_2</tt>. You could accomplish that with a manifest like this:

```

Manifest-Version: 1.0
Sealed: true

Name: com/myCompany/package_2/
Sealed: false

```

In this example the archive-level <tt>Sealed:&#160;true</tt> header indicates that all of the packages in the JAR file are to be sealed. However, the manifest also has a <tt>Sealed:&#160;false</tt> header associated with package <tt>com.myCompany.package_2</tt>, and that header overrides the archive-level sealing for that package. Therefore this manifest will cause all packages to be sealed except for <tt>com.myCompany.package_2</tt>.
