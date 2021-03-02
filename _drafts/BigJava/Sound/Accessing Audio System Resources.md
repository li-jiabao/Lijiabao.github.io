
# Accessing Audio System Resources

The Java Sound API takes a flexible approach to system configuration. Different sorts of audio devices (mixers) can be installed on a computer. The API makes few assumptions about what devices have been installed and what their capabilities are. Instead, it provides ways for the system to report about the available audio components, and ways for your program to access them.

<a name="113098" id="113098"></a> The following sections show how your program can learn what sampled-audio resources have been installed on the computer, and how it can gain access to the available resources. Among other things, the resources include mixers and the various types of lines owned by the mixers.

<a name="113101" id="113101"></a>

## The AudioSystem Class

<a name="113103" id="113103"></a> The 
[`AudioSystem`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/AudioSystem.html) class acts as a clearinghouse for audio components, including built-in services and separately installed services from third-party providers. `AudioSystem` serves as an application program's entry point for accessing these installed sampled-audio resources. You can query the `AudioSystem` to learn what sorts of resources have been installed, and then you can obtain access to them. For example, an application program might start out by asking the `AudioSystem` class whether there is a mixer that has a certain configuration, such as one of the input or output configurations illustrated earlier in the discussion of lines. From the mixer, the program would then obtain data lines, and so on.

Here are some of the resources an application program can obtain from the `AudioSystem`:

- Mixers &#8212; A system typically has multiple mixers installed. There is usually at least one for audio input and one for audio output. There might also be mixers that don't have I/O ports but instead accept audio from an application program and deliver the mixed audio back to the program. The `AudioSystem` class provides a list of all of the installed mixers.
- Lines &#8212; Even though every line is associated with a mixer, an application program can get a line directly from the `AudioSystem`, without dealing explicitly with mixers.
- Format conversions &#8212; An application program can use format conversions to translate audio data from one format to another.
- Files and streams &#8212; The `AudioSystem` class provides methods for translating between audio files and audio streams. It can also report the file format of a sound file and can write files in different formats.

## Information Objects

<a name="113125" id="113125"></a> Several classes in the Java Sound API provide useful information about associated interfaces. For example, 
[`Mixer.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Mixer.Info.html) provides details about an installed mixer, such as the mixer's vendor, name, description, and version. 
[`Line.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Line.Info.html) obtains the class of a specific line. Subclasses of `Line.Info` include 
[`Port.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Port.Info.html) and 
[`DataLine.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/DataLine.Info.html) , which obtain details relevant to a specific port and data line, respectively. Each of these classes is described further in the appropriate section below. It's important not to confuse the `Info` object with the mixer or line object that it describes.

<a name="113128" id="113128"></a>

### Getting a Mixer

Usually, one of the first things a program that uses the Java Sound API needs to do is to obtain a mixer, or at least one line of a mixer, so that you can get sound into or out of the computer. Your program might need a specific kind of mixer, or you might want to display a list of all the available mixers so that the user can select one. In either case, you need to learn what kinds of mixers are installed. `AudioSystem` provides the following method:

```

static Mixer.Info[] getMixerInfo()

```

Each 
[`Mixer.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Mixer.Info.html) object returned by this method identifies one type of mixer that is installed. (Usually a system has at most one mixer of a given type. If there happens to be more than one of a given type, the returned array still only has one `Mixer.Info` for that type.) An application program can iterate over the `Mixer.Info` objects to find an appropriate one, according to its needs. The `Mixer.Info` includes the following strings to identify the kind of mixer:

- Name
- Version
- Vendor
- Description

These are arbitrary strings, so an application program that needs a specific mixer must know what to expect and what to compare the strings to. The company that provides the mixer should include this information in its documentation. Alternatively, and perhaps more typically, the application program will display all the `Mixer.Info` objects' strings to the user and let the user choose the corresponding mixer.

<a name="113143" id="113143"></a> Once an appropriate mixer is found, the application program invokes the following `AudioSystem` method to obtain the desired mixer:

```

static Mixer getMixer(Mixer.Info info)

```

What if your program needs a mixer that has certain capabilities, but it doesn't need a specific mixer made by a specific vendor? And what if you can't depend on the user's knowing which mixer should be chosen? In that case, the information in the `Mixer.Info` objects won't be of much use. Instead, you can iterate over all the `Mixer.Info` objects returned by `getMixerInfo`, get a mixer for each by invoking `getMixer`, and query each mixer for its capabilities. For example, you might need a mixer that can write its mixed audio data to a certain number of target data lines simultaneously. In that case, you would query each mixer using this `Mixer` method:

```

int getMaxLines(Line.Info info)

```

<a name="115024" id="115024"></a> Here, the 
[`Line.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Line.Info.html) would specify a `TargetDataLine`. The `Line.Info` class is discussed in the next section.

