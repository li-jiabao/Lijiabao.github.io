
# Input the Signature Bytes

Next, input the signature bytes from the file specified as the second command line argument.

```

FileInputStream sigfis = new FileInputStream(args[1]);
byte[] sigToVerify = new byte[sigfis.available()]; 
sigfis.read(sigToVerify);
sigfis.close();

```

Now the byte array `sigToVerify` contains the signature bytes.
