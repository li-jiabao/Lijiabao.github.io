
# How to Use Password Fields

The 
[`JPasswordField`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPasswordField.html) class, a subclass of `JTextField`, provides specialized text fields for password entry. For security reasons, a password field does not show the characters that the user types. Instead, the field displays a character different from the one typed, such as an asterisk '*'. As another security precaution, a password field stores its value as an array of characters, rather than as a string. Like an ordinary [text field](textfield.html), a password field fires an 
[action event](../events/actionlistener.html) when the user indicates that text entry is complete, for example by pressing the Enter button.

Here is a picture of a demo that opens a small window and prompts the user to type in a password.

Click the Launch button to run PasswordDemo using 
[Java&#8482; Web Start ](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#PasswordDemo).

The password is "bugaboo". The password "bugaboo" is an example only. Use secure authentication methods in production systems. You can find the entire code for this program in 
[`<code>PasswordDemo.java`</code>](../examples/components/PasswordDemoProject/src/components/PasswordDemo.java). Here is the code that creates and sets up the password field:

```

passwordField = new JPasswordField(10);
passwordField.setActionCommand(OK);
passwordField.addActionListener(this);

```

The argument passed into the `JPasswordField` constructor indicates the preferred size of the field, which is at least 10 columns wide in this case. By default a password field displays a dot for each character typed. If you want to change the echo character, call the `setEchoChar` method. The code then adds an action listener to the password field, which checks the value typed in by the user. Here is the implementation of the action listener's `actionPerformed` method:

```

public void actionPerformed(ActionEvent e) {
    String cmd = e.getActionCommand();

    if (OK.equals(cmd)) { //Process the password.
        **char[] input = passwordField.getPassword();**
        if (isPasswordCorrect(input)) {
            JOptionPane.showMessageDialog(controllingFrame,
                "Success! You typed the right password.");
        } else {
            JOptionPane.showMessageDialog(controllingFrame,
                "Invalid password. Try again.",
                "Error Message",
                JOptionPane.ERROR_MESSAGE);
        }

        //Zero out the possible password, for security.
        Arrays.fill(input, '0');

        passwordField.selectAll();
        resetFocus();
    } else **...//handle the Help button...**
}

```

A program that uses a password field typically validates the password before completing any actions that require the password. This program calls a private method, `isPasswordCorrect`, that compares the value returned by the `getPassword` method to a value stored in a character array. Here is its code:

```

private static boolean isPasswordCorrect(char[] input) {
    boolean isCorrect = true;
    char[] correctPassword = { 'b', 'u', 'g', 'a', 'b', 'o', 'o' };

    if (input.length != correctPassword.length) {
        isCorrect = false;
    } else {
        isCorrect = Arrays.equals (input, correctPassword);
    }

    //Zero out the password.
    Arrays.fill(correctPassword,'0');

    return isCorrect;
}

```

## <a name="api" id="api">The Password Field API</a>

The following tables list the commonly used `JPasswordField` constructors and methods. For information on the API that password fields inherit, see [How to Use Text Fields](textfield.html).
<th id="h1">Constructor or Method</th><th id="h2">Purpose</th>
<td headers="h1">[JPasswordField()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPasswordField.html#JPasswordField--)<br />[JPasswordField(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPasswordField.html#JPasswordField-java.lang.String-)<br />[JPasswordField(String, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPasswordField.html#JPasswordField-java.lang.String-int-)<br />[JPasswordField(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPasswordField.html#JPasswordField-int-)<br />[JPasswordField(Document, String, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPasswordField.html#JPasswordField-javax.swing.text.Document-java.lang.String-int-)</td><td headers="h2">Creates a password field. When present, the `int` argument specifies the desired width in columns. The `String` argument contains the field's initial text. The `Document` argument provides a custom model for the field.</td>
<td headers="h1">[char[] getPassword()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPasswordField.html#getPassword--)</td><td headers="h2">Returns the password as an array of characters.</td>
<td headers="h1">[void setEchoChar(char)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPasswordField.html#setEchoChar-char-)<br />[char getEchoChar()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPasswordField.html#getEchoChar--)</td><td headers="h2">Sets or gets the echo character which is displayed instead of the actual characters typed by the user.</td>
<td headers="h1">[void addActionListener(ActionListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextField.html#addActionListener-java.awt.event.ActionListener-)<br />[void removeActionListener(ActionListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextField.html#removeActionListener-java.awt.event.ActionListener-)<br />**(defined in `JTextField`)**</td><td headers="h2">Adds or removes an action listener.</td>
<td headers="h1">[void selectAll()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#selectAll--)<br />**(defined in `JTextComponent`)**</td><td headers="h2">Selects all characters in the password field.</td>

## <a name="eg" id="eg">Examples That Use Password Fields</a>

[PasswordDemo](../examples/components/index.html#PasswordDemo) is the Tutorial's only example that uses a `JPasswordField` object. However, the Tutorial has many examples that use `JTextField` objects, whose API is inherited by `JPasswordField`. See [Examples That Use Text Fields](textfield.html#eg) for further information.

If you are programming in JavaFX, see
[Password Fields](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/password-field.htm).
