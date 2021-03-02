
# Full-Screen Exclusive Mode

Programmers who use Microsoft's DirectX API may already be familiar with full-screen exclusive mode. Other programmers may be somewhat new to the concept. In either case, full-screen exclusive mode is a powerful feature of J2SE&#8482; version 1.4 that allows the programmer to suspend the windowing system so that drawing can be done directly to the screen.

This is a slight paradigm shift from the usual kind of GUI program in many ways. In traditional Java GUI programs, the AWT is responsible for propagating *paint events* from the operating system, through the *event dispatch thread*, and by calling AWT's <tt>Component.paint</tt> method when appropriate. In full-screen exclusive applications, painting is usually done actively by the program itself. Additionally, a traditional GUI application is limited to the bit depth and size of the screen chosen by the user. In a full-screen exclusive application, the program can control the bit depth and size (*display mode*) of the screen. Finally, many more advanced techniques, such as *page flipping* (discussed below) and stereo buffering (utilizing systems which use a separate set of frames for each eye) require, on some platforms, that an application first be in full-screen exclusive mode.

## Hardware-Accelerated Image Basics

To understand the full-screen exclusive mode API, you need to understand some basic principles of hardware-accelerated images. The <tt>VolatileImage</tt> interface encapsulates a surface which may or may not take advantage of hardware acceleration. Such surfaces may lose their hardware acceleration or their memory at the behest of the operating system (hence, the name 'volatile'). See the `VolatileImage` **Tutorial** (coming soon) for more information on volatile images.

Full-screen exclusive mode is handled through a <tt>java.awt.GraphicsDevice</tt> object. For a list of all available screen graphics devices (in single or multi-monitor systems), you can call the method <tt>getScreenDevices</tt> on the local <tt>java.awt.GraphicsEnvironment</tt>; for the default (primary) screen (the only screen on a single-monitor system), you can call the method <tt>getDefaultScreenDevice</tt>.

Once you have the graphics device, you can call one of the following methods:

<li><tt>public boolean isFullScreenSupported()</tt><br />
This method returns whether or not full-screen exclusive mode is available. On systems where full-screen exclusive mode is not available, it is probably better to run an application in windowed mode with a fixed size rather than setting a full-screen window.
</li>
<li><tt>public void setFullScreenWindow(Window w)</tt><br />
Given a window, this method enters full-screen exclusive mode using that window. If full-screen exclusive mode is not available, the window is positioned at (0,0) and resized to fit the screen. Use this method with a <tt>null</tt> parameter to exit full-screen exclusive mode.
</li>

## Programming Tips

Here are some tips about programming using full-screen exclusive mode:

- Check for <tt>isFullScreenSupported</tt> before entering full-screen exclusive mode. If it isn't supported, performance may be degraded.
<li>Entering and exiting full-screen mode is more robust when using a <tt>try...finally</tt> clause. This is not only good coding practice, but it also prevents your program from staying in full-screen exclusive mode longer than it should:
<pre><code>
GraphicsDevice myDevice;
Window myWindow;

try {
    myDevice.setFullScreenWindow(myWindow);
    ...
} finally {
    myDevice.setFullScreenWindow(null);
}
</code></pre>
</li>
- Most full-screen exclusive applications are better suited to use undecorated windows. Turn off decorations in a frame or dialog using the <tt>setUndecorated</tt> method.
- Full-screen exclusive applications should not be resizable, since resizing a full-screen application can cause unpredictable (or possibly dangerous) behavior.
- For security reasons, the user must grant <tt>fullScreenExclusive</tt> permission when using full-screen exclusive mode in an applet.
