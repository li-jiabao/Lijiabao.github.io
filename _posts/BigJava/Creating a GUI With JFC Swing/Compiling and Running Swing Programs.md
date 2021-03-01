
# Compiling and Running Swing Programs

This section explains how to compile and run a Swing application from the command line. For information on compiling and running a Swing application using NetBeans IDE, see 
[Running Tutorial Examples in NetBeans IDE](../../information/examples.html). The compilation instructions work for all Swing programs &#151; applets, as well as applications. Here are the steps you need to follow:

- Install the latest release of the Java SE platform, if you haven't already done so.
- Create a program that uses Swing components.
- Compile the program.
- Run the program.

## Install the Latest Release of the Java SE Platform

You can download the latest release of the JDK for free from [http://www.oracle.com/technetwork/java/javase/downloads/index.html](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

## <a name="package" id="package">Create a Program That Uses Swing Components</a>

<a name="package__1" id="package__1">You can use a simple program we provide, called HelloWorldSwing, that brings up the GUI shown in the figure below. The program is in a single file, 
</a>[`HelloWorldSwing.java`](../examples/start/HelloWorldSwingProject/src/start/HelloWorldSwing.java). When you save this file, you must match the spelling and capitalization of its name exactly.

The `HelloWorldSwing.java` example, like all of our Swing tutorial examples, is created inside a package. If you look at the source code, you see the following line at the beginning of the file:

```

package start;

```

This means you must put the `HelloWorldSwing.java` file inside of a `start` directory. You compile and run the example from the directory above the `start` directory. The tutorial examples from the **Using Swing Components** lesson are inside of a `components` package and the examples from the **Writing Event Listeners** lesson are inside a `events` package, and so on. For more information, you might want to see the 
[`Packages`](../../java/package/index.html) lesson.

## Compile the Program

Your next step is to compile the program. To compile the example, from the directory above the 
[`HelloWorldSwing.java`](../examples/start/HelloWorldSwingProject/src/start/HelloWorldSwing.java) file:

```

javac start/HelloWorldSwing.java

```

If you prefer, you may compile the example from within the `start` directory:

```

javac HelloWorldSwing.java

```

but you must remember to leave the `start` directory to execute the program.

If you are unable to compile, make sure you are using the compiler in a recent release of the Java platform. You can verify the version of your compiler or Java Runtime Environment (JRE) using these commands

```

javac -version
java -version

```

Once you've updated your JDK, you should be able to use the programs in this trail without changes. Another common mistake is installing the JRE and not the full Java Development Kit (JDK) needed to compile these programs. Refer to the 
[Getting Started](../../getStarted/index.html) trail to help you solve any compiling problems you encounter. Another resource is the 
[Troubleshooting Guide for Java&#8482; SE 6 Desktop Technologies](http://www.oracle.com/technetwork/java/javase/index-142560.html).

## Run the Program

After you compile the program successfully, you can run it. From the directory above the `start` directory:

```

java start.HelloWorldSwing

```
