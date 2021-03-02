
# Setting up the CelsiusConverter Project

If you have worked with the NetBeans IDE in the past, much of this section will look familiar, since the initial steps are similar for most projects. Still, the following steps describe settings that are specific to this application, so take care to follow them closely. <a name="a3" id="a3"></a>

## Step 1: Create a New Project

To create a new project, launch the NetBeans IDE and choose New Project from the File menu:

Creating a New Project

Keyboard shortcuts for each command appear on the far right of each menu item. The look and feel of the NetBeans IDE may vary across platforms, but the functionality will remain the same. <a name="a4" id="a4"></a>

## Step 2: Choose General -&gt; Java Application

Next, select General from the Categories column, and Java Application from the Projects column:

You may notice mention of "J2SE" in the description pane; that is the old name for what is now known as the "Java SE" platform. Press the button labeled "Next" to proceed.

<a name="a5" id="a5"></a>

## Step 3: Set a Project Name

Now enter "CelsiusConverterProject" as the project name. You can leave the Project Location and Project Folder fields set to their default values, or click the Browse button to choose an alternate location on your system.

Make sure to deselect the "Create Main Class" checkbox; leaving this option selected generates a new class as the main entry point for the application, but our main GUI window (created in the next step) will serve that purpose, so checking this box is not necessary. Click the "Finish" button when you are done.

When the IDE finishes loading, you will see a screen similar to the above. All panes will be empty except for the Projects pane in the upper left hand corner, which shows the newly created project.

<a name="a6" id="a6"></a>

## Step 4: Add a JFrame Form

Now right-click the CelsiusConverterProject name and choose New -&gt; JFrame Form (`JFrame` is the Swing class responsible for the main frame for your application.) You will learn how to designate this class as the application's entry point later in this lesson.

<a name="a7" id="a7"></a>

## Step 5: Name the GUI Class

Next, type `CelsiusConverterGUI` as the class name, and `learn` as the package name. You can actually name this package anything you want, but here we are following the tutorial convention of naming the package after the lesson in which is resides.

The remainder of the fields should automatically be filled in, as shown above. Click the Finish button when you are done.

When the IDE finishes loading, the right pane will display a design-time, graphical view of the `CelsiusConverterGUI`. It is on this screen that you will visually drag, drop, and manipulate the various Swing components.
