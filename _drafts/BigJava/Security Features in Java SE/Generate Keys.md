
# Sign the JAR File

Now you are ready to sign the JAR file.

Type the following on one line in your command window to sign the JAR file `Contract.jar`, using the private key in the keystore entry aliased by `signLegal`, and to name the resulting signed JAR file `sContract.jar`:

```

jarsigner -keystore examplestanstore
    -signedjar sContract.jar
    Contract.jar signLegal

```

You will be prompted for the store password and the private key password.

The `jarsigner` tool extracts the certificate from the keystore entry whose alias is `signLegal` and attaches it to the generated signature of the signed JAR file.
