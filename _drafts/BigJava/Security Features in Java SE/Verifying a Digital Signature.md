
# Prepare Initial Program Structure

Here's the basic structure of the `VerSig` program created in the following parts of this lesson. Place this program structure in a file called `VerSig.java`.

```

import java.io.*;
import java.security.*;
import java.security.spec.*;

class VerSig {

    public static void main(String[] args) {

        /* Verify a DSA signature */

        if (args.length != 3) {
            System.out.println("Usage: VerSig " +
                "publickeyfile signaturefile " + "datafile");
        }
        else try {

        // the rest of the code goes here

        } catch (Exception e) {
            System.err.println("Caught exception " + e.toString());
        }
    }

}

```

<br />
Notes:

<li>
The methods for verifying data are in the `java.security` package, so the program imports everything from that package. The program also imports the `java.io` package for methods needed to input the file data to be signed, as well as the `java.security.spec` package, which contains the `X509EncodedKeySpec` class.
</li>
<li>
Three arguments are expected, specifying the public key, the signature, and the data files.
</li>
<li>
The code written in subsequent steps of this lesson will go between the `try` and the `catch` blocks.
</li>