<a name="113154" id="113154"></a>

### Getting a Line of a Desired Type

There are two ways to get a line:

<li>Directly from the 
[`AudioSystem`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/AudioSystem.html) object</li>
- From a mixer that you have already obtained from the `AudioSystem` object

### Getting a Line Directly from the AudioSystem

<a name="113161" id="113161"></a> Let's assume you haven't obtained a mixer, and your program is a simple one that really only needs a certain kind of line; the details of the mixer don't matter to you. You can use the `AudioSystem` method:

```

static Line getLine(Line.Info info)

```

which is analogous to the `getMixer` method discussed previously. Unlike 
[`Mixer.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Mixer.Info.html) , the 
[`Line.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Line.Info.html) used as an argument doesn't store textual information to specify the desired line. Instead, it stores information about the class of line desired.

<a name="114553" id="114553"></a> `Line.Info` is an abstract class, so you use one of its subclasses ( 
[`Port.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Port.Info.html) or 
[`DataLine.Info`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/DataLine.Info.html) ) to obtain a line. The following code excerpt uses the `DataLine.Info` subclass to obtain and open a target data line:

```

TargetDataLine line;
DataLine.Info info = new DataLine.Info(TargetDataLine.class, 
    format); // format is an AudioFormat object
if (!AudioSystem.isLineSupported(info)) {
    // Handle the error.
    }
    // Obtain and open the line.
try {
    line = (TargetDataLine) AudioSystem.getLine(info);
    line.open(format);
} catch (LineUnavailableException ex) {
        // Handle the error.
    //... 
}

```

This code obtains a 
[`TargetDataLine`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/TargetDataLine.html) object without specifying any attributes other than its class and its audio format. You can use analogous code to obtain other kinds of lines. For a `SourceDataLine` or a `Clip`, just substitute that class for `TargetDataLine` as the class of the line variable, and also in the first argument to the `DataLine.Info` constructor.

<a name="113191" id="113191"></a> For a `Port`, you can use static instances of `Port.Info`, in code like the following:

```

if (AudioSystem.isLineSupported(Port.Info.MICROPHONE)) {
    try {
        line = (Port) AudioSystem.getLine(
            Port.Info.MICROPHONE);
    }
}

```

Note the use of the method `isLineSupported` to see whether the mixer even has a line of the desired type.

<a name="113199" id="113199"></a> Recall that a source line is an input to a mixer, namely, a `Port` object if the mixer represents an audio-input device, and a `SourceDataLine` or `Clip` object if the mixer represents an audio-output device. Similarly, a target line is an output of the mixer: a `Port` object for an audio-output mixer, and a `TargetDataLine` object for an audio-input mixer. What if a mixer doesn't connect to any external hardware device at all? For example, consider an internal or software-only mixer that gets audio from an application program and delivers its mixed audio back to the program. This kind of mixer has `SourceDataLine` or `Clip` objects for its input lines and `TargetDataLine` objects for its output lines.

<a name="113201" id="113201"></a> You can also use the following `AudioSystem` methods to learn more about source and target lines of a specified type that are supported by any installed mixer:

```

static Line.Info[] getSourceLineInfo(Line.Info info)
static Line.Info[] getTargetLineInfo(Line.Info info)

```

Note that the array returned by each of these methods indicates unique types of lines, not necessarily all the lines. For example, if two of a mixer's lines, or two lines of different mixers, have identical `Line.Info` objects, the two lines will represented by only one `Line.Info` in the returned array. <a name="113208" id="113208"></a>

### Getting a Line from a Mixer

<a name="113210" id="113210"></a> The `Mixer` interface includes variations on the `AudioSystem` access methods for source and target lines, described above. These `Mixer` methods include ones that take `Line.Info` arguments, just as `AudioSystem's` methods do. However, `Mixer` also includes these variants, which take no arguments:

```

Line.Info[] getSourceLineInfo()
Line.Info[] getTargetLineInfo()

```

These methods return arrays of all the `Line.Info` objects for the particular mixer. Once you've obtained the arrays, you can iterate over them, calling `Mixer's` `getLine` method to obtain each line, followed by `Line's` `open` method to reserve use of each line for your program. <a name="113216" id="113216"></a>

### Selecting Input and Output Ports

<a name="113217" id="113217"></a> The previous section, regarding how to obtain a line of a desired type, applies to ports as well as other types of lines. You can obtain all of the source (i.e., input) and target (i.e, output) ports by passing a `Port.Info` object to the `AudioSystem` (or `Mixer`) methods `getSourceLineInfo` and `getTargetLineInfo` that take a `Line.Info` argument. You then iterate over the returned array of objects and invoke Mixer's `getLine` method to get each port.

<a name="113219" id="113219"></a> You can then open each `Port` by invoking `Line's` `open` method. Opening a port means you turn it on&#226;&#128;&#148;that is, you allow sound to come in or out the port. Similarly, you can close ports that you don't want sound to travel through, because some ports might already be open before you even obtain them. Some platforms leave all ports on by default; or a user or system administrator might have selected certain ports to be on or off, using another application program or operating-system software.

<a name="113220" id="113220"></a> **Warning:** If you want to select a certain port and make sure that the sound is actually going in or out the port, you can open the port as described. However, this can be considered user-hostile behavior! For example, a user might have the speaker port turned off so as not to disturb her co-workers. She would be rather upset if your program suddenly overrode her wishes and started blaring music. As another example, a user might want to be assured that his computer's microphone is never turned on without his knowledge, to avoid eavesdropping. In general, it is recommended not to open or close ports unless your program is responding to the user's intentions, as expressed through the user interface. Instead, respect the settings that the user or the operating system has already selected.

<a name="113221" id="113221"></a> It isn't necessary to open or close a port before the mixer it's attached to will function correctly. For example, you can start playing back sound into an audio-output mixer, even though all its output ports are closed. The data still flows into the mixer; the playback isn't blocked. The user just won't hear anything. As soon as the user opens an output port, the sound will be audible through that port, starting at whatever point in the media the playback has already reached.

<a name="113222" id="113222"></a> Also, you don't need to access the ports to learn whether the mixer has certain ports. To learn whether a mixer is actually an audio-output mixer, for example, you can invoke `getTargetLineInfo` to see whether it has output ports. There's no reason to access the ports themselves unless you want to change their settings (such as their open-or-closed state, or the settings of any controls they might have).

<a name="113223" id="113223"></a>

### Permission to Use Audio Resources

<a name="115642" id="115642"></a> The Java Sound API includes an 
[`AudioPermission`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/AudioPermission.html) class that indicates what kinds of access an applet (or an application running with a security manager) can have to the sampled-audio system. Permission to record sound is controlled separately. This permission should be granted with care, to help prevent security risks such as unauthorized eavesdropping. By default, applets and applications are granted permissions as follows:

- An **applet** running with the applet security manager can play, but not record, audio.
- An **application** running with no security manager can both play and record audio.
- An application running with the default security manager can play, but not record, audio.

In general, applets are run under the scrutiny of a security manager and aren't permitted to record sound. Applications, on the other hand, don't automatically install a security manager, and are able to record sound. (However, if the default security manager is invoked explicitly for an application, the application isn't permitted to record sound.)

<a name="115660" id="115660"></a> Both applets and applications can record sound even when running with a security manager if they have been granted explicit permission to do so.

<a name="113244" id="113244"></a> If your program doesn't have permission to record (or play) sound, an exception will be thrown when it attempts to open a line. There is nothing you can do about this in your program, other than to catch the exception and report the problem to the user, because permissions can't be changed through the API. (If they could, they would be pointless, because nothing would be secure!) Generally, permissions are set in one or more policy configuration files, which a user or system administrator can edit using a text editor or the Policy Tool program.
