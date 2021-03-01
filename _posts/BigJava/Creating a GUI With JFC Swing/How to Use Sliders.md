
# How to Use Sliders

A 
[`JSlider`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html) component is intended to let the user easily enter a numeric value bounded by a minimum and maximum value. If space is limited, a [spinner](spinner.html) is a possible alternative to a slider.

The following picture shows an application that uses a slider to control animation speed:

<li>Click the Launch button to run SliderDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#SliderDemo).[<img src="../../images/jws-launch-button.png" width="88" align="bottom" alt="Launches the SliderDemo application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/SliderDemoProject/SliderDemo.jnlp)<br /></li>
1. Use the slider to adjust the animation speed.
1. Push the slider to 0 to stop the animation.

Below is the code from the 
[`SliderDemo.java`](../examples/components/SliderDemoProject/src/components/SliderDemo.java) file that creates the slider in the previous example.

```

static final int FPS_MIN = 0;
static final int FPS_MAX = 30;
static final int FPS_INIT = 15;    //initial frames per second
. . .
JSlider framesPerSecond = new JSlider(JSlider.HORIZONTAL,
                                      FPS_MIN, FPS_MAX, FPS_INIT);
framesPerSecond.addChangeListener(this);

//Turn on labels at major tick marks.
framesPerSecond.setMajorTickSpacing(10);
framesPerSecond.setMinorTickSpacing(1);
framesPerSecond.setPaintTicks(true);
framesPerSecond.setPaintLabels(true);

```

