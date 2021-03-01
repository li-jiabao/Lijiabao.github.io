
# Compile and Run the Program


[`Here`](examples/VerSig.java) is the complete source code for the `VerSig.java` program, with some comments added.

Compile and run the program. Remember, you need to specify three arguments on the command line:

- The name of the file containing the encoded public key bytes
- The name of the file containing the signature bytes
- The name of the data file (the one for which the signature was generated)

Since you will be testing the output of the `GenSig` program, the file names you should use are

- `suepk`
- `sig`
- `data`

Here's a sample run; the bold indicates what you type.

```

**%java VerSig suepk sig data**
signature verifies: true

```
