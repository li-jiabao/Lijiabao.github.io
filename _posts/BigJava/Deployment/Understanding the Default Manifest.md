
# Understanding the Default Manifest

When you create a JAR file, it automatically receives a default manifest file. There can be only one manifest file in an archive, and it always has the pathname

```

META-INF/MANIFEST.MF

```

When you create a JAR file, the default manifest file simply contains the following:

```

Manifest-Version: 1.0
Created-By: 1.7.0_06 (Oracle Corporation)

```

These lines show that a manifest's entries take the form of "header: value" pairs. The name of a header is separated from its value by a colon. The default manifest conforms to version 1.0 of the manifest specification and was created by the 1.7.0_06 version of the JDK.

The manifest can also contain information about the other files that are packaged in the archive. Exactly what file information should be recorded in the manifest depends on how you intend to use the JAR file. The default manifest makes no assumptions about what information it should record about other files.

Digest information is not included in the default manifest. To learn more about digests and signing, see the [Signing and Verifying JAR Files](signindex.html) lesson.
