
# BufferStrategy and BufferCapabilities

## `BufferStrategy`

In Java 2 Standard Edition, you don't have to worry about video pointers or video memory in order to take full advantage of either double-buffering or page-flipping. The new class <tt>java.awt.image.BufferStrategy</tt> has been added for the convenience of dealing with drawing to surfaces and components in a general way, regardless of the number of buffers used or the technique used to display them.

A buffer strategy gives you two all-purpose methods for drawing: <tt>getDrawGraphics</tt> and <tt>show</tt>. When you want to start drawing, get a draw graphics and use it. When you are finished drawing and want to present your information to the screen, call <tt>show</tt>. These two methods are designed to fit rather gracefully into a rendering loop:

```

BufferStrategy myStrategy;

while (!done) {
    Graphics g = myStrategy.getDrawGraphics();
    render(g);
    g.dispose();
    myStrategy.show();
}

```

Buffer strategies have also been set up to help you monitor <tt>VolatileImage</tt> issues. When in full-screen exclusive mode, <tt>VolatileImage</tt> issues are especially important because the windowing system can sometimes take back the video memory it has given you. One important example is when the user presses the <tt>ALT+TAB</tt> key combination in Windows--suddenly your full-screen program is running in the background and your video memory is lost. You can call the <tt>contentsLost</tt> method to find out if this has happened. Similarly, when the windowing system returns your memory to you, you can find out using the <tt>contentsRestored</tt> method.

## `BufferCapabilities`

As mentioned before, different operating systems, or even different graphics cards on the same operating system, have different techniques available at their disposal. These *capabilities* are exposed for you so that you can pick the best technique for your application.

The class <tt>java.awt.BufferCapabilities</tt> encapsulates these capabilities. Every buffer strategy is controlled by its buffer capabilities, so picking the right ones for your application is very crucial. To find out what capabilities are available, call the <tt>getBufferCapabilities</tt> method from the <tt>GraphicsConfiguration</tt> objects available on your graphics device.

The capabilities available in Java 2 Standard Edition version 1.4 are:

<li>`isPageFlipping`<br />
This capability returns whether or not hardware page-flipping is available on this graphics configuration.
</li>
<li>`isFullScreenRequired`<br />
This capability returns whether or not full-screen exclusive mode is required before hardware page-flipping should be attempted.
</li>
<li>`isMultiBufferAvailable`<br />
This capability returns whether or not multiple buffering (two or more back buffers plus the primary surface) in hardware is available.
</li>
<li>`getFlipContents`<br />
This capability returns a hint of the technique used to do hardware page-flipping. This is important because the contents of the back buffer after a <tt>show</tt> are different depending on the technique used. The value returned can be null (if <tt>isPageFlipping</tt> returns <tt>false</tt>) or one of the following values. Any value can be specified for a buffer strategy so long as the <tt>isPageFlipping</tt> method returns true, though performance will vary depending on the available capabilities.
</li>
<li>`FlipContents.COPIED`<br />
This value means that the contents of the back buffer are copied to the primary surface. A "flip" is probably performed as a hardware blt, which means that hardware double-buffering is probably done using blitting instead of true page-flipping. This should (in theory) be faster, or at least as fast, as blitting from a <tt>VolatileImage</tt> to the primary surface, though your mileage may vary. The contents of the back buffer are the same as the primary surface after a flip.
</li>
<li>`FlipContents.BACKGROUND`<br />
This value means that the contents of the back buffer have been cleared with the background color. Either a true page-flip or a blt has occurred.
</li>
<li>`FlipContents.PRIOR`<br />
This value means that the contents of the back buffer are now the contents of the old primary surface, and vice versa. Generally this value indicates that true page-flipping occurs, though this is not guaranteed and, once again, your mileage on this operation may vary.
</li>
<li>`FlipContents.UNKNOWN`<br />
This value means that the contents of the back buffer are undefined after a flip. You may have to experiment to find which technique works best for you (or you may not care), and you will definitely have to set up the contents of the back buffer yourself each time you draw.
</li>

To create a buffer strategy for a component, call the <tt>createBufferStrategy</tt> method, supplying the number of buffers desired (this number includes the primary surface).&#160; If any particular buffering technique is desired, supply an appropriate <tt>BufferCapabilities</tt> object. Note that when you use this version of the method, you must catch an <tt>AWTException</tt> in the event that your choice is not available. Also note that these methods are only available on <tt>Canvas</tt> and <tt>Window</tt>.

Once a particular buffer strategy has been created for a component, you can manipulate it using the <tt>getBufferStrategy</tt> method. Note that this method is also only available for canvases and windows.

## Programming Tips

Some tips about using buffer capabilities and buffer strategies:

<li>Getting, using, and disposing a graphics object are more robust in a <tt>try...finally</tt> clause:
<pre><code>
BufferStrategy myStrategy;

while (!done) {
    Graphics g;
    try {
        g = myStrategy.getDrawGraphics();
        render(g);
    } finally {
        g.dispose();
    }
    myStrategy.show();
}
</code></pre>
</li>
- Check the available capabilities before using a buffer strategy.
- For best results, create your buffer strategy on a full-screen exclusive window. Make sure you check the <tt>isFullScreenRequired</tt> and <tt>isPageFlipping</tt> capabilities before using page-flipping.
- Don't make any assumptions about performance. Tweak your code as necessary, but remember that different operating systems and graphics cards have different capabilities. Profile your application!
- You may want to subclass your component to override the <tt>createBufferStrategy</tt> method. Use an algorithm for choosing a strategy that is best suited to your application. The <tt>FlipBufferStrategy</tt> and&#160; <tt>BltBufferStrategy</tt> inner classes are protected and can be subclassed.
- Don't forget that you may lose your drawing surfaces!&#160; Be sure to check <tt>contentsLost</tt> and <tt>contentsRestored</tt> before drawing. All buffers that have been lost have to be redrawn when they are restored.
- If you use a buffer strategy for double-buffering in a Swing application, you probably want to turn off double-buffering for your Swing components, since they will already be double-buffered. Video memory is somewhat valuable and should only be used whenever absolutely necessary.
- It may be end up being wasteful to use more than one back buffer. Multi-buffering is only useful when the drawing time exceeds the time spent to do a <tt>show</tt>. Profile your application!
