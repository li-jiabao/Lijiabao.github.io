
# How to Use HTML in Swing Components

Many Swing components display a text string as part of their GUI. By default, a component's text is displayed in a single font and color, all on one line. You can determine the font and color of a component's text by invoking the component's `setFont` and `setForeground` methods, respectively. For example, the following code creates a label and then sets its font and color:

```

label = new JLabel("A label");
label.setFont(new Font("Serif", Font.PLAIN, 14));
label.setForeground(new Color(0xffffdd));

```

If you want to mix fonts or colors within the text, or if you want formatting such as multiple lines, you can use HTML. HTML formatting can be used in all Swing buttons, menu items, labels, tool tips, and tabbed panes, as well as in components such as trees and tables that use labels to render text.

To specify that a component's text has HTML formatting, just put the `&lt;html&gt;` tag at the beginning of the text, then use any valid HTML in the remainder. Here is an example of using HTML in a button's text:

```

button = new JButton("&lt;html&gt;&lt;b&gt;&lt;u&gt;T&lt;/u&gt;wo&lt;/b&gt;&lt;br&gt;lines&lt;/html&gt;");

```

Here is the resulting button. 
<img src="../../figures/uiswing/components/HtmlButtonMetal.png" width="61" height="42" alt="Screenshot of a button that shows HTML in the Metal look and feel." />

## An Example: HtmlDemo

An application called `HtmlDemo` lets you play with HTML formatting by setting the text on a label. You can find the entire code for this program in 
[`HtmlDemo.java`](../examples/components/HtmlDemoProject/src/components/HtmlDemo.java). Here is a picture of the `HtmlDemo` example.

<li>Click the Launch button to run HtmlDemo using 
[Java&#8482; Web Start ](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#HtmlDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the HtmlDemo Application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/HtmlDemoProject/HtmlDemo.jnlp)<br /></li>
1. Edit the HTML formatting in the text area at the left and click the "Change the label" button. The label at the right shows the result.
1. Remove the &lt;html&gt; tag from the text area on the left. The label's text is no longer parsed as HTML.

## Example 2: ButtonHtmlDemo

Let us look at another example that uses HTML. `ButtonHtmlDemo` adds font, color, and other text formatting to three buttons. You can find the entire code for this program in 
[`ButtonHtmlDemo.java`](../examples/components/ButtonHtmlDemoProject/src/components/ButtonHtmlDemo.java). Here is a picture of the `ButtonHtmlDemo` example.

Click the Launch button to run ButtonHtmlDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#ButtonHtmlDemo).

The left and right buttons have multiple lines and text styles and are implemented using HTML. The middle button, on the other hand, uses just one line, font, and color, so it does not require HTML. Here is the code that specifies the text formatting for these three buttons:

```

b1 = new JButton("&lt;html&gt;&lt;center&gt;&lt;b&gt;&lt;u&gt;D&lt;/u&gt;isable&lt;/b&gt;&lt;br&gt;"
                 + "&lt;font color=#ffffdd&gt;middle button&lt;/font&gt;",
                 leftButtonIcon);
Font font = b1.getFont().deriveFont(Font.PLAIN);
b1.setFont(font);
...
b2 = new JButton("middle button", middleButtonIcon);
b2.setFont(font);
b2.setForeground(new Color(0xffffdd));
...
b3 = new JButton("&lt;html&gt;&lt;center&gt;&lt;b&gt;&lt;u&gt;E&lt;/u&gt;nable&lt;/b&gt;&lt;br&gt;"
                 + "&lt;font color=#ffffdd&gt;middle button&lt;/font&gt;",
                 rightButtonIcon);
b3.setFont(font);

```

Note that we have to use a `&lt;u&gt;` tag to cause the mnemonic characters "D" and "E" to be underlined in the buttons that use HTML. Note also that when a button is disabled, its HTML text unfortunately remains black, instead of becoming gray. (Refer to 
[bug #4783068](http://bugs.java.com/bugdatabase/view_bug.do?bug_id=4783068) to see if this situation changes.)

This section discussed how to use HTML in ordinary, non-text components. For information on components whose primary purpose is formatting text, see 
[Using Text Components](text.html).

If you are programming in JavaFX, see
[HTML Editor](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/editor.htm).
