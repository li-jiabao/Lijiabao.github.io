
# About the JFC and Swing

JFC is short for Java Foundation Classes, which encompass a group of features for building graphical user interfaces (GUIs) and adding rich graphics functionality and interactivity to Java applications. It is defined as containing the features shown in the table below.
<th id="h1">Feature</th><th id="h2">Description</th>
<td headers="h1">Swing GUI Components</td><td headers="h2">Includes everything from buttons to split panes to tables. Many components are capable of sorting, printing, and drag and drop, to name a few of the supported features.</td>
<td headers="h1">Pluggable Look-and-Feel Support</td><td headers="h2">The look and feel of Swing applications is pluggable, allowing a choice of look and feel. For example, the same program can use either the Java or the Windows look and feel. Additionally, the Java platform supports the GTK+ look and feel, which makes hundreds of existing look and feels available to Swing programs. Many more look-and-feel packages are available from various sources.</td>
<td headers="h1">Accessibility API</td><td headers="h2">Enables assistive technologies, such as screen readers and Braille displays, to get information from the user interface.</td>
<td headers="h1">Java 2D API</td><td headers="h2">Enables developers to easily incorporate high-quality 2D graphics, text, and images in applications and applets. Java 2D includes extensive APIs for generating and sending high-quality output to printing devices.</td>
<td headers="h1">Internationalization</td><td headers="h2">Allows developers to build applications that can interact with users worldwide in their own languages and cultural conventions. With the input method framework developers can build applications that accept text in languages that use thousands of different characters, such as Japanese, Chinese, or Korean.</td>

This trail concentrates on the Swing components. We help you choose the appropriate components for your GUI, tell you how to use them, and give you the background information you need to use them effectively. We also discuss other features as they apply to Swing components.

## Which Swing Packages Should I Use?

The Swing API is powerful, flexible &#8212; and immense. The Swing API has 18 public packages:
<td width="172">`javax.accessibility`</td><td width="150">`javax.swing.plaf`</td><td width="157">`javax.swing.text`</td>
<td width="172">`javax.swing`</td><td width="150">`javax.swing.plaf.basic`</td><td width="157">`javax.swing.text.html`</td>
<td width="172">`javax.swing.border`</td><td width="150">`javax.swing.plaf.metal`</td><td width="157">`javax.swing.text.html.parser`</td>
<td width="172">`javax.swing.colorchooser`</td><td width="150">`javax.swing.plaf.multi`</td><td width="157">`javax.swing.text.rtf`</td>
<td width="172">`javax.swing.event`</td><td width="150">`javax.swing.plaf.synth`</td><td width="157">`javax.swing.tree`</td>
<td width="172">`javax.swing.filechooser`</td><td width="150">`javax.swing.table`</td><td width="157">`javax.swing.undo`</td>

Fortunately, most programs use only a small subset of the API. This trail sorts out the API for you, giving you examples of common code and pointing you to methods and classes you're likely to need. Most of the code in this trail uses only one or two Swing packages:

- `javax.swing`
- `javax.swing.event` (not always required)
