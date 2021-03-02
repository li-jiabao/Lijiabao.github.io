
# Questions and Exercises: The Platform Environment

## Questions

1. A programmer installs a new library contained in a .jar file. In order to access the library from his code, he sets the CLASSPATH environment variable to point to the new .jar file. Now he finds that he gets an error message when he tries to launch simple applications:

```

**java Hello**
Exception in thread "main" java.lang.NoClassDefFoundError: Hello

```

In this case, the `Hello` class is compiled into a .class file in the current directory &#151; yet the `java` command can't seem to find it. What's going wrong?

## Exercises

1. Write an application, `PersistentEcho`, with the following features:

- If `PersistentEcho` is run with command line arguments, it prints out those arguments. It also saves the string printed out to a property, and saves the property to a file called `PersistentEcho.txt`
- If `PersistentEcho` is run with no command line arguments, it looks for an environment variable called PERSISTENTECHO. If that variable exists, `PersistentEcho` prints out its value, and also saves the value in the same way it does for command line arguments.
- If `PersistentEcho` is run with no command line arguments, and the PERSISTENTECHO environment variable is not defined, it retrieves the property value from `PersistentEcho.txt` and prints that out.


[Check your answers.](answers.html)
