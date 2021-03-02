
# ExampleGame

Below is the source code for `ExampleGame`. For simplicity, `ExampleGame` does not actually contain code to play a game. It simply retrieves or updates a user's high score.

To see what the user's current high score value is, you could run:

```

java ExampleGame get

```

To set a new high score value for the user, you could run:

```

java ExampleGame set *score* 

```

To retrieve the user's current high score, `ExampleGame` simply instantiates a `HighScore` object and makes a call to its `getHighScore` method. To set a new high score for the user, `ExampleGame` instantiates a `HighScore` object and calls `setHighScore`, passing it the user's new high score.

Here is the source code for `ExampleGame`, 
[`ExampleGame.java`](examples/com/gamedev/games/ExampleGame.java):

```


package com.gamedev.games;

import java.io.*;
import java.security.*;
import java.util.Hashtable;
import com.scoredev.scores.*;

public class ExampleGame
{
    public static void main(String args[])
	throws Exception 
    {
	HighScore hs = new HighScore("ExampleGame");

	if (args.length == 0)
	    usage();

	if (args[0].equals("set")) {
	    hs.setHighScore(Integer.parseInt(args[1]));
	} else if (args[0].equals("get")) {
	    System.out.println("score = "+ hs.getHighScore());
	} else {
	    usage();
	}
    }

    public static void usage()
    {
	System.out.println("ExampleGame get");
	System.out.println("ExampleGame set &lt;score&gt;");
	System.exit(1);
    }
}

```
