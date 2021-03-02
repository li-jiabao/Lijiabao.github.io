
# How to Use Spinners

Spinners are similar to 
[combo boxes](combobox.html) and 
[lists](list.html) in that they let the user choose from a range of values. Like editable combo boxes, spinners allow the user to type in a value. Unlike combo boxes, spinners do not have a drop-down list that can cover up other components. Because spinners do not display possible values &#151; only the current value is visible &#151; they are often used instead of combo boxes or lists when the set of possible values is extremely large. However, spinners should only be used when the possible values and their sequence are obvious.

A spinner is a compound component with three subcomponents: two small buttons and an **editor**. The editor can be any `JComponent`, but by default it is implemented as a panel that contains a [formatted text field](formattedtextfield.html). The spinner's possible and current values are managed by its **model**.

Here is a picture of an application named `SpinnerDemo` that has three spinners used to specify dates:

The code for the main class can be found in 
[`SpinnerDemo.java`](../examples/components/SpinnerDemoProject/src/components/SpinnerDemo.java). The Month spinner displays the name of the first month in the user's locale. The possible values for this spinner are specified using an array of strings. The Year spinner displays one value of a range of integers, initialized to the current year. The Another Date spinner displays one value in a range of `Date` objects (initially the current date) in a custom format that shows just a month and year.

