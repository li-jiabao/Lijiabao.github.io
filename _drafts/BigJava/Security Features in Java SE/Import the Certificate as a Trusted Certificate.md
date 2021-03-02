
# Verify the JAR File Signature

Acting as Ruth, you have now imported Stan's public key certificate into the `exampleruthstore` keystore as a "trusted certificate." You can now use the `jarsigner` tool to verify the authenticity of the JAR file signature.

When you verify a signed JAR file, you verify that the signature is valid and that the JAR file has not been tampered with. You can do this for the `sContract.jar` file via the following command:

```

**jarsigner -verify -verbose -keystore exampleruthstore sContract.jar **

```

You should see something like the following:

```

       183 Fri Jul 31 10:49:54 PDT 1998 META-INF/SIGNLEGAL.SF
       1542 Fri Jul 31 10:49:54 PDT 1998 META-INF/SIGNLEGAL.DSA
       0 Fri Jul 31 10:49:18 PDT 1998 META-INF/
smk    1147 Wed Jul 29 16:06:12 PDT 1998 contract

 s = signature was verified 
 m = entry is listed in manifest
 k = at least one certificate was found in keystore
 i = at least one certificate was found in identity scope

jar verified.

```

Be sure to run the command with the `-verbose` option to get enough information to ensure the following:

- the contract file is among the files in the JAR file that were signed and its signature was verified (that's what the `s` signifies)
- the public key used to verify the signature is in the specified keystore and thus trusted by you (that's what the `k` signifies).
