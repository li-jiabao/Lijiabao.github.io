
# Using File Associations

One of the advantages of providing a self-contained application to your users is the ability to set up file associations. Specific types of files can be associated with your application based on MIME type or file extension so that your application is used to open an associated file. For example, if your application edits text files, you can set up a file association that runs your application when a user double-clicks on a file with the extension `.txt`.

The File Association Demo reads JavaScript and Groovy code. Using both MIME types and file extensions, the application is associated with JavaScript and Groovy files. 

You can download the source files for the File Association Demo from 
[Self-Contained Application Examples](../selfContainedApps/examplesIndex.html).

## Setting Up File Associations

The Ant tasks for generating the self-contained application bundles are in the `build.xml` file for the File Association Demo. The `&lt;fx:association&gt;` Ant element is used to associate file extensions or MIME types with your application. Linux bundlers require the MIME type, Windows bundlers require the file extension, and OS X bundlers require at least one of the properties. It is a good practice to use both properties with a one-to-one mapping between the MIME type and file extension, which enables you to use the same build file on multiple platforms. See 
[&lt;fx:association&gt;](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/javafx_ant_task_reference.html#JSDPG997) for more information about this element.

The following code shows what needs to be included in the `fx:deploy` element to associate the application with the extensions `.js` and `.groovy` and the MIME types `text/javascript` and `text/x-groovy`.

```

&lt;fx:info title="File Association Demo"
         vendor="MySamples"
         description="A Demo of File Associations for Java Packager"
         category="Demos"
         license="3 Clause BSD"&gt;
    &lt;fx:association extension="js" mimetype="text/javascript" description="JavaScript Source"/&gt;
    &lt;fx:association extension="groovy" mimetype="text/x-groovy" description="Groovy Source"/&gt;
&lt;/fx:info&gt;

```

If a bundler does not support file associations, the associations are ignored. As of the 8u40 release of the JDK, the Windows EXE and MSI bundlers, Linux DEB and RPM bundlers, and the Mac .app bundler support file associations. The OS X PKG and DMG bundlers support file associations through their use of the Mac .app bundler.

See 
[`build.xml`](examples/packager_FileAssociations/build.xml) for the complete build code.

To generate the installable bundles for the File Association Demo, see the "Generating the Bundles" section in 
[Converting an Existing Application](../selfContainedApps/converting.html).

## Launching from Associated Files

File associations are set up by the installer when the self-contained application bundle is installed on a user's system. After the application is installed, opening a file that is associated with your application causes your application to be started. The actions taken to launch your application depend on the platform on which it is running.

### Launching on Linux and Windows

On Linux and Windows, when an application is launched based on a file association, the files being opened are passed as arguments to the main class, which overrides the default argument for the class. For the File Associations Demo, the arguments are passed to the `loadscript` method after an instance of the application is started. A different instance of the application is started for each file opened.

See 
[`ScriptRunnerApplication.java`](examples/packager_FileAssociations/src/sample/fa/ScriptRunnerApplication.java) for the Linux and Windows version of the code.

### Launching on OS X

On OS X, only a single instance of an application is run. When an associated file is opened, an event is sent to the application. The application must have an event listener registered to handle the event.

The File Association Demo for OS X has a subclass with a different main method than the version for Linux and Windows. This main method handles the default argument in the same way as the main method for the Linux and Windows version, and then registers a listener with OS X for `FileOpenHandler`. The event method for this listener is called when an associated file is opened, and the file name is extracted from the `getFiles` method of the `OpenFilesEvent` object.

See 
[`ScriptRunnerApplicationMac.java`](examples/packager_FileAssociations/src/sample/fa/ScriptRunnerApplicationMac.java) for the OS X version of the code.

Building the OS X version of the File Association Demo requires access to the OS X-specific classes that come with the Oracle JDK. Most `com.apple.eawt` classes are not included in the symbols file that the `javac` compiler uses. To tell the compiler to ignore the symbols file, the `-XDignore.symbol.file=true` argument is passed to the `javac` compiler in the `-pre-init` Ant task in the build file. See 
[`build.xml`](examples/packager_FileAssociations/build.xml).

## More About the File Association Demo

The project for the File Association Demo contains the Java source files for the application in the `/src/sample/fa` directory. Custom icons are provided in the `/src/package/<var>platform</var>` directory. Sample files to be packaged with the application are in the `/src` directory.

To handle Groovy code, the File Association Demo requires the Groovy library. The build process downloads the Groovy library to the `/lib` directory. See 
[Adding an External Library](../selfContainedApps/addlibrary.html) for information.

After the JAR file is generated, the build process copies the `/src` and `/lib` directories to the `/dist` directory. The `/dist` directory then contains all of the files for your application.

The File Association Demo takes a file name as an argument. If the application is started by opening an associated file, the name of the associated file is passed in. If the application is started directly, the sample file  `sample.js`, which is bundled with the application, is passed in. See 
[Providing a Default Argument](../selfContainedApps/defaultarg.html) for information.

Admin privileges are required to set up file associations. By default the EXE installer for Windows does not request admin privileges. To force a request for admin privileges for the File Association Demo, the bundle argument `win.exe.systemWide` is set to `true`. This setting indicates that a system-wide installation is performed, which requires admin privileges. 

The File Association Demo runs on Linux, OS X, and Windows. The demo is set up to use a single build file that contains the information for all platforms. See 
[Using a Common Build File for All Platforms](../selfContainedApps/commonbuild.html) for information.

## Additional Resources

For more information about file associations, see 
[Associating Files with a Self-Contained Application](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/self-contained-packaging.html#JSDPG996).

For more information about JavaFX Ant arguments, see 
[JavaFX Ant Task Reference](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/javafx_ant_task_reference.html).
