
# Adding the Application Logic

It is now time to add in the application logic. <a name="a23" id="a23"></a>

## Step 1: Change the Default Variable Names

The figure below shows the default variable names as they currently appear within the Inspector. For each component, the variable name appears first, followed by the object's type in square brackets. For example, `jTextField1 [JTextField]` means that "jTextField1" is the variable name and "JTextField" is its type.

Default Variable Names

The default names are not very relevant in the context of this application, so it makes sense to change them from their defaults to something that is more meaningful. Right-click each variable name and choose "Change variable name." When you are finished, the variable names should appear as follows:

New Variable Names

The new variable names are "tempTextField", "celsiusLabel", "convertButton", and "fahrenheitLabel." Each change that you make in the Inspector will automatically propagate its way back into the source code. You can rest assured that compilation will not fail due to typos or mistakes of that nature &#8212; mistakes that are common when editing by hand. <a name="a24" id="a24"></a>

## Step 2: Register the Event Listeners

When an end-user interacts with a Swing GUI component (such as clicking the Convert button), that component will generate a special kind of object &#8212; called an *event object* &#8212; which it will then broadcast to any other objects that have previously registered themselves as *listeners* for that event. The NetBeans IDE makes event listener registration extremely simple:

In the Design Area, click on the Convert button to select it. Make sure that *only* the Convert button is selected (if the JFrame itself is also selected, this step will not work.) Right-click the Convert button and choose Events -&gt; Action -&gt; ActionPerformed. This will generate the required event-handling code, leaving you with empty method bodies in which to add your own functionality:

There are many different event types representing the various kinds of actions that an end-user can take (clicking the mouse triggers one type of event, typing at the keyboard triggers another, moving the mouse yet another, and so on.) Our application is only concerned with the ActionEvent; for more information about event handling, see 
[Writing Event Listeners](../events/index.html).

<a name="a25" id="a25"></a>

## Step 3: Add the Temperature Conversion Code

The final step is to simply paste the temperature conversion code into the empty method body. The following code is all that is necessary to convert a temperature from Celsius to Fahrenheit:

This example is not localizable because the `parseDouble` method is not localizable. This code snippet is for illustration purposes only. A more robust implementation would use the 
[Scanner](https://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html) class to parse the user input.

```

    //Parse degrees Celsius as a double and convert to Fahrenheit.
    int tempFahr = (int)((Double.parseDouble(**tempTextField**.getText()))
            * 1.8 + 32);
    **fahrenheitLabel**.setText(tempFahr + " Fahrenheit");

```

Simply copy this code and paste it into the convertButtonActionPerformed method as shown below:

With the conversion code in place, the application is now complete. <a name="a26" id="a26"></a>

## Step 4: Run the Application

Running the application is simply a matter of choosing Run -&gt; Run Main Project within the NetBeans IDE. The first time you run this application, you will be prompted with a dialog asking to set `CelsiusConverterGUI` as the main class for this project. Click the OK button, and when the program finishes compiling, you should see the application running in its own window.

Congratulations! You have completed your first Swing application!
