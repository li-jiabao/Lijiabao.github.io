
# Export the Public Key Certificate

You now have a signed JAR file `sContract.jar`. Recipients wanting to use this file will also want to authenticate your signature. To do this, they need the public key that corresponds to the private key you used to generate your signature. You supply your public key by sending them a copy of the certificate that contains your public key. Copy that certificate from the keystore `examplestanstore` to a file named `StanSmith.cer` via the following:

```

keytool -export -keystore examplestanstore
-alias signLegal -file StanSmith.cer

```

You will be prompted for the store password.

Once they have that certificate and the signed JAR file, your recipient can use the `jarsigner` tool to authenticate your signature. See [Steps for the Contract Receiver](receiver.html).
