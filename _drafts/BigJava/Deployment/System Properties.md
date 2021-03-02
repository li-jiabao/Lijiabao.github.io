
# System Properties

This topic lists system properties that can be accessed by rich Internet applications (RIAs) that are restricted to the security sandbox and are launched with or without the Java Network Launch Protocol (JNLP). Some system properties cannot be accessed by sandbox RIAs.

## Secure System Properties Accessible by All RIAs

All RIAs can retrieve the following secure system properties:

- `<code>java.class.version`</code>
- `<code>java.vendor`</code>
- `<code>java.vendor.url`</code>
- `<code>java.version`</code>
- `<code>os.name`</code>
- `<code>os.arch`</code>
- `<code>os.version`</code>
- `<code>file.separator`</code>
- `<code>path.separator`</code>
- `<code>line.separator`</code>

## Secure System Properties Accessible by RIAs Launched by Using JNLP

RIAs launched by using JNLP can set and retrieve the following secure properties:

- `awt.useSystemAAFontSettings`
- `http.agent`
- `http.keepAlive`
- `java.awt.syncLWRequests`
- `java.awt.Window.locationByPlatform`
- `javaws.cfg.jauthenticator`
- `javax.swing.defaultlf`
- `sun.awt.noerasebackground`
- `sun.awt.erasebackgroundonresize`
- `sun.java2d.d3d`
- `sun.java2d.dpiaware`
- `sun.java2d.noddraw`
- `sun.java2d.opengl`
- `swing.boldMetal`
- `swing.metalTheme`
- `swing.noxp`
- `swing.useSystemFontSettings`

## Forbidden System Properties

Sandbox RIAs cannot access the following system properties:

- `java.class.path`
- `java.home`
- `user.dir`
- `user.home`
- `user.name`