By default, spacing for major and minor tick marks is zero. To see tick marks, you must explicitly set the spacing for either major or minor tick marks (or both) to a non-zero value and call the `setPaintTicks(true)` method. However, you also need labels for your tick marks. To display standard, numeric labels at major tick mark locations, set the major tick spacing, then call the `setPaintLabels(true)` method. The example program provides labels for its slider in this way. But you are not constrained to using only these labels. [Customizing Labels on a Slider](#labels) shows you how to customize slider labels. In addition, a slider feature allows you to set a font for the `JSlider` component.

```

Font font = new Font("Serif", Font.ITALIC, 15);
framesPerSecond.setFont(font);

```

When you move the slider's knob, the `stateChanged` method of the slider's `ChangeListener` is called. For information about change listeners, refer to [How to Write a Change Listener](../events/changelistener.html). Here is the change listener code that reacts to slider value changes:

```

public void stateChanged(ChangeEvent e) {
    JSlider source = (JSlider)e.getSource();
    if (!source.getValueIsAdjusting()) {
        int fps = (int)source.getValue();
        if (fps == 0) {
            if (!frozen) stopAnimation();
        } else {
            delay = 1000 / fps;
            timer.setDelay(delay);
            timer.setInitialDelay(delay * 10);
            if (frozen) startAnimation();
        }
    }
}

```

Notice that the `stateChanged` method changes the animation speed only if the `getValueIsAdjusting` method returns `false`. Many change events are fired as the user moves the slider knob. This program is interested only in the final result of the user's action.

## <a name="labels" id="labels">Customizing Labels on a Slider</a>

The demo below is a modified version of the SliderDemo that uses a slider with custom labels:



The source for this program can be found in 
[`SliderDemo2.java`](../examples/components/SliderDemo2Project/src/components/SliderDemo2.java). Click the Launch button to run SliderDemo2 using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#SliderDemo2).

The following code creates the slider and customizes its labels:

```

//Create the slider
JSlider framesPerSecond = new JSlider(JSlider.VERTICAL,
                                      FPS_MIN, FPS_MAX, FPS_INIT);
framesPerSecond.addChangeListener(this);
framesPerSecond.setMajorTickSpacing(10);
framesPerSecond.setPaintTicks(true);

//Create the label table
Hashtable labelTable = new Hashtable();
labelTable.put( new Integer( 0 ), new JLabel("Stop") );
labelTable.put( new Integer( FPS_MAX/10 ), new JLabel("Slow") );
labelTable.put( new Integer( FPS_MAX ), new JLabel("Fast") );
framesPerSecond.setLabelTable( labelTable );

framesPerSecond.setPaintLabels(true);

```

Each key-value pair in the hashtable specified with the `setLabelTable` method gives the position and the value of one label. The hashtable key must be of an `Integer` type and must have a value within the slider's range at which to place the label. The hashtable value associated with each key must be a `Component` object. This demo uses `JLabel` instances with text only. An interesting modification would be to use `JLabel` instances with icons or buttons that move the knob to the label's position.

Use the `createStandardLabels` method of the `JSlider` class to create a set of numeric labels positioned at a specific interval. You can also modify the table returned by the `createStandardLabels` method in order to customize it.

## <a name="api" id="api">The Slider API</a>

The following tables list the commonly used `JSlider` constructors and methods. See [The JComponent Class](jcomponent.html) for tables of commonly used inherited methods.

The API for using sliders is divided into these categories:

- [Creating the Slider](#creating)
- [Fine Tuning the Slider's Appearance](#looks)
- [Watching the Slider Operate](#operation)
- [Working Directly with the Data Model](#modelapi)
<th id="h1">Constructor</th><th id="h2">Purpose</th>
<td headers="h1">[JSlider()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#JSlider--)</td><td headers="h2">Creates a horizontal slider with the range 0 to 100 and an initial value of 50.</td>
<td headers="h1">[JSlider(int min, int max)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#JSlider-int-int-)<br />[JSlider(int min, int max, int value)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#JSlider-int-int-int-)</td><td headers="h2">Creates a horizontal slider with the specified minimum and maximum values. The third `int` argument, when present, specifies the slider's initial value.</td>
<td headers="h1">[JSlider(int orientation)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#JSlider-int-)<br />[JSlider(int orientation, int min, int max, int value)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#JSlider-int-int-int-int-)</td><td headers="h2">Creates a slider with the specified orientation, which must be either `JSlider.HORIZONTAL` or `JSlider.VERTICAL`. The last three `int` arguments, when present, specify the slider's minimum, maximum, and initial values, respectively.</td>
<td headers="h1">[JSlider(BoundedRangeModel)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#JSlider-javax.swing.BoundedRangeModel-)</td><td headers="h2">Creates a horizontal slider with the specified model, which manages the slider's minimum, maximum, and current values and their relationships.</td>
<th id="h101">Method</th><th id="h102">Purpose</th>
<td headers="h101">[void setValue(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#setValue-int-)<br />[int getValue()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#getValue--)</td><td headers="h102">Sets or gets the slider's current value. The set method also positions the slider's knob.</td>
<td headers="h101">[void setOrientation(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#setOrientation-int-)<br />[int getOrientation()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#getOrientation--)</td><td headers="h102">Sets or gets the orientation of the slider. Possible values are `JSlider.HORIZONTAL` or `JSlider.VERTICAL`.</td>
<td headers="h101">[void setInverted(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#setInverted-boolean-)<br />[boolean getInverted()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#getInverted--)</td><td headers="h102">Sets or gets whether the maximum is shown at the left of a horizontal slider or at the bottom of a vertical one, thereby inverting the slider's range.</td>
<td headers="h101">[void setMinimum(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#setMinimum-int-)<br />[int getMinimum()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#getMinimum--)<br />[void setMaximum(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#setMaximum-int-)<br />[int getMaximum()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#getMaximum--)</td><td headers="h102">Sets or gets the minimum or maximum values of the slider. Together, these methods set or get the slider's range.</td>
<td headers="h101">[void setMajorTickSpacing(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#setMajorTickSpacing-int-)<br />[int getMajorTickSpacing()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#getMajorTickSpacing--)<br />[void setMinorTickSpacing(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#setMinorTickSpacing-int-)<br />[int getMinorTickSpacing()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#getMinorTickSpacing--)</td><td headers="h102">Sets or gets the range between major and minor ticks. You must call `setPaintTicks(true)` for the tick marks to appear.</td>
<td headers="h101">[void setPaintTicks(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#setPaintTicks-boolean-)<br />[boolean getPaintTicks()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#getPaintTicks--)</td><td headers="h102">Sets or gets whether tick marks are painted on the slider.</td>
<td headers="h101">[void setPaintLabels(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#setPaintLabels-boolean-)<br />[boolean getPaintLabels()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#getPaintLabels--)</td><td headers="h102">Sets or gets whether labels are painted on the slider. You can provide custom labels with `setLabelTable` or get automatic labels by setting the major tick spacing to a non-zero value.</td>
<td headers="h101">[void setLabelTable(Dictionary)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#setLabelTable-java.util.Dictionary-)<br />[Dictionary getLabelTable()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#getLabelTable--)</td><td headers="h102">Sets or gets the labels for the slider. You must call `setPaintLabels(true)` for the labels to appear.</td>
<td headers="h101">[Hashtable createStandardLabels(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#createStandardLabels-int-)<br />[Hashtable createStandardLabels(int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#createStandardLabels-int-int-)</td><td headers="h102">Creates a standard set of numeric labels. The first `int` argument specifies the increment, the second `int` argument specifies the starting point. When left unspecified, the starting point is set to the slider's minimum number.</td>
<td headers="h101">[setFont(java.awt.Font)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#setFont-java.awt.Font-)</td><td headers="h102">Sets the font for slider labels .</td>
<th id="h201">Method</th><th id="h202">Purpose</th>
<td headers="h201">[void addChangeListener(ChangeListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#addChangeListener-javax.swing.event.ChangeListener-)</td><td headers="h202">Registers a change listener with the slider.</td>
<td headers="h201">[boolean getValueIsAdjusting()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#getValueIsAdjusting--)</td><td headers="h202">Determines whether the user gesture to move the slider's knob is complete.</td>
<th id="h301">Class, Interface, or Method</th><th id="h302">Purpose</th>
<td headers="h301">[BoundedRangeModel](https://docs.oracle.com/javase/8/docs/api/javax/swing/BoundedRangeModel.html)</td><td headers="h302">The interface required for the slider's data model.</td>
<td headers="h301">[DefaultBoundedRangeModel](https://docs.oracle.com/javase/8/docs/api/javax/swing/DefaultBoundedRangeModel.html)</td><td headers="h302">An implementation of the `BoundedRangeModel` interface.</td>
<td headers="h301">[void setModel()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#setModel-javax.swing.BoundedRangeModel-)<br />[getModel()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSlider.html#getModel--)<br />**(in `JSlider`)**</td><td headers="h302">Sets or gets the data model used by the slider. You can also set the model by using the constructor that takes a single argument of type `BoundedRangeModel`.</td>

## <a name="eg" id="eg">Examples that Use Sliders</a>

This table shows the examples that use `JSlider` and where those examples are described.
<th id="h401" align="left">Example</th><th id="h402" align="left">Where Described</th><th id="h403" align="left">Notes</th>

If you are programming in JavaFX, see
[Slider](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/slider.htm).
