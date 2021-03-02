
# Add a Policy Entry with a SignedBy Alias

To grant code signed by `susan` permission to read any files in the `C:\TestData` directory, you need to create a policy entry granting this permission. Note that "Code signed by `susan`" is an abbreviated way of saying "Code in a class file contained in a JAR file, where the JAR file was signed using the private key corresponding to the public key that appears in a keystore certificate in an entry aliased by `susan`."

Choose the **Add Policy Entry** button in the main Policy Tool window. This brings up the Policy Entry dialog box:

Using this dialog box, type the following alias into the **SignedBy** text box:

```

**susan**

```

Leave the **CodeBase** text box blank, to grant *all* code signed by `susan` the permission, no matter where it comes from.

```

file:/C:/Test/*

```

To add the permission, choose the **Add Permission** button. This brings up the Permissions dialog box.

Do the following.

1. Choose **File Permission** from the Permission drop-down list. The complete permission type name (`java.io.FilePermission`) now appears in the text box to the right of the drop-down list.
<li>Type the following in the text box to the right of the list labeled Target Name to specify all files in the `C:\TestData\` directory:
<pre><code>
**C:\TestData\***
</code></pre>
</li>
1. Specify read access by choosing the **read** option from the Actions drop-down list.

Now the Permissions dialog box looks like the following.

Choose the **OK** button. The new permission appears in a line in the Policy Entry dialog, as follows.

You are now done specifying this policy entry, so choose the **Done** button in the Policy Entry dialog. The Policy Tool window now contains a line representing the policy entry, showing the **SignedBy** value.
