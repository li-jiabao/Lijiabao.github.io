
# Questions and Exercises: Creating and Using Packages

## Questions

Assume you have written some classes. Belatedly, you decide they should be split into three packages, as listed in the following table. Furthermore, assume the classes are currently in the default package (they have no `package` statements).

&#160;
<th id="h1">Package Name</th><th id="h2">Class Name</th>
<td headers="h1">`mygame.server`</td><td headers="h2">`Server`</td>
<td headers="h1">`mygame.shared`</td><td headers="h2">`Utilities`</td>
<td headers="h1">`mygame.client`</td><td headers="h2">`Client`</td>

1. Which line of code will you need to add to each source file to put each class in the right package?
1. To adhere to the directory structure, you will need to create some subdirectories in the development directory and put source files in the correct subdirectories. What subdirectories must you create? Which subdirectory does each source file go in?
1. Do you think you'll need to make any other changes to the source files to make them compile correctly? If so, what?

## Exercises

Download the source files as listed here.

<li>
[`Client`](question/Client.java)</li>
<li>
[`Server`](question/Server.java)</li>
<li>
[`Utilities`](question/Utilities.java)</li>

1. Implement the changes you proposed in questions 1 through 3 using the source files you just downloaded.
1. Compile the revised source files. (*Hint:* If you're invoking the compiler from the command line (as opposed to using a builder), invoke the compiler from the directory that contains the `mygame` directory you just created.)

&#160;
