
# Download and Try the Sample Application

This lesson uses a simple application that we provide to you.

<li>Create a file named `Count.java` on your local computer by either copying or downloading the 
[`Count.java`](examples/Count.java) source code. The examples in this lesson assume that you place `count` in the `C:\Test` directory.</li>
<li>The `count` application needs to access a text file containing the data it will process. Download a

[`sample data file`](examples/data), or use any other convenient text file as data.<br />
<br />
<hr />**Important:**&#160;Put your data file into a directory **other than** the directory containing your downloaded `count` class file. We suggest `C:\TestData\data`.<br />
<br />
Later in this lesson you will see how an application running under a security manager cannot read a file unless it has explicit permission to do so. However, an application can *always* read a file from the same directory (or a subdirectory). It does not need explicit permission.
<hr />
</li>
<li>Compile and then run the `Count` application to see what it does.<br />
<br />
When you run the `count` application, you need to specify (as an argument) the path name of a file to be read.<br />
<br />
`java Count C:\TestData\data`</li>

Here is a sample run:

```

    C:\Test&gt;**java Count C:\TestData\data**
    Counted 65 chars.

```
