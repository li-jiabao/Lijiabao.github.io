
# How to Support Assistive Technologies

You might be wondering what exactly assistive technologies are, and why you should care. Primarily, assistive technologies exist to enable people with permanent or temporary disabilities to use the computer. For example, if you get carpal tunnel syndrome, you can use assistive technologies to accomplish your work without using your hands.

Assistive technologies &#151; voice interfaces, screen readers, alternate input devices, and so on &#151; are useful not only for people with disabilities, but also for people using computers in non-office environments. For example, if you're stuck in a traffic jam, you might use assistive technologies to check your email, using only voice input and output. The information that enables assistive technologies can be used for other tools, as well, such as automated GUI testers and input devices such as touchscreens. Assistive technologies get information from components using the Accessibility API, which is defined in the 
[`javax.accessibility`](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/package-summary.html) package.


Because support for the Accessibility API is built into the Swing components, your Swing program will probably work just fine with assistive technologies, even if you do nothing special. For example, assistive technologies can automatically get the text information that is set by the following lines of code:

```

JButton button = new JButton("I'm a Swing button!");
label = new JLabel(labelPrefix + "0    ");
label.setText(labelPrefix + numClicks);
JFrame frame = new JFrame("SwingApplication");

```

Assistive technologies can also grab the tool-tip text (if any) associated with a component and use it to describe the component to the user. 


Making your program function smoothly with assistive technologies is easy to do and, in the United States, may be required by federal law.

The rest of this section covers these topics:

