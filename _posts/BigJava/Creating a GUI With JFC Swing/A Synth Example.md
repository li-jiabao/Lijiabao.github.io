
# A Synth Example

In the lesson titled 
[A GroupLayout Example ](../../uiswing/layout/groupExample.html), `GroupLayout` was used to create a search dialog box called "Find." The program that created the dialog box, `Find.java`, used the cross platform ("Metal") look and feel with the "Ocean" theme:

This lesson creates the same dialog box with Synth, using an external XML file. Here is the listing of the 
[`<code>SynthDialog.java`</code>](../examples/lookandfeel/SynthDialogProject/src/lookandfeel/SynthDialog.java)file.

`SynthDialog.java` is exactly the same as `Find.java` except for the `initLookAndFeel()` method, which has been altered to use the Synth look and feel with an external file called `synthDemo.xml`. Here is the new `initLookAndFeel()` method:

```

private static void initLookAndFeel() {
   SynthLookAndFeel lookAndFeel = new SynthLookAndFeel();
       
   // SynthLookAndFeel load() method throws a checked exception
   // (java.text.ParseException) so it must be handled
   try {
      lookAndFeel.load(SynthDialog.class.getResourceAsStream("synthDemo.xml"),
                                                               SynthDialog.class);
      UIManager.setLookAndFeel(lookAndFeel);
   } 
            
   catch (ParseException e) {
      System.err.println("Couldn't get specified look and feel ("
                                   + lookAndFeel
                                    + "), for some reason.");
      System.err.println("Using the default look and feel.");
      e.printStackTrace();
   }
}

```

## The XML File

The XML file, `synthDemo.xml`, begins with a style bound to all regions. It is good practice to do this to ensure that regions without a style bound to them will contain something. This style makes all regions paint their background in an opaque color. It also sets a default font and default colors.

```

  &lt;!-- Style that all regions will use --&gt;
  &lt;style id="backingStyle"&gt;
    &lt;!-- Make all the regions opaque--&gt;
    &lt;opaque value="TRUE"/&gt;
    &lt;font name="Dialog" size="14"/&gt;
    &lt;state&gt;
      &lt;color value="#D8D987" type="BACKGROUND"/&gt;
      &lt;color value="RED" type="FOREGROUND"/&gt;
    &lt;/state&gt;
  &lt;/style&gt;
  &lt;bind style="backingStyle" type="region" key=".*"/&gt;

```

1. The color definitions must be inside a &lt;state&gt; element. This permits changing colors depending on state. The &lt;state&gt; element in `backingStyle` has no attributes and is therefore applied to all regions, irrespective of their state. If a region has other states, the states are merged with precedence given to state definitions that appear later in the file.

2. The font definition is not inside a &lt;state&gt; element because the font should *not* change when there is a change of state (many components are sized depending on their font, and a change in font could cause components to change in size unintentionally).

The next &lt;style&gt; element defined is for the text field, which is painted using an image.

```

  &lt;style id="textfield"&gt;
    &lt;insets top="4" left="6" bottom="4" right="6"/&gt;
    &lt;state&gt;
       &lt;font name="Verdana" size="14"/&gt;
       &lt;color value="#D2DFF2" type="BACKGROUND"/&gt;
       &lt;color value="#000000" type="TEXT_FOREGROUND"/&gt;
    &lt;/state&gt;
    &lt;imagePainter method="textFieldBorder" path="images/textfield.png"
                  sourceInsets="4 6 4 6" paintCenter="false"/&gt;
  &lt;/style&gt;
  &lt;bind style="textfield" type="region" key="TextField"/&gt;

```

1. The font and color definitions override the definitions in `backingStyle`.

2. The `insets` and `sourceInsets` are given the same values, which is just a coincidence because they are unrelated to each other.

3. The BACKGROUND color, #D2DFF2, is a pale blue&#8212;the same color as the background in the image, `textfield.png`.

