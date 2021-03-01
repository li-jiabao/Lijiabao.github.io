
# Prepare Initial Program Structure

Here's the basic structure of the `GenSig` program. Place it in a file called `GenSig.java`.

```

import java.io.*;
import java.security.*;

class GenSig {

    public static void main(String[] args) {

        /* Generate a DSA signature */

        if (args.length != 1) {
            System.out.println("Usage: GenSig nameOfFileToSign");
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
The methods for signing data are in the `java.security` package, so the program imports everything from that package. The program also imports the `java.io` package, which contains the methods needed to input the file data to be signed.
</li>
<li>
A single argument is expected, specifying the data file to be signed.
</li>
<li>
The code written in subsequent steps will go between the `try` and the `catch` blocks.
</li>
