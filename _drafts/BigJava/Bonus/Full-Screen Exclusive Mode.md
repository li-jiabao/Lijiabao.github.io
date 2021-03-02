
# Display Mode

Once an application is in full-screen exclusive mode, it may be able to take advantage of actively setting the *display mode*. A display mode (<tt>java.awt.DisplayMode</tt>) is composed of the size (width and height of the monitor, in pixels), bit depth (number of bits per pixel), and refresh rate (how frequently the monitor updates itself). Some operating systems allow you to use multiple bit depths at the same time, in which case the special value <tt>BIT_DEPTH_MULTI</tt> is used for the value of bit depth. Also, some operating systems may not have any control over the refresh rate (or you may not care about the refresh rate setting). In this case, the special value <tt>REFRESH_RATE_UNKNOWN</tt> is used for the refresh rate value.

## How to Set the Display Mode

To get the current display mode, simply call the <tt>getDisplayMode</tt> method on your graphics device. To obtain a list of all possible display modes, call the <tt>getDisplayModes</tt> method. Both <tt>getDisplayMode</tt> and <tt>getDisplayModes</tt> can be called at any time, regardless of whether or not you are in full-screen exclusive mode.

Before attempting to change the display mode, you should first call the <tt>isDisplayChangeSupported</tt> method. If this method returns <tt>false</tt>, the operating system does not support changing the display mode.

Changing the display mode can only be done when in full-screen exclusive mode. To change the display mode, call the <tt>setDisplayMode</tt> method with the desired display mode. A runtime exception will be thrown if the display mode is not available, if display mode changes are not supported, or if you are not running in full-screen exclusive mode.

## Reasons for Changing the Display Mode

The main reason for setting the display mode is *performance*. An application can run much more quickly if the images it chooses to display share the same bit depth as the screen. Also, if you can count on the display to be a particular size, it makes drawing to that display much simpler, since you do not have to scale things down or up depending on how the user has set the display.

## Programming Tips

Here are some tips for choosing and setting the display mode:

- Check the value returned by the <tt>isDisplayChangeSupported</tt> method before attempting to change the display mode on a graphics device.
- Make sure you are in full-screen exclusive mode before attempting to change the display mode.
<li>As with using full-screen mode, setting the display mode is more robust when in a `try...finally` clause:
<pre><code>
GraphicsDevice myDevice;
Window myWindow;
DisplayMode newDisplayMode;
DisplayMode oldDisplayMode 
    = myDevice.getDisplayMode();

try {
    myDevice.setFullScreenWindow(myWindow);
    myDevice.setDisplayMode(newDisplayMode);
    ...
} finally {
    myDevice.setDisplayMode(oldDisplayMode);
    myDevice.setFullScreenWindow(null);
}
</code></pre>
</li>
- When choosing a display mode for your application, you may want to keep a list of preferred display modes, then choose the best one from the list of available display modes.
- As a fallback, if the display mode you desire is not available, you may want to simply run in windowed mode at a fixed size.
