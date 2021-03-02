
# Setting Privileges for Extensions

If a 

[The Security Manager](../../essential/environment/security.html)
 is in force, the following conditions must be met to enable any software, including extension software, to perform security-sensitive operations:

- The security-sensitive code in the extension must be wrapped in a `PrivilegedAction` object.
- The security policy implemented by the security manager must grant the appropriate permission to the extension. By default, installed extensions are granted all security permissions as if they were part of the core platform API. The permissions granted by the security policy apply only to code wrapped in the `PrivilegedAction` instance.

Let's look at each of these conditions in a little more detail, with some examples.

## Using the PrivilegedAction Class

Suppose that you want to modify the `RectangleArea` class in the extension example of the previous lesson to write rectangle areas to a file rather than to stdout. Writing to a file, however, is a security-sensitive operation, so if your software is going to be running under a security manager, you'll need to mark your code as being privileged. There are two steps you need to take to do so:

1. You need to place code that performs security-sensitive operations within the `run` method of an object of type `java.security.PrivilegedAction`.
1. You must use that `PrivilegedAction` object as the argument in a call to the `doPrivileged` method of `java.security.AccessController`.

If we apply those guidelines to the `RectangleArea` class, our class definition would look something like this:

```

import java.io.*;
import java.security.*;

public final class RectangleArea {
    public static void
    writeArea(final java.awt.Rectangle r) {
        AccessController.
          doPrivileged(new PrivilegedAction() {
            public Object run() {
                try { 
                    int area = r.width * r.height;
                    String userHome = System.getProperty("user.home");
                    FileWriter fw = new FileWriter( userHome + File.separator
                        + "test" + File.separator + "area.txt");
                    fw.write("The rectangle's area is " + area);
                    fw.flush();
                    fw.close();
                } catch(IOException ioe) {
                    System.err.println(ioe);
                }
                return null;
            }
        });
    }
}

```

The single method in this class, `writeArea`, computes the area of a rectangle, and writes the area to a file called `area.txt` in the `test` directory under the user's home directory.

The security-sensitive statements dealing with the output file are placed within the `run` method of a new instance of `PrivilegedAction`. (Note that `run` requires that an `Object` instance be returned. The returned object can be `null`.) The new `PrivilegedAction` instance is then passed as an argument in a call to `AccessController.doPrivileged`.

For more information about using `doPrivileged`, see 
[API for Privileged Blocks](https://docs.oracle.com/javase/8/docs/technotes/guides/security/doprivileged.html) in the JDK&#8482; documentation.

Wrapping security-sensitive code in a `PrivilegedAction` object in this manner is the first requirement for enabling an extension to perform security-sensitive operations. The second requirement is: getting the security manager to grant the privileged code the appropriate permissions.

## Specifying Permissions with the Security Policy

The security policy in force at runtime is specified by a **policy file**. The default policy is set by the file `lib/security/java.policy` in the JRE software.

The policy file assigns security privileges to software by using **grant** entries. The policy file can contain any number of grant entries. The default policy file has this grant entry for installed extensions:

```

grant codeBase "file:${{java.ext.dirs}}/*" {
    permission java.security.AllPermission;
};

```

This entry specifies that files in the directories specified by `file:${{java.ext.dirs}}/*` are to be granted the permission called `java.security.AllPermission`. (Note that as of Java 6, `java.ext.dirs` refers to a <tt>classpath</tt>-like path of directories, each of which can hold installed extensions.) It's not too hard to guess that `java.security.AllPermission` grants installed extensions all the security privileges that it's possible to grant.

By default, then, installed extensions have no security restrictions. Extension software can perform security-sensitive operations as if there were no security manager installed, provided that security-sensitive code is contained in an instance of `PrivilegedAction` passed as an argument in a `doPrivileged` call.

To limit the privileges granted to extensions, you need to modify the policy file. To deny all privileges to all extensions, you could simply remove the above grant entry.

Not all permissions are as comprehensive as the `java.security.AllPermission` granted by default. After deleting the default grant entry, you can enter a new grant entry for particular permissions, including:

- `java.awt.**AWTPermission**`
- `java.io.**FilePermission**`
- `java.net.**NetPermission**`
- `java.util.**PropertyPermission**`
- `java.lang.reflect.**ReflectPermission**`
- `java.lang.**RuntimePermission**`
- `java.security.**SecurityPermission**`
- `java.io.**SerializablePermission**`
- `java.net.**SocketPermission**`

The 
[Permissions in the JDK](https://docs.oracle.com/javase/8/docs/technotes/guides/security/permissions.html) documentation provides details about each of these permissions. Let's look at those needed to use RectangleArea as an extension.

The `RectangleArea.writeArea` method needs two permissions: one to determine the path to the user's home directory, and the other to write to a file. Assuming that the `RectangleArea` class is bundled in the file `area.jar`, you could grant write privileges by adding this entry to the policy file:

```

grant codeBase "file:${java.home}/lib/ext/area.jar" {
    permission java.io.PropertyPermission "user.home",
        "read";
    permission java.io.FilePermission
        "${user.home}${/}test${/}*", "write";
};

```

The `codeBase&#160;"file:${java.home}/lib/ext/area.jar"` part of this entry guarantees that any permissions specified by this entry will apply only to the `area.jar`. The `java.io.PropertyPermission` permits access to properties. The first argument, `"user.home"`, names the property, and the second argument, `"read"`, indicates that the property can be read. (The other choice is `"write"`.)

The `java.io.FilePermission` permits access to files. The first argument, `"${user.home}${/}test${/}*"`, indicates that `area.jar` is being granted permission to access all files in the `test` directory that is in the user's home directory. (Note that `${/}` is a platform-independent file separator.) The second argument indicates that the file access being granted is only for writing. (Other choices for the second argument are `"read"`, `"delete"`, and `"execute"`.)

## Signing Extensions

You can use the policy file to place additional restrictions on the permissions granted to extensions by requiring them to be signed by a trusted entity. (For a review of signing and verifying JAR files, see the 

[Signing JAR Files](../../deployment/jar/signing.html) lesson in this tutorial.)

To allow signature verification of extensions or other software in conjunction with granting permissions, the policy file must contain a **keystore entry**. The keystore entry specifies which keystore is to be used in the verification. Keystore entries have the form

```

keystore "*keystore_url*";

```

The URL **keystore_url** is either an absolute or relative. If it's relative, the URL is relative to the location of the policy file. For example, to use the default keystore used by <tt>keytool</tt>, add this entry to <tt>java.policy</tt>

```

keystore "file://${user.home}/.keystore";

```

To indicate that an extension must be signed in order to be granted security privileges, you use the `signedBy` field. For example, the following entry indicates that the extension `area.jar` is to be granted the listed privileges only if it is signed by the users identified in the keystore by the aliases Robert and Rita:

```

grant signedBy "Robert,Rita",
    codeBase "file:${java.home}/lib/ext/area.jar" {
        permission java.io.PropertyPermission
            "user.home", "read";
        permission java.io.FilePermission
            "${user.home}${/}test${/}*", "write";
};

```

If the `codeBase` field is omitted, as in the following "grant", the permissions are granted to **any** software, including installed or download extensions, that are signed by "Robert" or "Rita":

```

grant signedBy "Robert,Rita" {
    permission java.io.FilePermission "*", "write";  
};

```

For further details about the policy file format, see section 3.3.1 of the 
[Security Architecture Specification](https://docs.oracle.com/javase/8/docs/technotes/guides/security/spec/security-spec.doc3.html#20131) in the JDK documentation.
