
# Environment Variables

Many operating systems use *environment variables* to pass configuration information to applications. Like properties in the Java platform, environment variables are key/value pairs, where both the key and the value are strings. The conventions for setting and using environment variables vary between operating systems, and also between command line interpreters. To learn how to pass environment variables to applications on your system, refer to your system documentation.

## Querying Environment Variables

On the Java platform, an application uses
[`System.getenv`](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#getenv--) to retrieve environment variable values. Without an argument, `getenv` returns a read-only instance of `java.util.Map`, where the map keys are the environment variable names, and the map values are the environment variable values. This is demonstrated in the 
[`<code>EnvMap`</code>](examples/EnvMap.java) example:

```


import java.util.Map;

public class EnvMap {
    public static void main (String[] args) {
        Map&lt;String, String&gt; env = System.getenv();
        for (String envName : env.keySet()) {
            System.out.format("%s=%s%n",
                              envName,
                              env.get(envName));
        }
    }
}


```

With a `String` argument, `getenv` returns the value of the specified variable. If the variable is not defined, `getenv` returns `null`. The 
[`<code>Env`</code>](examples/Env.java) example uses `getenv` this way to query specific environment variables, specified on the command line:

```


public class Env {
    public static void main (String[] args) {
        for (String env: args) {
            String value = System.getenv(env);
            if (value != null) {
                System.out.format("%s=%s%n",
                                  env, value);
            } else {
                System.out.format("%s is"
                    + " not assigned.%n", env);
            }
        }
    }
}


```

## Passing Environment Variables to New Processes

When a Java application uses a 
[`ProcessBuilder`](https://docs.oracle.com/javase/8/docs/api/java/lang/ProcessBuilder.html) object to create a new process, the default set of environment variables passed to the new process is the same set provided to the application's virtual machine process. The application can change this set using `ProcessBuilder.environment`.

## Platform Dependency Issues

There are many subtle differences between the way environment variables are implemented on different systems. For example, Windows ignores case in environment variable names, while UNIX does not. The way environment variables are used also varies. For example, Windows provides the user name in an environment variable called `USERNAME`, while UNIX implementations might provide the user name in `USER`, `LOGNAME`, or both.

To maximize portability, never refer to an environment variable when the same value is available in a system property. For example, if the operating system provides a user name, it will always be available in the system property `user.name`.