<li>Click the Launch button to run SpinnerDemo using 
[Java&#8482; Web Start ](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#SpinnerDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the SpinnerDemo Application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/SpinnerDemoProject/SpinnerDemo.jnlp)<br /></li>
<li>With the Month spinner, use the arrow buttons or keys to cycle forward and backward through the possible values.<br />
Note that the lowest value is the first month of the year (for example, January) and the highest is the last (for example, December). The exact values depend on your locale. Also note that the values do not cycle &#151; you cannot use the up arrow button or key to go from December directly to January &#151; because the standard spinner models do not support cycling.</li>
<li>Type in a valid month name for your locale &#151; for example, July.<br />
Note that the spinner automatically completes the month name.</li>
<li>Moving on to the Year spinner, try typing a year over 100 years ago &#151; for example, 1800 &#151; and then click on another spinner or press the Tab key to move the focus out of the spinner.<br />
Because this program restricts the spinner's model to numbers within 100 years of the current year, 1800 is invalid. When the focus moves out of the spinner, the displayed text changes back to the last valid value.</li>
<li>Moving to the Another Date spinner, use the arrow buttons or keys to change the date.<br />
Note that by default the first part of the date &#151; in this case, the month number &#151; changes. You can change which part of the date changes either by clicking the mouse or using the arrow keys to move to another part of the date.</li>

To create a spinner, first create its model and then pass the model into the 
[JSpinner](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.html) constructor. For example:

```

String[] monthStrings = getMonthStrings(); //get month names
SpinnerListModel monthModel = new SpinnerListModel(monthStrings);
JSpinner spinner = new JSpinner(monthModel);

```

The rest of this section covers the following topics:

- [Using Standard Spinner Models and Editors](#standard)
- [Specifying Spinner Formatting](#format)
- [Creating Custom Spinner Models and Editors](#model)
- [Detecting Spinner Value Changes](#change)
- [The Spinner API](#api)
- [Examples That Use Spinners](#eg)

&#160;

## <a name="standard" id="standard">Using Standard Spinner Models and Editors</a>

The Swing API provides three spinner models:

```

SpinnerModel model =
        new SpinnerNumberModel(currentYear, //initial value
                               currentYear - 100, //min
                               currentYear + 100, //max
                               1);                //step

```

```

Date initDate = calendar.getTime();
calendar.add(Calendar.YEAR, -100);
Date earliestDate = calendar.getTime();
calendar.add(Calendar.YEAR, 200);
Date latestDate = calendar.getTime();
model = new SpinnerDateModel(initDate,
                             earliestDate,
                             latestDate,
                             Calendar.YEAR);

```

When you set the spinner's model, the spinner's editor is automatically set. The Swing API provides an editor class corresponding to each of the three model classes listed above. These classes &#151; 
[JSpinner.ListEditor](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.ListEditor.html), 
[JSpinner.NumberEditor](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.NumberEditor.html), and 
[JSpinner.DateEditor](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.DateEditor.html) &#151; are all subclasses of the 
[JSpinner.DefaultEditor](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.DefaultEditor.html) class that feature editable formatted text fields. If you use a model that does not have an editor associated with it, the editor is by default a `JSpinner.DefaultEditor` instance with a non-editable formatted text field.

## <a name="format" id="format">Specifying Spinner Formatting</a>

To change the formatting used in a standard spinner editor, you can create and set the editor yourself.

The `JSpinner.NumberEditor` and `JSpinner.DateEditor` classes have constructors that allow you to create an editor that formats its data in a particular way. For example, the following code sets up the Another Date spinner so that instead of using the default date format, which is long and includes the time, it shows just a month and year in a compact way.

```

spinner.setEditor(new JSpinner.DateEditor(spinner, "MM/yyyy"));

```

You can play with date formats by running `ComboBoxDemo2` example. Click the Launch button to run ComboBoxDemo2 using 
[Java&#8482; Web Start ](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#ComboBoxDemo2).

For information about format strings, see the 
[Formatting](../../i18n/format/index.html) lesson of the Internationalization trail.

To change the formatting when using a default editor, you can obtain the editor's formatted text field and invoke methods on it. You can call those methods using the `getTextField` method defined in the `JSpinner.DefaultEditor` class. Note that the Swing-provided editors are not formatted text fields. Instead, they are the `JPanel` instances that contain a formatted text field. Here is an example of getting and invoking methods on the editor's formatted text field:

```

//Tweak the spinner's formatted text field.
ftf = getTextField(spinner);
if (ftf != null ) {
    ftf.setColumns(8); //specify more width than we need
    ftf.setHorizontalAlignment(JTextField.RIGHT);
}
...

public JFormattedTextField getTextField(JSpinner spinner) {
    JComponent editor = spinner.getEditor();
    if (editor instanceof JSpinner.DefaultEditor) {
        return ((JSpinner.DefaultEditor)editor).getTextField();
    } else {
        System.err.println("Unexpected editor type: "
                           + spinner.getEditor().getClass()
                           + " isn't a descendant of DefaultEditor");
        return null;
    }
}

```

## <a name="model" id="model">Creating Custom Spinner Models and Editors</a>

If the existing spinner models or editors do not meet your needs, you can create your own.

The easiest route to creating a custom spinner model is to create a subclass of an existing `AbstractSpinnerModel` subclass that already does most of what you need. An alternative is to implement your own class by extending 
[`AbstractSpinnerModel`](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractSpinnerModel.html) class, which implements the event notifications required for all spinner models.

The following subclass of `SpinnerListModel` implements a spinner model that cycles through an array of objects. It also lets you specify a second spinner model that will be updated whenever the cycle begins again. For example, if the array of objects is a list of months, the linked model could be for a spinner that displays the year. When the month flips over from December to January the year is incremented. Similarly, when the month flips back from January to December the year is decremented.

```

public class CyclingSpinnerListModel extends SpinnerListModel {
    Object firstValue, lastValue;
    SpinnerModel linkedModel = null;

    public CyclingSpinnerListModel(Object[] values) {
        super(values);
        firstValue = values[0];
        lastValue = values[values.length - 1];
    }

    public void setLinkedModel(SpinnerModel linkedModel) {
        this.linkedModel = linkedModel;
    }

    public Object getNextValue() {
        Object value = super.getNextValue();
        if (value == null) {
            value = firstValue;
            if (linkedModel != null) {
                linkedModel.setValue(linkedModel.getNextValue());
            }
        }
        return value;
    }

    public Object getPreviousValue() {
        Object value = super.getPreviousValue();
        if (value == null) {
            value = lastValue;
            if (linkedModel != null) {
                linkedModel.setValue(linkedModel.getPreviousValue());
            }
        }
        return value;
    }
}

```

The `CyclingSpinnerListModel` model is used for the Month spinner in the `SpinnerDemo2` example, an example that is almost identical to the `SpinnerDemo`. Click the Launch button to run SpinnerDemo2 using 
[Java&#8482; Web Start ](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#SpinnerDemo2).

As we mentioned before, if you implement a spinner model that does not descend from `SpinnerListModel`, `SpinnerNumberModel`, or `SpinnerDateModel`, then the spinner's default editor is a non-editable instance of `JSpinner.DefaultEditor`. As you have already seen, you can set a spinner's editor by invoking the `setEditor` method on the spinner after the spinner's model property has been set. An alternative to using `setEditor` is to create a subclass of the `JSpinner` class and override its `createEditor` method so that it returns a particular kind of editor whenever the spinner model is of a certain type.

In theory at least, you can use any `JComponent` instance as an editor. Possibilities include using a subclass of a standard component such as `JLabel`, or a component you have implemented from scratch, or a subclass of `JSpinner.DefaultEditor`. The only requirements are that the editor must be updated to reflect changes in the spinner's value, and it must have a reasonable preferred size. The editor should generally also set its tool tip text to whatever tool tip text has been specified for the spinner. An example of implementing an editor is provided in the next section.

## <a name="change" id="change">Detecting Spinner Value Changes</a>

You can detect that a spinner's value has changed by registering a change listener on either the spinner or its model. Here is an example of implementing such a change listener. This example is from `SpinnerDemo3`, which is based on `SpinnerDemo` and uses a change listener to change the color of some text to match the value of the Another Date spinner. Click the Launch button to run SpinnerDemo3 using 
[Java&#8482; Web Start ](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#SpinnerDemo3).

```

public class SpinnerDemo3 extends JPanel
                          implements ChangeListener {
    protected Calendar calendar;
    protected JSpinner dateSpinner;
    ...
    public SpinnerDemo3() {
        ...
        SpinnerDateModel dateModel = ...;
        ...
        setSeasonalColor(dateModel.getDate()); //initialize color

        //Listen for changes on the date spinner.
        dateSpinner.addChangeListener(this);
        ...
    }

    public void stateChanged(ChangeEvent e) {
        SpinnerModel dateModel = dateSpinner.getModel();
        if (dateModel instanceof SpinnerDateModel) {
            setSeasonalColor(((SpinnerDateModel)dateModel).getDate());
        }
    }

    protected void setSeasonalColor(Date date) {
        calendar.setTime(date);
        int month = calendar.get(Calendar.MONTH);
        JFormattedTextField ftf = getTextField(dateSpinner);
        if (ftf == null) return;

        //Set the color to match northern hemisphere seasonal conventions.
        switch (month) {
            case 2:  //March
            case 3:  //April
            case 4:  //May
                     ftf.setForeground(SPRING_COLOR);
                     break;
            ...
            default: //December, January, February
                     ftf.setForeground(WINTER_COLOR);
        }
    }
    ...
}

```

The following example implements an editor which has a change listener so that it can reflect the spinner's current value. This particular editor displays a solid color of gray, ranging anywhere from white to black. Click the Launch button to run SpinnerDemo4 using 
[Java&#8482; Web Start ](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#SpinnerDemo4).

```

**...//Where the components are created:**
JSpinner spinner = new JSpinner(new GrayModel(170));
spinner.setEditor(new GrayEditor(spinner));

class GrayModel extends SpinnerNumberModel {
    ...
}

class GrayEditor extends JLabel
                 implements ChangeListener {
    public GrayEditor(JSpinner spinner) {
        setOpaque(true);
        ...
        //Get info from the model.
        GrayModel myModel = (GrayModel)(spinner.getModel());
        setBackground(myModel.getColor());
        spinner.addChangeListener(this);
        ...
        updateToolTipText(spinner);
    }

    protected void updateToolTipText(JSpinner spinner) {
        String toolTipText = spinner.getToolTipText();
        if (toolTipText != null) {
            //JSpinner has tool tip text.  Use it.
            if (!toolTipText.equals(getToolTipText())) {
                setToolTipText(toolTipText);
            }
        } else {
            //Define our own tool tip text.
            GrayModel myModel = (GrayModel)(spinner.getModel());
            int rgb = myModel.getIntValue();
            setToolTipText("(" + rgb + "," + rgb + "," + rgb + ")");
        }
    }

    public void stateChanged(ChangeEvent e) {
            JSpinner mySpinner = (JSpinner)(e.getSource());
            GrayModel myModel = (GrayModel)(mySpinner.getModel());
            setBackground(myModel.getColor());
            updateToolTipText(mySpinner);
    }
}

```

## <a name="api" id="api">The Spinner API</a>

The following tables list some of the commonly used API for using spinners. If you need to deal directly with the editor's formatted text field, you should also see [The FormattedTextField API](formattedtextfield.html#api). Other methods you might use are listed in the API tables in [The JComponent Class](jcomponent.html#api).

- [Classes Related to Spinners](#newclassesapi)
- [Useful JSpinner Constructors and Methods](#jspinnerapi)
- [Useful Editor Constructors and Methods](#editorapi)
- [SpinnerListModel Methods](#listapi)
- [SpinnerDateModel Methods](#dateapi)
- [SpinnerNumberModel Methods](#numberapi)
<th id="h1" align="left">Class or Interface</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[JSpinner](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.html)</td><td headers="h2">A single-line input field that allows the user to select a number or object value from an ordered sequence.</td>
<td headers="h1">[SpinnerModel](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerModel.html)</td><td headers="h2">The interface implemented by all spinner models.</td>
<td headers="h1">[AbstractSpinnerModel](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractSpinnerModel.html)</td><td headers="h2">The usual superclass for spinner model implementations.</td>
<td headers="h1">[SpinnerListModel](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerListModel.html)</td><td headers="h2">A subclass of `AbstractSpinnerModel` whose values are defined by an array or a `List`.</td>
<td headers="h1">[SpinnerDateModel](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerDateModel.html)</td><td headers="h2">A subclass of `AbstractSpinnerModel` that supports sequences of `Date` instances.</td>
<td headers="h1">[SpinnerNumberModel](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerNumberModel.html)</td><td headers="h2">A subclass of `AbstractSpinnerModel` that supports sequences of numbers.</td>
<td headers="h1">[JSpinner.DefaultEditor](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.DefaultEditor.html)</td><td headers="h2">Implements an uneditable component that displays the spinner's value. Subclasses of this class are generally more specialized (and editable).</td>
<td headers="h1">[JSpinner.ListEditor](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.ListEditor.html)</td><td headers="h2">A subclass of `JSpinner.DefaultEditor` whose values are defined by an array or a `List`.</td>
<td headers="h1">[JSpinner.DateEditor](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.DateEditor.html)</td><td headers="h2">A subclass of `JSpinner.DefaultEditor` that supports sequences of `Date` instances.</td>
<td headers="h1">[JSpinner.NumberEditor](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.NumberEditor.html)</td><td headers="h2">A subclass of `JSpinner.DefaultEditor` that supports sequences of numbers.</td>
<th id="h101" align="left">Constructor or Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[JSpinner()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.html#JSpinner--)<br />[JSpinner(SpinnerModel)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.html#JSpinner-javax.swing.SpinnerModel-)</td><td headers="h102">Creates a new `JSpinner`. The no-argument constructor creates a spinner with an integer `SpinnerNumberModel` with an initial value of 0 and no minimum or maximum limits. The optional parameter on the second constructor allows you to specify your own `SpinnerModel`.</td>
<td headers="h101">[void setValue(java.lang.Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.html#setValue-java.lang.Object-)<br />[Object getValue()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.html#getValue--)</td><td headers="h102">Sets or gets the currently displayed element of the sequence.</td>
<td headers="h101">[Object getNextValue()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.html#getNextValue--)<br />[Object getPreviousValue()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.html#getPreviousValue--)</td><td headers="h102">Gets the object in the sequence that comes before or after the object returned by the `getValue` method.</td>
<td headers="h101">[SpinnerModel getModel()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.html#getModel--)<br />[void setModel(SpinnerModel)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.html#setModel-javax.swing.SpinnerModel-)</td><td headers="h102">Gets or sets the spinner's model.</td>
<td headers="h101">[JComponent getEditor()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.html#getEditor--)<br />[void setEditor(JComponent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.html#setEditor-javax.swing.JComponent-)</td><td headers="h102">Gets or sets the spinner's editor, which is often an object of type `JSpinner.DefaultEditor`.</td>
<td headers="h101">[protected JComponent createEditor(SpinnerModel)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.html#createEditor-javax.swing.SpinnerModel-)</td><td headers="h102">Called by the `JSpinner` constructors to create the spinner's editor. Override this method to associate an editor with a particular type of model.</td>

&#160;
<th id="h201" align="left">Constructor or Method</th><th id="h202" align="left">Purpose</th>
<td headers="h201">[JSpinner.NumberEditor(JSpinner, String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.NumberEditor.html#JSpinner.NumberEditor-javax.swing.JSpinner-java.lang.String-)</td><td headers="h202">Creates a `JSpinner.NumberEditor` instance that displays and allows editing of the number value of the specified spinner. The string argument specifies the format to use to display the number. See the API documentation for [DecimalFormat](https://docs.oracle.com/javase/8/docs/api/java/text/DecimalFormat.html) for information about decimal format strings.</td>
<td headers="h201">[JSpinner.DateEditor(JSpinner, String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.DateEditor.html#JSpinner.DateEditor-javax.swing.JSpinner-java.lang.String-)</td><td headers="h202">Creates a `JSpinner.DateEditor` instance that displays and allows editing of the `Date` value of the specified spinner. The string argument specifies the format to use to display the date. See the API documentation for [SimpleDateFormat](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html) for information about date format strings.</td>
<td headers="h201">[JFormattedTextField getTextField()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSpinner.DefaultEditor.html#getTextField--)<br />**(defined in `JSpinner.DefaultEditor`)**</td><td headers="h202">Gets the formatted text field that provides the main GUI for this editor.</td>
<th id="h301" align="left">Method</th><th id="h302" align="left">Purpose</th>
<td headers="h301">[void setList(List)](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerListModel.html#setList-java.util.List-)<br />[List getList()](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerListModel.html#getList--)</td><td headers="h302">Sets or gets the `List` that defines the sequence for this model.</td>
<th id="h401" align="left">Method</th><th id="h402" align="left">Purpose</th>
<td headers="h401"></td>
<td headers="h401">[void setValue(Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerDateModel.html#setValue-java.lang.Object-)<br />[Date getDate()](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerDateModel.html#getDate--)<br />[Object getValue()](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerDateModel.html#getValue--)</td><td headers="h402">Sets or gets the current `Date` for this sequence.</td>
<td headers="h401">[void setStart(Comparable)](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerDateModel.html#setStart-java.lang.Comparable-)<br />[Comparable getStart()](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerDateModel.html#getStart--)</td><td headers="h402">Sets or gets the first `Date` in this sequence. Use `null` to specify that the spinner has no lower limit.</td>
<td headers="h401">[void setEnd(Comparable)](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerDateModel.html#setEnd-java.lang.Comparable-)<br />[Comparable getEnd()](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerDateModel.html#getEnd--)</td><td headers="h402">Sets or gets the last `Date` in this sequence. Use `null` to specify that the spinner has no upper limit.</td>
<td headers="h401">[void setCalendarField(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerDateModel.html#setCalendarField-int-)<br />[int getCalendarField()](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerDateModel.html#getCalendarField--)</td><td headers="h402">Sets or gets the size of the date value increment used by the `getNextValue` and `getPreviousValue` methods. This property is **not** used when the user explicitly increases or decreases the value; instead, the selected part of the formatted text field is incremented or decremented. The specified parameter must be one of the following constants, defined in `Calendar`: `ERA`, `YEAR`, `MONTH`, `WEEK_OF_YEAR`, `WEEK_OF_MONTH`, `DAY_OF_MONTH`, `DAY_OF_YEAR`, `DAY_OF_WEEK`, `DAY_OF_WEEK_IN_MONTH`, `AM_PM`, `HOUR_OF_DAY`, `MINUTE`, `SECOND`, `MILLISECOND`.</td>
<th id="h501" align="left">Method</th><th id="h502" align="left">Purpose</th>
<td headers="h501"></td>
<td headers="h501">[void setValue(Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerNumberModel.html#setValue-java.lang.Object-)<br />[Number getNumber()](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerNumberModel.html#getNumber--)</td><td headers="h502">Sets or gets the current value for this sequence.</td>
<td headers="h501">[void setMaximum(Comparable)](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerNumberModel.html#setMaximum-java.lang.Comparable-)<br />[Comparable getMaximum()](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerNumberModel.html#getMaximum--)</td><td headers="h502">Sets or gets the upper bound for numbers in this sequence. If the maximum is `null`, there is no upper bound.</td>
<td headers="h501">[void setMinimum(Comparable)](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerNumberModel.html#setMinimum-java.lang.Comparable-)<br />[Comparable getMinimum()](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerNumberModel.html#getMinimum--)</td><td headers="h502">Sets or gets the lower bound for numbers in this sequence. If the minimum is `null`, there is no lower bound.</td>
<td headers="h501">[void setStepSize(Number)](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerNumberModel.html#setStepSize-java.lang.Number-)<br />[Number getStepSize()](https://docs.oracle.com/javase/8/docs/api/javax/swing/SpinnerNumberModel.html#getStepSize--)</td><td headers="h502">Sets or gets the increment used by `getNextValue` and `getPreviousValue` methods.</td>

## <a name="eg" id="eg">Examples That Use Spinners</a>

This table lists examples that use spinners and points to where those examples are described.
<th id="h601" align="left">Example</th><th id="h602" align="left">Where Described</th><th id="h603" align="left">Notes</th>
<td headers="h601">[`SpinnerDemo`](../examples/components/index.html#SpinnerDemo)</td><td headers="h602">This section</td><td headers="h603">Uses all three standard spinner model classes. Contains the code to use a custom spinner model, but the code is turned off by default.</td>
<td headers="h601">[`SpinnerDemo2`](../examples/components/index.html#SpinnerDemo2)</td><td headers="h602">This section</td><td headers="h603">A `SpinnerDemo` subclass that uses the custom spinner model for its Months spinner.</td>
<td headers="h601">[`SpinnerDemo3`](../examples/components/index.html#SpinnerDemo3)</td><td headers="h602">This section</td><td headers="h603">Based on SpinnerDemo, this application shows how to listen for changes in a spinner's value.</td>
<td headers="h601">[`SpinnerDemo4`](../examples/components/index.html#SpinnerDemo4)</td><td headers="h602">This section</td><td headers="h603">Implements a custom model and a custom editor for a spinner that displays shades of gray.</td>
