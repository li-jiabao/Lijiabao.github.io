
# Import the Certificate as a Trusted Certificate

Suppose that you are Ruth and have received from Stan Smith

- The signed JAR file `sContract.jar` containing a contract
- The file `StanSmith.cer` containing the public key certificate for the public key corresponding to the private key used to sign the JAR file

Before you can use the `jarsigner` tool to check the authenticity of the JAR file's signature, you need to import Stan's certificate into your keystore.

Even though you (acting as Stan) created these files and they haven't actually been transported anywhere, you can simulate being someone other than the creater and sender, Stan. Acting as Ruth, type the following command to create a keystore named `exampleruthstore` and import the certificate into an entry with an alias of `stan`.

```

**keytool -import -alias stan -file StanSmith.cer -keystore exampleruthstore**

```

Since the keystore doesn't yet exist, `keytool` will create it for you. It will prompt you for a keystore password.

The `keytool` prints the certificate information and asks you to verify it; For example, by comparing the displayed certificate fingerprints with those obtained from another (trusted) source of information. (Each fingerprint is a relatively short number that uniquely and reliably identifies the certificate.) For example, in the real world you can phone Stan and ask him what the fingerprints should be. He can get the fingerprints of the `StanSmith.cer` file he created by executing the command

```

**keytool -printcert -file StanSmith.cer**

```

If the fingerprints he sees are the same as the ones reported to you by `keytool`, then you both can assume that the certificate has not been modified in transit. You can safely let `keytool` procede to place a "trusted certificate" entry into your keystore. This entry contains the public key certificate data from the file `StanSmith.cer`. `keytool` assigns the alias `stan` to this new entry.