- [Rules for Supporting Accessibility](#supporting)
- [Testing for Accessibility](#testing)
- [Setting Accessible Names and Descriptions on Components](#namesdescriptions)
- [Concepts: How Accessibility Works](#developing)
- [Making Custom Components Accessible](#accessiblecomponents)
- [The Accessibility API](#api)
- [Examples that Use the Accessibility API](#eg)

## <a name="supporting" id="supporting">Rules for Supporting Accessibility</a>

Here are a few things you can do to make your program work as well as possible with assistive technologies:

- If a component doesn't display a short string (which serves as its default name), specify a name with the `setAccessibleName` method. You might want to do this for image-only buttons, panels that provide logical groupings, text areas, and so on.
<li>Set 
[tool tip](../components/tooltip.html) text for components whenever it makes sense to do so. For example:
<pre><code>
aJComponent.setToolTipText(
     "Clicking this component causes XYZ to happen.");
</code></pre>
</li>
<li>If you don't want to provide a tool tip for a component, use the `setAccessibleDescription` method to provide a description that assistive technologies can give the user. For example:
<pre><code>
aJComponent.getAccessibleContext().
    setAccessibleDescription(
    "Clicking this component causes XYZ to happen.");
</code></pre>
</li>
<li>Specify keyboard alternatives wherever possible. Make sure you can use your program with only the keyboard. Try hiding your mouse! Note that if the focus is in an editable text component, you can use Shift-Tab to move focus to the next component.
<p>Support for keyboard alternatives varies by component. 
[Buttons](../components/button.html) support keyboard alternatives with the `setMnemonic` method. Menus inherit the button mnemonic support and also support accelerators, as described in 
[Enabling Keyboard Operation](../components/menu.html#mnemonic). Other components can use 
[key bindings](../components/jcomponent.html#keyboardAction) to associate user typing with program actions.</p>
</li>
- Assign a textual description to all [`ImageIcon`](../components/icon.html) objects in your program. You can set this property by using either the `setDescription` method or one of the `String` forms of the `ImageIcon` constructors.
<li>If a bunch of components form a logical group, try to put them into one container. For example, use a 
[`JPanel`](../components/panel.html) to contain all the radio buttons in a radio button group.</li>
<li>Whenever you have a 
[label](../components/label.html) that describes another component, use the `setLabelFor` method so that assistive technologies can find the component that the label is associated with. This is especially important when the label displays a mnemonic for another component (such as a text field).</li>
- If you create a custom component, make sure it supports accessibility. In particular, be aware that subclasses of `JComponent` are not automatically accessible. Custom components that are descendants of other Swing components should override inherited accessibility information as necessary. For more information, see [Concepts: How Accessibility Works](#developing) and [Making Custom Components Accessible](#accessiblecomponents).
- Use the examples provided with the accessibility utilities to test your program. Although the primary purpose of these examples is to show programmers how to use the Accessibility API when implementing assistive technologies, these examples are also quite useful for testing application programs for accessibility. [Testing for Accessibility](#testing) shows `ScrollDemo` running with Monkey, one of the accessibility utilities examples. Monkey shows the tree of accessible components in a program and lets you interact with those components.
- Finally, don't break what you get for free! If your GUI has an inaccessible container &#8212; for example, your own subclass of `Container` or `JComponent` or any other container that doesn't implement the `Accessible` interface &#8212; any components inside that container become inaccessible.

## <a name="testing" id="testing">Testing for Accessibility</a>

The examples that come with the accessibility utilities can give you an idea of how accessible your program is. For instructions on getting these utilities, see the 
[Java SE Desktop Accessibility home page](http://www.oracle.com/technetwork/java/javase/tech/index-jsp-140174.html). Follow the instructions in the accessibility utilities documentation for setting up the Java Virtual Machine (VM) to run one or more of the utilities automatically.

Let's use an accessibility utility to compare the original version of one of our demos to a version in which the rules for supporting accessibility have been applied. Here's a picture of a program called `ScrollDemo`.

<li>
<p>Click the Launch button to run `ScrollDemo` using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Or, to compile and run the example yourself, consult the [example index](../examples/components/index.html#ScrollDemo).</p>
[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ScrollDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ScrollDemoProject/ScrollDemo.jnlp)<br /></li>
<li>Next, click the Launch button to run `AccessibleScrollDemo` using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Or, to compile and run the example yourself, consult the [example index](../examples/misc/index.html#AccessibleScrollDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the AccessibleScrollDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/AccessibleScrollDemoProject/AccessibleScrollDemo.jnlp)<br /></li>
<li>
Compare the two versions side by side. The only noticeable difference is that the **cm** toggle button and the photograph have tool tips in the accessible version.
</li>
<li>
Now run the two versions under the accessibility utility called Monkey. Note that when the accessibility tools have been downloaded and configured in the `accessibility.properties` file, the Monkey window automatically comes up when you click on the Run ScrollDemo and AccessibleScrollDemo links (in steps 1 and 2).
If the Monkey window does not appear on startup, the problem may be that the `accessibility.properties` file is not present in the version of the VM being used by Java Web Start. You can change the VM you use by running the Java Web Start [Application Manager](../../information/player.jnlp) and selecting **File &gt; Preferences &gt; Java**.
</li>
<li>
Note that when the Monkey window comes up you need to select **File &gt; Refresh Trees** to see information appear under `Accessible Tree`. You can then expand the tree by successively clicking on the horizontal icons displayed by each folder icon. When the tree has been expanded, you can see detailed information for the various components. The custom components (rules and corners) that weren't accessible in the original version are accessible in the modified version. This can make quite a difference to assistive technologies.
</li>

Here's a snapshot of Monkey running on `ScrollDemo`:

The left side of the split pane shows the actual component hierarchy for the program. The right side shows the accessible components in the hierarchy, which is what interests us.

The first thing to notice is that, even with no explicit support in `ScrollDemo`, Monkey is able to discover a lot of information about the various components in the program. Most of the components and their children appear in the tree. However, the names for most of the components are empty (null), which is rather unhelpful. The descriptions are also empty.

Further trouble comes with the program's custom components. The two rulers are inaccessible, so they are not included in the accessible tree. The viewports that contain the rulers are displayed as leaf nodes because they have no accessible children. The custom corners are also missing from the accessible tree.

Now here's a picture of the Monkey window for `AccessibleScrollDemo`:

<br />
The rules are now listed as children of the viewports, and the corners are listed as children of the scroll pane. Furthermore, many of the components now have non-null names.

In the previous snapshot of Monkey, the Column Header item is selected. Monkey highlights the corresponding component in `ScrollDemo` program.

When an item is selected, you can use Monkey's **Panels** menu to bring up one of four different panels that let you interact with the selected component. Choosing **Panels &gt; Accessibility API panel** brings up a panel like the one shown in the following figure. This panel displays information available through methods defined in the `AccessibleContext` base class and the `AccessibleComponent` interface.

Monkey has three other panels:

- **AccessibleAction**:  Shows the actions supported by an accessible component and lets you invoke the action. Works only with an accessible component whose context implements the `AccessibleAction` interface.
- **AccessibleSelection**:  Shows the current selection of an accessible component and lets you manipulate the selection. Works only with accessible component whose context implements the `AccessibleSelection` interface.
- **AccessibleHypertext**: Shows any hyperlinks contained within an accessible component and lets you traverse them. Works only with accessible component whose context implements the `AccessibleHypertext` interface.




The accessibility utilities examples are handy as testing tools and can give you an idea of how accessible the components in your program are. However, even if your components behave well in Monkey or the other examples, they still might not be completely accessible because Monkey and the other examples exercise only certain portions of the Accessibility API.

The only true test of accessibility is to run your programs with real-world assistive technologies, however, you may find the following free and open source screen reader useful: 
[NonVisual Desktop Access (NVDA)](http://www.nvda-project.org/).

## <a name="namesdescriptions" id="namesdescriptions">Setting Accessible Names and Descriptions on Components</a>

Giving your program's components accessible names and descriptions is one of the easiest and most important steps in making your program accessible. Following is a complete listing of the `AccessibleScrollDemo` constructor that creates the scroll pane and the custom components it uses. The boldface statements give components names and descriptions that assistive technologies can use.

```

public AccessibleScrollDemo() {
    // Get the image to use.
    <b>ImageIcon bee = createImageIcon("images/flyingBee.jpg",
                      "Photograph of a flying bee.");</b>

    // Create the row and column headers.
    columnView = new Rule(Rule.HORIZONTAL, true);
    if (bee != null) {
        columnView.setPreferredWidth(bee.getIconWidth());
    } else {
        columnView.setPreferredWidth(320);
    }
    **columnView.getAccessibleContext().setAccessibleName("Column Header");**
    <b>columnView.getAccessibleContext().
            setAccessibleDescription("Displays horizontal ruler for " +
                                     "measuring scroll pane client.");</b>
    rowView = new Rule(Rule.VERTICAL, true);
    if (bee != null) {
        rowView.setPreferredHeight(bee.getIconHeight());
    } else {
        rowView.setPreferredHeight(480);
    }
    **rowView.getAccessibleContext().setAccessibleName("Row Header");**
    <b>rowView.getAccessibleContext().
            setAccessibleDescription("Displays vertical ruler for " +
                                     "measuring scroll pane client.");</b>

    // Create the corners.
    JPanel buttonCorner = new JPanel();
    isMetric = new JToggleButton("cm", true);
    isMetric.setFont(new Font("SansSerif", Font.PLAIN, 11));
    isMetric.setMargin(new Insets(2,2,2,2));
    isMetric.addItemListener(this);
    <b>isMetric.setToolTipText("Toggles rulers' unit of measure " +
                            "between inches and centimeters.");</b>
    buttonCorner.add(isMetric); //Use the default FlowLayout
    <b>buttonCorner.getAccessibleContext().
                 setAccessibleName("Upper Left Corner");</b>

    <b>String desc = "Fills the corner of a scroll pane " +
                  "with color for aesthetic reasons.";</b>
    Corner lowerLeft = new Corner();
    <b>lowerLeft.getAccessibleContext().
              setAccessibleName("Lower Left Corner");</b>
    **lowerLeft.getAccessibleContext().setAccessibleDescription(desc);**

    Corner upperRight = new Corner();
    <b>upperRight.getAccessibleContext().
               setAccessibleName("Upper Right Corner");</b>
    **upperRight.getAccessibleContext().setAccessibleDescription(desc);**
    
    // Set up the scroll pane.
    picture = new ScrollablePicture(bee,
                                    columnView.getIncrement());
    **picture.setToolTipText(bee.getDescription());**
    <b>picture.getAccessibleContext().setAccessibleName(
                                     "Scroll pane client");</b>

    JScrollPane pictureScrollPane = new JScrollPane(picture);
    pictureScrollPane.setPreferredSize(new Dimension(300, 250));
    pictureScrollPane.setViewportBorder(
            BorderFactory.createLineBorder(Color.black));

    pictureScrollPane.setColumnHeaderView(columnView);
    pictureScrollPane.setRowHeaderView(rowView);

    // In theory, to support internationalization you would change
    // UPPER_LEFT_CORNER to UPPER_LEADING_CORNER,
    // LOWER_LEFT_CORNER to LOWER_LEADING_CORNER, and
    // UPPER_RIGHT_CORNER to UPPER_TRAILING_CORNER.  In practice,
    // bug #4467063 makes that impossible (at least in 1.4.0).
    pictureScrollPane.setCorner(JScrollPane.UPPER_LEFT_CORNER,
                                buttonCorner);
    pictureScrollPane.setCorner(JScrollPane.LOWER_LEFT_CORNER,
                                lowerLeft);
    pictureScrollPane.setCorner(JScrollPane.UPPER_RIGHT_CORNER,
                                upperRight);

    // Put it in this panel.
    setLayout(new BoxLayout(this, BoxLayout.X_AXIS));
    add(pictureScrollPane);
    setBorder(BorderFactory.createEmptyBorder(20,20,20,20));
}

```

Often, the program sets a component's name and description directly through the component's accessible context. Other times, the program sets an accessible description indirectly with tool tips. In the case of the **cm** toggle button, the description is set automatically to the text on the button. <a name="developing" id="developing"></a>

## <a name="developing__1" id="developing__1">Concepts: How Accessibility Works</a>

An object is accessible if it implements the 
[`Accessible`](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/Accessible.html) interface. The `Accessible` interface defines just one method, `getAccessibleContext`, which returns an 
[`AccessibleContext`](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleContext.html) object. The `AccessibleContext` object is an intermediary that contains the accessible information for an accessible object. The following figure shows how assistive technologies get the accessible context from an accessible object and query it for information:




`AccessibleContext` is an abstract class that defines the minimum set of information an accessible object must provide about itself. The minimum set includes name, description, role, state set, and so on. To identify its accessible object as having particular capabilities, an accessible context can implement one or more of the interfaces as shown in the [Accessible Interfaces](#interfaceList) table. For example, `JButton` implements `AccessibleAction`, `AccessibleValue`, `AccessibleText`, and `AccessibleExtendedComponent`. It is not necessary for `JButton` to implement `AccessibleIcon` because that is implemented by the `ImageIcon` attached to the button.

Because the `JComponent` class itself does not implement the `Accessible` interface, instances of its direct subclasses are not accessible. If you write a custom component that inherits directly from `JComponent`, you need to explicitly make it implement the `Accessible` interface. `JComponent` does have an accessible context, called `AccessibleJComponent`, that implements the `AccessibleComponent` interface and provides a minimal amount of accessible information. You can provide an accessible context for your custom components by creating a subclass of `AccessibleJComponent` and overriding important methods. [Making Custom Components Accessible](#accessiblecomponents) shows two examples of doing this.

All the other standard Swing components implement the `Accessible` interface and have an accessible context that implements one or more of the preceding interfaces as appropriate. The accessible contexts for Swing components are implemented as inner classes and have names of this style:

```

**Component**.Accessible**Component**

```

If you create a subclass of a standard Swing component and your subclass is substantially different from its superclass, then you should provide a custom accessible context for it. The easiest way is to create a subclass of the superclass's accessible context class and override methods as necessary. For example, if you create a `JLabel` subclass substantially different from `JLabel`, then your `JLabel` subclass should contain an inner class that extends `AccessibleJLabel`. The next section shows how to do so, using examples in which `JComponent` subclasses extend `AccessibleJComponent`. <a name="accessiblecomponents" id="accessiblecomponents"></a>

## <a name="accessiblecomponents__1" id="accessiblecomponents__1">Making Custom Components Accessible</a>

The scroll demo program uses three custom component classes. `ScrollablePicture` is a subclass of `JLabel`, and `Corner` and `Rule` are both subclasses of `JComponent`.

The `ScrollablePicture` class relies completely on accessibility inherited from `JLabel` through 
[`JLabel.AccessibleJLabel`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.AccessibleJLabel.html). The code that creates an instance of `ScrollablePicture` sets the tool-tip text for the scrollable picture. The tool-tip text is used by the context as the component's accessible description. This behavior is provided by `AccessibleJLabel`.

The accessible version of the `Corner` class contains just enough code to make its instances accessible. We implemented accessibility support by adding the code shown in bold to the original version of `Corner`.

```

public class Corner extends JComponent **implements Accessible** {

    protected void paintComponent(Graphics g) {
        //Fill me with dirty brown/orange.
        g.setColor(new Color(230, 163, 4));
        g.fillRect(0, 0, getWidth(), getHeight());
    }

    <strong>public AccessibleContext getAccessibleContext() {
        if (accessibleContext == null) {
            accessibleContext = new AccessibleCorner();
        }
        return accessibleContext;
    }

    protected class AccessibleCorner extends AccessibleJComponent {
        //Inherit everything, override nothing.
    }</strong>
}

```

All of the accessibility provided by this class is inherited from 
[`AccessibleJComponent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.AccessibleJComponent.html). This approach is fine for `Corner` because `AccessibleJComponent` provides a reasonable amount of default accessibility information and because corners are uninteresting: they exist only to take up a little bit of space onscreen. Other classes, such as `Rule`, need to provide customized information.

`Rule` provides an accessible context for itself in the same manner as `Corner`, but the context overrides two methods to provide details about the component's role and state:

```

protected class AccessibleRuler extends AccessibleJComponent {

    public AccessibleRole getAccessibleRole() {
        return AccessibleRuleRole.RULER;
    }

    public AccessibleStateSet getAccessibleStateSet() {
        AccessibleStateSet states =
            super.getAccessibleStateSet();
        if (orientation == VERTICAL) {
            states.add(AccessibleState.VERTICAL);
        } else {
            states.add(AccessibleState.HORIZONTAL);
        }
        if (isMetric) {
            states.add(AccessibleRulerState.CENTIMETERS);
        } else {
            states.add(AccessibleRulerState.INCHES);
        }
        return states;
    }
}

```


[`AccessibleRole`](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleRole.html) is an enumeration of objects that identify roles that Swing components can play. It contains predefined roles such as label, button, and so on. The rulers in our example don't fit well into any of the predefined roles, so the program invents a new one in a subclass of `AccessibleRole`:

```

class AccessibleRuleRole extends AccessibleRole {
    public static final AccessibleRuleRole RULER
        = new AccessibleRuleRole("ruler");

    protected AccessibleRuleRole(String key) {
        super(key);
    }

    //Should really provide localizable versions of these names.
    public String toDisplayString(String resourceBundleName,
                                  Locale locale) {
        return key;
    }
}

```

Any component that has state can provide state information to assistive technologies by overriding the `getAccessibleStateSet` method. A rule has two sets of states: its orientation can be either vertical or horizontal, and its units of measure can be either centimeters or inches. 
[`AccessibleState`](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleState.html) is an enumeration of predefined states. This program uses its predefined states for vertical and horizontal orientation. Because `AccessibleState` contains nothing for centimeters and inches, the program makes a subclass to provide appropriate states:

```

class AccessibleRulerState extends AccessibleState {
    public static final AccessibleRulerState INCHES
        = new AccessibleRulerState("inches");
    public static final AccessibleRulerState CENTIMETERS
        = new AccessibleRulerState("centimeters");

    protected AccessibleRulerState(String key) {
        super(key);
    }

    //Should really provide localizable versions of these names.
    public String toDisplayString(String resourceBundleName,
                                  Locale locale) {
        return key;
    }
}

```

You've seen how to implement accessibility for two simple components, that exist only to paint themselves onscreen. Components that do more, such as responding to mouse or keyboard events, need to provide more elaborate accessible contexts. You can find examples of implementing accessible contexts by delving in the source code for the Swing components.

## <a name="api" id="api">The Accessibility API</a>

The tables in this section cover just part of the accessibility API. For more information about the accessibility API, see the API documentation for the classes and packages in the 
[accessibility package](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/package-summary.html). Also, refer to the API documentation for the accessible contexts for individual Swing components.

The API for supporting accessibility falls into the following categories:

- [Naming and Linking Components](#settingnamesapi)
- [Making a Custom Component Accessible](#accessibleapi)
- [Accessible Interfaces](#interfaceList)
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[getAccessibleContext().setAccessibleName(String)](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleContext.html#setAccessibleName-java.lang.String-)<br />[getAccessibleContext().setAccessibleDescription(String)](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleContext.html#setAccessibleDescription-java.lang.String-)<br />(**on a `JComponent` or `Accessible` object**)</td><td headers="h2">Provide a name or description for an accessible object.</td>
<td headers="h1">[void setToolTipText(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setToolTipText-java.lang.String-)<br />(**in `JComponent`**)</td><td headers="h2">Set a component's tool tip. If you don't set the description, than many accessible contexts use the tool-tip text as the accessible description.</td>
<td headers="h1">[void setLabelFor(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setLabelFor-java.awt.Component-)<br />(**in `JLabel`**)</td><td headers="h2">Associate a label with a component. This tells assistive technologies that a label describes another component.</td>
<td headers="h1">[void setDescription(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/ImageIcon.html#setDescription-java.lang.String-)<br />(**in `ImageIcon`**)</td><td headers="h2">Provide a description for an image icon.</td>

<br />
<th id="h101" align="left">Interface or Class</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[Accessible](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/Accessible.html)<br />(**an interface**)</td><td headers="h102">Components that implement this interface are accessible. Subclasses of `JComponent` must implement this explicitly.</td>
<td headers="h101">[AccessibleContext](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleContext.html)<br />[**JComponent**.Accessible**JComponent**](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.AccessibleJComponent.html)<br />(**an abstract class and its subclasses**)</td><td headers="h102">`AccessibleContext` defines the minimal set of information required of accessible objects. The accessible context for each Swing component is a subclass of this and named as shown. For example, the accessible context for `JTree` is `JTree.AccessibleJTree`. To provide custom accessible contexts, custom components should contain an inner class that is a subclass of `AccessibleContext`. For more information, see [Making Custom Components Accessible](#accessiblecomponents).</td>
<td headers="h101">[AccessibleRole](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleRole.html)<br />[AccessibleStateSet](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleStateSet.html)<br />(**classes**)</td><td headers="h102">Define the objects returned by an `AccessibleContext` object's `getAccessibleRole` and `getAccessibleStateSet` methods, respectively.</td>
<td headers="h101">[AccessibleRelation](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleRelation.html)<br />[AccessibleRelationSet](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleRelationSet.html)<br /></td><td headers="h102">Define the relations between components that implement this interface and one or more other objects. </td>

<br />
<th id="h201" align="left">Interface</th><th id="h202" align="left">Purpose</th>
<td headers="h201">[AccessibleAction](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleAction.html)</td><td headers="h202">Indicates that the object can perform actions. By implementing this interface, the accessible context can give information about what actions the accessible object can perform and can tell the accessible object to perform them.</td>
<td headers="h201">[AccessibleComponent](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleComponent.html)</td><td headers="h202">Indicates that the accessible object has an onscreen presence. Through this interface, an accessible object can provide information about its size, position, visibility and so on. The accessible contexts for all standard Swing components implement this interface, directly or indirectly. The accessible contexts for your custom components should do the same. The `AccessibleExtendedComponent` method is preferred.</td>
<td headers="h201">[AccessibleEditableText](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleEditableText.html)<br /></td><td headers="h202">Indicates that the accessible object displays editable text. In addition to the information available from its superinterface, `AccessibleText`, methods are provided for cutting, pasting, deleting, selecting, and inserting text.</td>
<td headers="h201">[AccessibleExtendedComponent](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleExtendedComponent.html)<br /></td><td headers="h202">In addition to the information available from its superinterface, `AccessibleComponent`, methods are provided for obtaining key bindings, border text, and tool-tip text.</td>
<td headers="h201">[AccessibleExtendedTable](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleExtendedTable.html)<br /></td><td headers="h202">In addition to the information available from its superinterface, `AccessibleTable`, methods are provided to convert between an index and its row or column.</td>
<td headers="h201">[AccessibleHypertext](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleHypertext.html)</td><td headers="h202">Indicates that the accessible object contains hyperlinks. Through this interface, an accessible object can provide information about its links and allow them to be traversed.</td>
<td headers="h201">[AccessibleIcon](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleIcon.html)</td><td headers="h202">Indicates that the accessible object has an associated icon. Methods are provided that return information about the icon, such as size and description.</td>
<td headers="h201">[AccessibleKeyBinding](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleKeyBinding.html)<br /></td><td headers="h202">Indicates that the accessible object supports one or more keyboard shortcuts that can be used to select the object. Methods are provided that return the key bindings for a given object.</td>
<td headers="h201">[AccessibleSelection](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleSelection.html)</td><td headers="h202">Indicates that the accessible object can contain a selection. Accessible contexts that implement this interface can report information about the current selection and can modify the selection.</td>
<td headers="h201">[AccessibleTable](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleTable.html)</td><td headers="h202">Indicates that the accessible object presents data in a two-dimensional data object. Through this interface information about the table such as table caption, row and column size, description, and name are provided. The `AccessibleExtendedTable` method is preferred.</td>
<td headers="h201">[AccessibleText](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleText.html)</td><td headers="h202">Indicates that the accessible object displays text. This interface provides methods for returning all or part of the text, attributes applied to it, and other information about the text such as its length.</td>
<td headers="h201">[AccessibleValue](https://docs.oracle.com/javase/8/docs/api/javax/accessibility/AccessibleValue.html)</td><td headers="h202">Indicates that the object has a numeric value. Through this interface an accessible object provides information about its current value and its minimum and maximum values.</td>

## <a name="eg" id="eg">Examples that Use the Accessibility API</a>

The following table lists some of our examples that have good support for assistive technologies.<br />

<th id="h301" align="left">Example</th><th id="h302" align="left">Where Described</th><th id="h303" align="left">Notes</th>
<td headers="h301">[`AccessibleScrollDemo`](../examples/misc/index.html#AccessibleScrollDemo)</td><td headers="h302">This section</td><td headers="h303">Contains two custom components that implement the `Accessible` interface. To see a less accessible version of this program see [How to Use Scroll Panes](../components/scrollpane.html).</td>
<td headers="h301">[`ButtonDemo`](../examples/components/index.html#ButtonDemo)</td><td headers="h302">[How to Use the Common Button API](../components/button.html#abstractbutton)</td><td headers="h303">Uses three buttons. Supports accessibility through button text, mnemonics, and tool tips.</td>
