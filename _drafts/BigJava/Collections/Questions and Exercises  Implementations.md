
# Questions and Exercises: Implementations

## Questions

1. You plan to write a program that uses several basic collection interfaces: `Set`, `List`, `Queue`, and `Map`. You're not sure which implementations will work best, so you decide to use general-purpose implementations until you get a better idea how your program will work in the real world. Which implementations are these?
1. If you need a `Set` implementation that provides value-ordered iteration, which class should you use?
1. Which class do you use to access wrapper implementations?

## Exercises

<li>Write a program that reads a text file, specified by the first command line argument, into a `List`. The program should then print random lines from the file, the number of lines printed to be specified by the second command line argument. Write the program so that a correctly-sized collection is allocated all at once, instead of being gradually expanded as the file is read in. Hint: To determine the number of lines in the file, use 
[`java.io.File.length`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#length--) to obtain the size of the file, then divide by an assumed size of an average line.</li>


[Check your answers.](answers.html)
