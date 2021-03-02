
# Lesson: Implementing Your Own Permission

This lesson demonstrates how to write a class that defines its own special permission. The basic components in this lesson include:

1. A sample game called **ExampleGame**.
1. A class called **HighScore**, which is used by `ExampleGame` to store a user's latest high score.
1. A class called **HighScorePermission**, which is used to protect access to the user's stored high score value.
1. A user's security **policy file**, which grants permission to `ExampleGame` to update his/her high score.

The basic scenario is as follows:

1. A user plays `ExampleGame`.
1. If the user reaches a new high score, `ExampleGame` uses the `HighScore` class to save this new value.
1. The `HighScore` class looks into the user's security policy to check if `ExampleGame` has permission to update the user's high score value.
1. If `ExampleGame` has permission to update the high score, then the HighScore class updates that value.

We describe the key points of each of the basic components and then show a sample run:

- [ExampleGame](game.html)
- [The HighScore Class](highscore.html)
- [The HighScorePermission Class](perm.html)
- [A Sample Policy File](policy.html)
- [Putting It All Together](together.html)