4. `paintCenter` is `false` so that you can see the background color.

The next &lt;style&gt; element is for buttons that are painted with different images, depending on the button state. When the mouse passes over the button, its appearance changes. When it is clicked (PRESSED) the image changes again.

```

 &lt;style id="button"&gt;
        &lt;!-- Shift the text one pixel when pressed --&gt;
    &lt;property key="Button.textShiftOffset" type="integer" value="1"/&gt;
    &lt;!-- set size of buttons --&gt;
    &lt;insets top="15" left="20" bottom="15" right="20"/&gt;
    &lt;state&gt;
      &lt;imagePainter method="buttonBackground" path="images/button.png"
                           sourceInsets="10 10 10 10" /&gt;
      &lt;font name="Dialog" size="16"/&gt;
      &lt;color type="TEXT_FOREGROUND" value="#FFFFFF"/&gt;
    &lt;/state&gt;
              
    &lt;state value="PRESSED"&gt; 
      &lt;imagePainter method="buttonBackground"
          path="images/button_press.png"
                   sourceInsets="10 10 10 10" /&gt;
    &lt;/state&gt;
            
    &lt;state value="MOUSE_OVER"&gt;    
      &lt;imagePainter method="buttonBackground"
          path="images/button_over.png"
                 sourceInsets="10 10 10 10" /&gt;
    &lt;/state&gt;
  &lt;/style&gt;
  &lt;bind style="button" type="region" key="Button"/&gt;

```

1. The font and color definitions inside the &lt;state&gt; element without attributes apply to all button states. This is because the definitions of all states that apply (and the &lt;state&gt; element without attributes is one of these) will merge and there are no other font and color definitions that might take precedence.

2. The `sourceInsets` values are large enough that the curved corners of the button image will not be stretched.

3. The order of the `PRESSED` and `MOUSE_OVER` states is important. Since the mouse will always be over the button when it is pressed, both states will apply to a pressed button and the first state defined (`PRESSED`) will apply. When the mouse is over the button but it is not pressed, only the `MOUSE_OVER` state applies. If the order of the `PRESSED` and `MOUSE_OVER` states is reversed, the `PRESSED` state image will never be used.

The next &lt;style&gt; element is for checkboxes that are painted with different icons, depending on the checkbox state.

```

  &lt;style id="checkbox"&gt;
    &lt;imageIcon id="check_off" path="images/checkbox_off.png"/&gt;
    &lt;imageIcon id="check_on" path="images/checkbox_on.png"/&gt;
    &lt;property key="CheckBox.icon" value="check_off"/&gt;
    &lt;state value="SELECTED"&gt;   
      &lt;property key="CheckBox.icon" value="check_on"/&gt;
    &lt;/state&gt;
  &lt;/style&gt;
  &lt;bind style="checkbox" type="region" key="Checkbox"/&gt;    

```

1. You must use the &lt;imageIcon&gt; element to define any icons to be used.

2. The &lt;insets&gt; element and the `sourceInsets` attribute are not used with icons because they are rendered in their fixed size and are not stretched.

3. The icon used to render the checkbox is the icon named in the `CheckBox.icon` property. (see 
[`javax/swing/plaf/synth/doc-files/componentProperties.html`](https://docs.oracle.com/javase/8/docs/api/javax/swing/plaf/synth/doc-files/componentProperties.html)), which is the icon with id="check_off" unless the checkbox state is `SELECTED`.

The `synthDemo.xml` file is constructed of the styles presented above, wrapped in &lt;synth&gt;&lt;/synth&gt; tags. You can open the completed file by clicking 
[`<code>synthDemo.xml`</code>](../examples/lookandfeel/SynthDialogProject/src/lookandfeel/synthDemo.xml).

Click the Launch button to run the SynthDialog example using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the <a href="../examples/lookandfeel/index.html#SynthDialog">example index&lt;/
a&gt;.</a>
