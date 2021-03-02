
# The HighScorePermission Class

The `HighScorePermission` class defines the permission that `ExampleGame` needs to update the user's high score.

All permission classes should subclass from either `java.security.Permission` or `java.security.BasicPermission`. The basic difference between the two is that `java.security.Permission` defines more complex permissions that require names and actions. For example, a `java.io.FilePermission` extends from `java.security.Permission`, and requires a name (a filename), and actions allowed for that file (read/write/delete).

In contrast, `java.security.BasicPermission` defines simpler permissions that only require a name. For example, `java.lang.RuntimePermission` extends from `java.security.BasicPermission` and simply needs a name (like "exitVM"), which allows programs to exit the Java Virtual Machine.

Our `HighScorePermission` is a simple permission, and hence can be extended from `java.security.BasicPermission`.

Often, the method implementations in the `BasicPermission` class itself do not need to be overridden by its subclasses. That is the case with our `HighScorePermission`, so all we need to implement are the constructors, which just invoke the superclass constructors, as shown in the 
[`following`](examples/com/scoredev/scores/HighScorePermission.java):

```


package com.scoredev.scores;

import java.security.*;

public final class HighScorePermission extends BasicPermission {

    public HighScorePermission(String name)
    {
	super(name);
    }

    // note that actions is ignored and not used,
    // but this constructor is still needed
    public HighScorePermission(String name, String actions) 
    {
	super(name, actions);
    }
}

```
