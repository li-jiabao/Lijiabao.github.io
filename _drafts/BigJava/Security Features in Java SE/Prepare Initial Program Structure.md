
# Input and Convert the Encoded Public Key Bytes

Next, `VerSig` needs to import the encoded public key bytes from the file specified as the first command line argument and to convert them to a `PublicKey`. A `PublicKey` is needed because that is what the `Signature` `initVerify` method requires in order to initialize the `Signature` object for verification.

First, read in the encoded public key bytes.

```

FileInputStream keyfis = new FileInputStream(args[0]);
byte[] encKey = new byte[keyfis.available()];  
keyfis.read(encKey);

keyfis.close();

```

Now the byte array `encKey` contains the encoded public key bytes.

You can use a `KeyFactory` class in order to instantiate a DSA public key from its encoding. The `KeyFactory` class provides conversions between opaque keys (of type `Key`) and key specifications, which are transparent representations of the underlying key material. With an opaque key you can obtain the algorithm name, format name, and encoded key bytes, but not the key material, which, for example, may consist of the key itself and the algorithm parameters used to calculate the key. (Note that `PublicKey`, because it extends `Key`, is itself a `Key`.)

So, first you need a key specification. You can obtain one via the following, assuming that the key was encoded according to the X.509 standard, which is the case, for example, if the key was generated with the built-in DSA key-pair generator supplied by the SUN provider:

```

X509EncodedKeySpec pubKeySpec = new X509EncodedKeySpec(encKey);

```

Now you need a `KeyFactory` object to do the conversion. That object must be one that works with DSA keys.

```

KeyFactory keyFactory = KeyFactory.getInstance("DSA", "SUN");

```

Finally, you can use the `KeyFactory` object to generate a `PublicKey` from the key specification.

```

PublicKey pubKey =
    keyFactory.generatePublic(pubKeySpec);

```
