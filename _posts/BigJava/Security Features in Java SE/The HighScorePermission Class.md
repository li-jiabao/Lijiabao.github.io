
# A Sample Policy File

Below is a complete policy file for a user wanting to run `ExampleGame`.

The policy file syntax is not described here; if you are interested, see the 
[Default Policy Implementation and Policy File Syntax](https://docs.oracle.com/javase/8/docs/technotes/guides/security/PolicyFiles.html) page.

You don't need to know the syntax; you can always use the Policy Tool to create policy files, as shown in 
[Creating a Policy File](../tour1/index.html), 
[Quick Tour of Controlling Applications](../tour2/index.html), and 
[Signing Code and Granting It Permissions](../toolsign/index.html) lessons.

Below is the sample policy file, followed by a description of the individual entries. Assume that

- The policy file is on Kim's computer, and Kim's keystore is named `kim.keystore`.
- `ExampleGame` has been signed by the game creator Terry's private key, and the corresponding public key is in the keystore entry aliased by `"terry"`.
- The `HighScore` and `HighScorePermissions` classes were signed by the private key of the person who implemented them (Chris), and the corresponding public key is in the keystore entry aliased by `"chris"`.

Here is the policy file: 
[`kim.policy`](examples/kim.policy)

```

keystore "kim.keystore";

// Here is the permission ExampleGame needs.
// It grants code signed by "terry" the
// HighScorePermission, if the
// HighScorePermission was signed by "chris"
grant SignedBy "terry" {
  permission
    com.scoredev.scores.HighScorePermission
      "ExampleGame", signedBy "chris";
};

// Here is the set of permissions the HighScore
// class needs:
grant SignedBy "chris" {
  // The HighScore class needs permission to read
  // "user.home" to find the location of the
  // highscore file

  permission java.util.PropertyPermission
    "user.home", "read";

  // It needs permission to read and write the
  // high score file itself

  permission java.io.FilePermission
      "${user.home}${/}.highscore", "read,write";

  // It needs to get granted its own permission,
  // so it can call checkPermission
  // to see if its caller has permission.
  // Only grant it the permission
  // if the permission itself was signed by
  // "chris"

  permission
    com.scoredev.scores.HighScorePermission 
      "*", signedBy "chris";
};



```

## The Keystore Entry

A *keystore* is a repository of keys and certificates, and is used to look up the public keys of the signers specified in the policy file (`"terry"` and `"chris"` in this example).

The `keytool` utility is used to create and administer keystores.

For this lesson, assume Kim would like to play `ExampleGame`. If Kim's keystore is named `kim.keystore`, then Kim's policy file needs the following line at the very beginning:

```

keystore "kim.keystore";

```

## The ExampleGame Entry

A policy file entry specifies one or more permissions for code from a particular *code source* - either code from a particular location (URL), or code signed by a particular entity, or both.

Our policy file needs an entry for each game, granting code signed by a key from that game's creator a `HighScorePermission` whose name is the game name. That permission allows the game to call the `HighScore` methods to get or update the user's high score value for that particular game.

The entry required for `ExampleGame` is:

```

grant SignedBy "terry" {
    permission
        com.scoredev.scores.HighScorePermission 
            "ExampleGame", signedBy "chris";
};

```

Requiring that `ExampleGame` be signed by `"terry"` enables Kim to know that the game is the actual game that Terry developed. For this to work, Kim must have already stored Terry's public key certificate into `kim.keystore` using the alias `"terry"`.

Notice that the `HighScorePermission` needs to be signed by `"chris"`, the person who actually implemented that permission. This ensures that `ExampleGame` is granted the actual permission implemented by `"chris"`, and not someone else. As before, for this to work Kim must have already stored Chris's public key certificate into `kim.keystore` using the alias `"chris"`.

## The HighScore Entry

The final entry in the policy file grants permissions to the `HighScore` class. More specifically, it grants permissions to code signed by `"chris"`, who created and signed the class. Requiring the class to be signed by `"chris"` ensures that when `ExampleGame` calls upon this class to update the user's high score, `ExampleGame` knows for sure that it is using the original class implemented by `"chris"`.

To update the user's high score value for any games that call upon it do so, the `HighScore` class requires three permissions:

### 1. Permission to read the `"user.home"` property value.

The `HighScore` class stores the user's high score values in a `.highscore` file in the user's home directory. Therefore this class needs a `java.util.PropertyPermission` that allows it to read the `"user.home"` property value to find out exactly where the user's home directory resides:

```

permission java.util.PropertyPermission 
    "user.home", "read";

```

### 2. Permission to read and write to the high score file itself.

This permission is needed so the `HighScore` `getHighScore` and `setHighScore` methods can access the user's `.highscore` file to get or set, respectively, the current high score for the current game.

Here is the required permission:

```

permission java.io.FilePermission
    "${user.home}${/}.highscore", "read,write";

```

Note: The notation `${propName}` specifies the value of a property. Thus, `${user.home}` will be replaced by the value of the `"user.home"` property. The notation `${/}` is a platform-independent way of specifying a file separator.

### 3. All HighScorePermissions (i.e, HighScorePermissions of any name).

This permission is needed so that the `HighScore` checks to ensure the calling game has been granted a `HighScorePermission` whose name is the game name will work. That is, the `HighScore` class must *also* be granted the permission, since a permission check requires that all code currently on the stack have the specified permission.

Here is the required permission:

```

permission com.scoredev.scores.HighScorePermission
    "*", signedBy "chris";

```

As before, the `HighScorePermission` itself needs to be signed by `"chris"`, the person who actually implemented the permission.
