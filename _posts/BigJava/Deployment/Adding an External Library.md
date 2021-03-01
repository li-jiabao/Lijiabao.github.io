
# Adding an External Library

Self-contained applications include everything that an application needs to run. If your application requires an external library, that library can be added to the bundle for the application. Adding the library can be done in different ways.

The File Association Demo described in 
[Using File Associations](../selfContainedApps/fileassociation.html) downloads the Groovy library as part of the build process. The library is placed in the `/lib` directory in the project for the application. This directory is then copied to the `/dist` directory from which the self-contained application bundle is generated. 

The following code in the `-pre-init` task in the `build.xml` file shows how the library is downloaded:

```

&lt;!-- download and copy groovy library --&gt;
&lt;copy toFile="lib/groovy-all-2.3.8.jar"&gt;
    &lt;resources&gt;
      &lt;url url="http://central.maven.org/maven2/org/codehaus/groovy/groovy-all/2.3.8/groovy-all-2.3.8.jar"/&gt;
    &lt;/resources&gt;
&lt;/copy&gt;

```

See 
[`build.xml`](examples/packager_FileAssociations/build.xml) for the complete build code.

You can download the source files for the File Association Demo from 
[Self-Contained Application Examples](../selfContainedApps/examplesIndex.html).
