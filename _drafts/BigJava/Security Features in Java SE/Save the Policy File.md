
# See the Policy File Effects

In the previous steps you created an entry in the `exampleraypolicy` policy file granting code signed by `susan` permission to read files from the `C:\TestData\` directory (or the `testdata` directory in your home directory if you're working on UNIX). Now you should be able to successfully execute the `Count` program to read and to count the characters in a file from the specified directory, even when you run the application with a security manager.

As described at the end of the 
[Creating a Policy File](../tour1/index.html) lesson, there are two possible ways you can have the `exampleraypolicy` file be considered as part of the overall policy, in addition to the policy files specified in the security properties file. The first approach is to specify the additional policy file in a property passed to the runtime system. The second approach is to add a line in the security properties file specifying the additional policy file.

## <a name="Approach1" id="Approach1">Approach 1</a>

You can use a `-Djava.security.policy` command-line argument to specify a policy file that should be used in addition to or instead of the ones specified in the security properties file.

To run the `Count` application and have the `exampleraypolicy` policy file included, type the following while in the directory containing the `sCount.jar` and `exampleraypolicy` files:

```

<b>java -Djava.security.manager
    -Djava.security.policy=exampleraypolicy
    -cp sCount.jar Count C:\TestData\data</b>

```

Note: Type the command on a single line, with a space before `-D` and `-cp`.

The program should report the number of characters in the specified file.

If it still reports an error, something is wrong in the policy file. Use the Policy Tool to check the permission you just created in the [previous step](rstep3.html), and change any typos or other errors.

## <a name="Approach2" id="Approach2">Approach 2</a>

You can specify a number of URLs -- including ones of the form "http://" -- in `policy.url.n` properties in the security properties file, and all the designated policy files will get loaded.

So one way to have your `exampleraypolicy` file's policy entries considered by the interpreter is to add an entry indicating that file in the security properties file.

The security properties file is located at

- **Windows**: `*java.home*\lib\security\java.security` 
- **UNIX**: `*java.home*/lib/security/java.security`

The *`java.home`* portion indicates the directory into which the JRE was installed.

To modify the security properties file, open it in an editor suitable for editing an ASCII text file. Then add the following line after the line starting with `policy.url.2`:

- **Windows**: `**policy.url.3=file:/C:/Test/exampleraypolicy**`
- **UNIX**: `  **policy.url.3=file:${user.home}/test/exampleraypolicy**`

On a UNIX system you can alternatively explicitly specify your home directory, as in

```

**policy.url.3=file:/home/susanj/test/exampleraypolicy**

```

Next, in your command window, go to the directory containing the `sCount.jar` file, that is, the `C:\Test` or `~/test` directory. Type the following command on one line:

```

<b>java -Djava.security.manager
        -cp sCount.jar Count C:\TestData\data</b>

```

As with approach 1, if the program still reports an error, something is wrong with the policy file. Use the Policy Tool to check the permission you just created in the [previous step](rstep3.html), and change any typos or other errors.
