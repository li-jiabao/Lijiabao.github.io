
# Passive vs. Active Rendering

As mentioned before, most full-screen applications usually function better if they are at the helm during drawing. In traditional windowed GUI applications, the question of when to paint is usually handled by the operating system. When operating in a windowed environment, this makes perfect sense. A windowed application does not know when the user is going to move, resize, expose, or cover an application by another window until it actually happens. In a Java GUI application, the operating system delivers a *paint event* to the AWT, which figures out what needs to be painted, creates a <tt>java.awt.Graphics</tt> object with the appropriate clipping region, then calls the <tt>paint</tt> method with that <tt>Graphics</tt> object:

```

// Traditional GUI Application paint method:
// This can be called at any time, usually 
// from the event dispatch thread
public void paint(Graphics g) {
    // Use g to draw my Component
}

```

This is sometimes referred to as *passive rendering*. As you can imagine, such a system incurs a lot of overhead, much to the annoyance of many performance-sensitive AWT and Swing programmers.

When in full-screen exclusive mode, you don't have to worry anymore about the window being resized, moved, exposed, or occluded (unless you've ignored my suggestion to turn off resizing). Instead, the application window is drawn directly to the screen (*active rendering*). This simplifies painting quite a bit, since you don't ever need to worry about paint events. In fact, paint events delivered by the operating system may even be delivered at inappropriate or unpredictable times when in full-screen exclusive mode.

Instead of relying on the <tt>paint</tt> method in full-screen exclusive mode, drawing code is usually more appropriately done in a *rendering loop*:

```

public void myRenderingLoop() {
    while (!done) {
        Graphics myGraphics = getPaintGraphics();
        // Draw as appropriate using myGraphics
        myGraphics.dispose();
    }
}

```

Such a rendering loop can done from any thread, either its own helper thread or as part of the main application thread.

## Programming Tips

Some tips about using active rendering:

- Don't put drawing code in the <tt>paint</tt> routine. You may never know when that routine may get called! Instead, use another method name, such as <tt>render(Graphics g)</tt>, which can be called from the <tt>paint</tt> method when operating in windowed mode, or alternately called with its own graphics from the rendering loop.
- Use the <tt>setIgnoreRepaint</tt> method on your application window and components to turn off all paint events dispatched from the operating system completely, since these may be called during inappropriate times, or worse, end up calling <tt>paint</tt>, which can lead to race conditions between the AWT event thread and your rendering loop.
- Separate your drawing code from your rendering loop, so that you can operate fully under both full-screen exclusive and windowed modes.
- Optimize your rendering so that you aren't drawing everything on the screen at all times (unless you are using page-flipping or double-buffering, both discussed below).
- Do not rely on the <tt>update</tt> or <tt>repaint</tt> methods for delivering paint events.
- Do not use heavyweight components, since these will still incur the overhead of involving the AWT and the platform's windowing system.
- If you use lightweight components, such as Swing components, you may have to fiddle with them a bit so that they draw using your <tt>Graphics</tt>, and not directly as a result of calling the <tt>paint</tt> method. Feel free to call Swing methods such as <tt>paintComponents</tt>, <tt>paintComponent</tt>, <tt>paintBorder</tt>, and <tt>paintChildren</tt> directly from your rendering loop.
- Feel free to use passive rendering if you just want a simple full-screen Swing or AWT application, but remember that paint events may be somewhat unreliable or unnecessary while in full-screen exclusive mode. Additionally, if you use passive rendering, you will not be able to use more advanced techniques such as page-flipping. Finally, be very careful to avoid deadlocks if you decide to use both active and passive rendering simultaneously--this approach is not recommended.
