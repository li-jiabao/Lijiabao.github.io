
# Sign the Data

Now that you have created a public key and a private key, you are ready to sign the data. In this example you will sign the data contained in a file. `GenSig` gets the file name from the command line. A digital signature is created (or verified) using an instance of the `Signature` class.

Signing data, generating a digital signature for that data, is done with the following steps.

**Get a Signature Object**: The following gets a `Signature` object for generating or verifying signatures using the DSA algorithm, the same algorithm for which the program generated keys in the previous step, [Generate Public and Private Keys](step2.html).

```

Signature dsa = Signature.getInstance("SHA1withDSA", "SUN"); 

```

Note: When specifying the signature algorithm name, you should also include the name of the message digest algorithm used by the signature algorithm. SHA1withDSA is a way of specifying the DSA signature algorithm, using the SHA-1 message digest algorithm.

Initialize the Signature Object

Before a `Signature` object can be used for signing or verifying, it must be initialized. The initialization method for signing requires a private key. Use the private key placed into the `PrivateKey` object named `priv` in the previous step.

```

dsa.initSign(priv);

```

**Supply the Signature Object the Data to Be Signed** This program will use the data from the file whose name is specified as the first (and only) command line argument. The program will read in the data a buffer at a time and will supply it to the `Signature` object by calling the `update` method.

```

FileInputStream fis = new FileInputStream(args[0]);
BufferedInputStream bufin = new BufferedInputStream(fis);
byte[] buffer = new byte[1024];
int len;
while ((len = bufin.read(buffer)) &gt;= 0) {
    dsa.update(buffer, 0, len);
};
bufin.close();

```

Generate the Signature

Once all of the data has been supplied to the `Signature` object, you can generate the digital signature of that data.

```

byte[] realSig = dsa.sign();

```
