
# Nimbus Look and Feel

Nimbus is a polished cross-platform look and feel introduced in the Java SE 6 Update 10 (6u10) release. The following screen capture, from SwingSet3 shows the Nimbus look and feel.

Nimbus uses Java 2D vector graphics to draw the user interface (UI), rather than static bitmaps, so the UI can be crisply rendered at any resolution.

Nimbus is highly customizable. You can use the Nimbus look and feel as is, or you can **skin** (customize) the look with your own brand.

## <a name="enable" id="enable">Enabling the Nimbus Look and Feel</a>

For backwards compatibility, Metal is still the default Swing look and feel, but you can change to Nimbus in one of three ways:

<li>Add the following code to the event-dispatching thread before creating the graphical user interface (GUI):
<pre><code>
import javax.swing.UIManager.*;

try {
    for (LookAndFeelInfo info : UIManager.getInstalledLookAndFeels()) {
        if ("Nimbus".equals(info.getName())) {
            UIManager.setLookAndFeel(info.getClassName());
            break;
        }
    }
} catch (Exception e) {
    // If Nimbus is not available, you can set the GUI to another look and feel.
}
</code></pre>
The first line of code retrieves the list of all installed look and feel implementations for the platform and then iterates through the list to determine if Nimbus is available. If so, Nimbus is set as the look and feel.
<hr />**Version Note:**&#160;Do not set the Nimbus look and feel explicitly by invoking the `UIManager.setLookAndFeel` method because not all versions or implementations of Java SE 6 support Nimbus. Additionally, the location of the Nimbus package changed between the JDK 6 Update 10 and JDK 7 releases. Iterating through all installed look and feel implementations is a more robust approach because if Nimbus is not available, the default look and feel is used. For the JDK 6 Update 10 release, the Nimbus package is located at `com.sun.java.swing.plaf.nimbus.NimbusLookAndFeel`.
<hr />
</li>
<li>Specify Nimbus as the default look and feel for a particular application at the command line, as follows:
<pre><code>
java -Dswing.defaultlaf=javax.swing.plaf.nimbus.NimbusLookAndFeel *MyApp*
</code></pre>
</li>
<li>Permanently set the default look and feel to Nimbus by adding the following line to the `&lt;**JAVA_HOME**&gt;/lib/swing.properties` file:
<pre><code>
swing.defaultlaf=javax.swing.plaf.nimbus.NimbusLookAndFeel
</code></pre>
If the `swing.properties` file does not yet exist, you need to create it.</li>
