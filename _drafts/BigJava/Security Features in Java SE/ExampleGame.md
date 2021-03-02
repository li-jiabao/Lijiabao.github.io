
# The HighScore Class

The `HighScore` class stores and protects access to the user's high score for `ExampleGame` (and any other games that call it). For simplicity, this class saves the high score value into a file, called `.highscore`, in the user's home directory. However, before allowing `ExampleGame` to retrieve or update the user's high score value, this class checks to make sure that the user has granted `ExampleGame` permission to access the high score in his/her security policy file.

## Checking that `ExampleGame` has the HighScorePermission

To check whether or not `ExampleGame` has permission to access the user's high score, the `HighScore` class must:

1. Call `System.getSecurityManager()` to get the currently installed security manager.
<li>If the result is not null (i.e., there *is* a security manager, as opposed to the caller being an application that is unrestricted), then
<ol>
1. Construct a `HighScorePermission` object, and
1. Call the security manager's `checkPermission` method, and pass it the newly constructed `HighScorePermission` object.

Here is the code:

```

SecurityManager sm = System.getSecurityManager();
if (sm != null) {
    sm.checkPermission(
        new HighScorePermission(gameName));
}

```

The `checkPermission` method essentially asks the security manager if `ExampleGame` has the specified `HighScorePermission`. In other words, it asks the security manager if `ExampleGame` has permission to update the user's high score value for the specified game (`ExampleGame`). The underlying security framework will consult the user's security policy to see if `ExampleGame` indeed has this permission.

## The HighScore Code


[`Here`](examples/com/scoredev/scores/HighScore.java) is the complete source code for the `HighScore` class.

Note: The `doPrivileged` method calls are used to enable `HighScore` to temporarily access resources that are available to it but that are not available to the code that calls it (`ExampleGame`). For example, it is expected that the policy file will grant `HighScore` permission to access the `.highscore` file in the user's home directory, but it will not grant this permission to games, such as `ExampleGame`.
