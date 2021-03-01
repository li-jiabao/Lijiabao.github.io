
# See How to Restrict Applications

As you saw in the [previous step](step1.html), the Java runtime does *not* automatically install a Security Manager when it runs an *application*. To apply the same security policy to an application found on the local file system as to downloaded sandbox applets, you can invoke the interpreter with the new `-Djava.security.manager` command line argument.

To execute the `GetProps` application with the default security manager, type the following:

```

**java -Djava.security.manager GetProps**

```

Here's the output from the program:

```

C:\TEST&gt;java -Djava.security.manager GetProps
    About to get os.name property value
      The name of your operating system is: SunOS
    About to get java.version property value
      The version of the JVM you are running is: 1.7.0
    About to get user.home property value
    Caught exception java.security.AccessControlException:
        access denied ("java.util.PropertyPermission"
        "user.home" "read")

```

The process is shown in the following figure.

<br />

## Security-Sensitive Properties

The Java runtime loads a default policy file by default and grants all code permission to access some commonly useful properties such as `"os.name"` and `"java.version"`. These properties are not security-sensitive, so granting these permissions does not normally pose a security risk.

The other properties `GetProps` tries to access, `"user.home"` and `"java.home"`, are *not* among the properties for which the system policy file grants read permission. Thus as soon as `GetProps` attempts to access the first of these properties (`"user.home"`), the security manager prevents the access and reports an `AccessControlException`. This exception indicates that the policy currently in effect, which consists of entries in one or more policy files, doesn't allow permission to read the `"user.home"` property.

## The Default Policy File

The default policy file,
[`java.policy`](examples/java.policy) is (by default) located at:

- **Windows**: `*java.home*\lib\security\java.policy` 
- **UNIX**: `*java.home*/lib/security/java.policy`

Note that *java.home* represents the value of the `"java.home"` property, which is a system property specifying the directory into which the JRE was installed. Thus if the JRE was installed in the directory named `C:\jdk\jre` on Windows and `/jdk/jre` on UNIX, the system policy file is located at:

- **Windows**: `C:\jdk\jre\lib\security\java.policy`
- **UNIX**: `/jdk/jre/lib/security/java.policy`
