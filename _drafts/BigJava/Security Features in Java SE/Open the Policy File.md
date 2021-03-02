
# Grant the Required Permissions

To grant the `GetProps` application permission to read the `"user.home"` and `"java.home"` property values, you must create a policy entry granting those permissions. Choose the **Add Policy Entry** button in the main Policy Tool window. This brings up the Policy Entry dialog box, as shown in the following figure.

Type the following file URL into the **CodeBase** text box to indicate that you are going to be granting a permission to code from the specified directory, which is the directory in which `GetProps.class` is stored.

```

**file:/C:/Test/**

```

(Note, this is a URL and thus must always have slashes, not backslashes.)

Leave the **SignedBy** text box blank, since you aren't requiring the code to be signed.

To add permission to read the `"user.home"` property value, choose the **Add Permission** button. This brings up the Permissions dialog box.

Do the following.

1. Choose **Property Permission** from the Permission drop-down list. The complete permission type name (`java.util.PropertyPermission`) now appears in the text box to the right of the drop-down list.
<li>Type the following in the text box to the right of the list labeled Target Name to specify the `"user.home"` property:
<pre><code>
**user.home**
</code></pre>
</li>
1. Specify permission to read this property by selecting the **read** option from the Actions drop-down list.

Now the Permissions dialog box looks like the following.

Choose the **OK** button. The new permission appears in a line in the policy entry window.

To add permission to read the `"java.home"` property value, choose the **Add Permission** button again. In the Permissions dialog box, do the following:

1. Choose **Property Permission** from the Permission drop-down list.
<li>Type the following in the text box to the right of the list labeled Target Name to specify the `"java.home"` property:
<pre><code>
**java.home**
</code></pre>
</li>
1. Specify permission to read this property by choosing the **read** option from the Actions drop-down list.

Now the Permissions dialog box looks like the following.

Choose the **OK** button. The new permission and the previously added permission appear in lines in the policy entry window, as shown in the following figure.

You are now done specifying this policy entry, so choose the **Done** button in the Policy Entry dialog. The Policy Tool window now includes a line representing the new policy entry, showing the **CodeBase** value.
