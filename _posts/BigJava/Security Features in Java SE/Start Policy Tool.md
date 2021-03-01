
# Specify the Keystore

For this lesson you will grant all code in JAR files signed by the alias susan read access to all files in the `C:\TestData\` directory. You need to

1. Specify the keystore containing the certificate information aliased by susan
1. Create the policy entry granting the permission

The keystore is the one named `exampleraystore` created in the [Import the Certificate as a Trusted Certificate](rstep2.html) step.

To specify the keystore, choose the **Change Keystore** command in the **Edit** menu of the main Policy Tool window. This brings up a dialog box in which you can specify the keystore URL and the keystore type.

To specify the keystore named `exampleraystore` in the `Test` directory on the `C:` drive, type the following `file` URL into the text box labeled New KeyStore URL

```

file:/C:/Test/exampleraystore

```

You can leave the text box labeled New KeyStore Type blank if the keystore type is the default one, as specified in the security properties file. Your keystore will be the default type, so leave the text box blank.

When you are done specifying the keystore URL, choose **OK**. The text box labeled Keystore is now filled in with the URL.

Next, you need to specify the new policy entry.
