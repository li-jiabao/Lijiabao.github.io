
# Solving Common Event-Handling Problems

This section discusses problems that you might encounter while handling events.

**Problem:** I'm trying to handle certain events from a component, but the component isn't generating the events it should.

- First, make sure you registered the right kind of listener to detect the events. See whether another kind of listener might detect the kind of events you need.
- Make sure you registered the listener on the right object.
- Did you implement the event handler correctly? For example, if you extended an adapter class, then make sure you used the right method signature. Make sure each event-handling method is `public void`, that the name spelled right and that the argument is of the right type.

**Problem:** My combo box isn't generating low-level events such as focus events.

<li>Combo boxes are compound components &#226;&#128;&#148; components implemented using multiple components. For this reason, combo boxes don't fire the low-level events that simple components fire. For more information, see 
[Handling Events on a Combo Box](../components/combobox.html#listeners).</li>

**Problem:** The document for an editor pane (or text pane) isn't firing document events.

- The document instance for an editor pane or text pane might change when loading text from a URL. Thus, your listeners might be listening for events on an unused document. For example, if you load an editor pane or text pane with HTML that was previously loaded with plain text, the document will change to an `HTMLDocument` instance. If your program dynamically loads text into an editor pane or text pane, make sure the code adjusts for possible changes to the document (re-register document listeners on the new document, and so on).

If you don't see your problem in this list, see 
[Solving Common Component Problems](../components/problems.html).
