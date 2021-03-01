
# Before Internationalization

Suppose that you've written a program that displays three messages, as follows:

```

public class NotI18N {

    static public void main(String[] args) {

        System.out.println("Hello.");
        System.out.println("How are you?");
        System.out.println("Goodbye.");
    }
}

```

You've decided that this program needs to display these same messages for people living in France and Germany. Unfortunately your programming staff is not multilingual, so you'll need help translating the messages into French and German. Since the translators aren't programmers, you'll have to move the messages out of the source code and into text files that the translators can edit. Also, the program must be flexible enough so that it can display the messages in other languages, but right now no one knows what those languages will be.

It looks like the program needs to be internationalized.
