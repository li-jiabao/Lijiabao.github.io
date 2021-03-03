---
title: Using Files and Format Converters
author: lijiabao
date: 2020-12-06 01:47:44.580109000 +0800
category: Sound
categories: Sound
tags: Sound
---

# Using Files and Format Converters

<a name="117603" id="117603"></a> Most application programs that deal with sound need to read sound files or audio streams. This is common functionality, regardless of what the program may subsequently do with the data it reads (such as play, mix, or process it). Similarly, many programs need to write sound files (or streams). In some cases, the data that has been read (or that will be written) needs to be converted to a different format.

<a name="117604" id="117604"></a> As was briefly mentioned in 
[Accessing Audio System Resources](accessing.html), the Java Sound API provides application developers with various facilities for file input/output and format translations. Application programs can read, write, and translate between a variety of sound file formats and audio data formats.

<a name="114517" id="114517"></a> 
[Overview of the Sampled Package](sampled-overview.html) introduced the main classes related to sound files and audio data formats. As a review:

- <a name="114884" id="114884"></a>A stream of audio data, as might be read from or written to a file, is represented by an `AudioInputStream` object. (`AudioInputStream` inherits from `java.io.InputStream`.)
<li><a name="114885" id="114885"></a>The format of this audio data is represented by an `AudioFormat` object.
<a name="117222" id="117222"></a>This format specifies how the audio samples themselves are arranged, but not the structure of a file that they might be stored in. In other words, an `AudioFormat` describes "raw" audio data, such as the system might hand your program after capturing it from a microphone input or after parsing it from a sound file. An `AudioFormat` includes such information as the encoding, the byte order, the number of channels, the sampling rate, and the number of bits per sample.
</li>
- <a name="114521" id="114521"></a>There are several well-known, standard formats for sound files, such as WAV, AIFF, or AU. The different types of sound file have different structures for storing audio data as well as for storing descriptive information about the audio data. A sound file format is represented in the Java Sound API by an `AudioFileFormat` object. The `AudioFileFormat` includes an `AudioFormat` object to describe the format of the audio data stored in the file, and also includes information about the file type and the length of the data in the file.
- <a name="114522" id="114522"></a>The `AudioSystem` class provides methods for (1) storing a stream of audio data from an `AudioInputStream` into an audio file of a particular type (in other words, writing a file), (2) extracting a stream of audio bytes (an `AudioInputStream`) from an audio file (in other words, reading a file), and (3) converting audio data from one data format to another. This page, which is divided into three sections, explains these three kinds of activity.

<a name="114524" id="114524"></a>

an implementation of the Java Sound API does not necessarily provide comprehensive facilities for reading, writing, and converting audio in different data and file formats. It might support only the most common data and file formats. However, service providers can develop and distribute conversion services that extend this set, as you'll later see in [Providing Sampled-Audio Services](SPI-providing-sampled.html). The `AudioSystem` class supplies methods that allow application programs to learn what conversions are available, as described later under [Converting File and Data Formats](#114640). <a name="114527" id="114527"></a>

## Reading Sound Files

<a name="114529" id="114529"></a> The `AudioSystem` class provides two types of file-reading services:

- <a name="114530" id="114530"></a>Information about the format of the audio data stored in the sound file
- <a name="114531" id="114531"></a>A stream of formatted audio data that can be read from the sound file

<a name="118569" id="118569"></a> The first of these is given by three variants of the `getAudioFileFormat` method:

```

    static AudioFileFormat getAudioFileFormat (java.io.File file)
    static AudioFileFormat getAudioFileFormat(java.io.InputStream stream)
    static AudioFileFormat getAudioFileFormat (java.net.URL url)

```

As mentioned above, the returned `AudioFileFormat` object tells you the file type, the length of the data in the file, encoding, the byte order, the number of channels, the sampling rate, and the number of bits per sample.

<a name="114541" id="114541"></a> The second type of file-reading functionality is given by these `AudioSystem` methods

```

    static AudioInputStream getAudioInputStream (java.io.File file)
    static AudioInputStream getAudioInputStream (java.net.URL url)
    static AudioInputStream getAudioInputStream (java.io.InputStream stream)

```

These methods give you an object (an `AudioInputStream`) that lets you read the file's audio data, using one of the read methods of `AudioInputStream`. We'll see an example momentarily.

<a name="114549" id="114549"></a> Suppose you're writing a sound-editing application that allows the user to load sound data from a file, display a corresponding waveform or spectrogram, edit the sound, play back the edited data, and save the result in a new file. Or perhaps your program will read the data stored in a file, apply some kind of signal processing (such as an algorithm that slows the sound down without changing its pitch), and then play the processed audio. In either case, you need to get access to the data contained in the audio file. Assuming that your program provides some means for the user to select or specify an input sound file, reading that file's audio data involves three steps:

1. <a name="114551" id="114551"></a>Get an `AudioInputStream` object from the file.
1. <a name="114552" id="114552"></a>Create a byte array in which you'll store successive chunks of data from the file.
1. <a name="114553" id="114553"></a>Repeatedly read bytes from the audio input stream into the array. On each iteration, do something useful with the bytes in the array (for example, you might play them, filter them, analyze them, display them, or write them to another file).

<a name="114555" id="114555"></a> The following code snippet outlines these steps:

```

int totalFramesRead = 0;
File fileIn = new File(somePathName);
// somePathName is a pre-existing string whose value was
// based on a user selection.
try {
  AudioInputStream audioInputStream = 
    AudioSystem.getAudioInputStream(fileIn);
  int bytesPerFrame = 
    audioInputStream.getFormat().getFrameSize();
    if (bytesPerFrame == AudioSystem.NOT_SPECIFIED) {
    // some audio formats may have unspecified frame size
    // in that case we may read any amount of bytes
    bytesPerFrame = 1;
  } 
  // Set an arbitrary buffer size of 1024 frames.
  int numBytes = 1024 * bytesPerFrame; 
  byte[] audioBytes = new byte[numBytes];
  try {
    int numBytesRead = 0;
    int numFramesRead = 0;
    // Try to read numBytes bytes from the file.
    while ((numBytesRead = 
      audioInputStream.read(audioBytes)) != -1) {
      // Calculate the number of frames actually read.
      numFramesRead = numBytesRead / bytesPerFrame;
      totalFramesRead += numFramesRead;
      // Here, do something useful with the audio data that's 
      // now in the audioBytes array...
    }
  } catch (Exception ex) { 
    // Handle the error...
  }
} catch (Exception e) {
  // Handle the error...
}

```

Let's take a look at what's happening in the above code sample. First, the outer try clause instantiates an `AudioInputStream` object through the call to the `AudioSystem.getAudioInputStream(File)` method. This method transparently performs all of the testing required to determine whether the specified file is actually a sound file of a type that is supported by the Java Sound API. If the file being inspected (`fileIn` in this example) is not a sound file, or is a sound file of some unsupported type, an `UnsupportedAudioFileException` exception is thrown. This behavior is convenient, in that the application programmer need not be bothered with testing file attributes, nor with adhering to any file-naming conventions. Instead, the `getAudioInputStream` method takes care of all the low-level parsing and verification that is required to validate the input file. <a name="114595" id="114595"></a> The outer `try` clause then creates a byte array, `audioBytes`, of an arbitrary fixed length. We make sure that its length in bytes equals an integral number of frames, so that we won't end up reading only part of a frame or, even worse, only part of a sample. This byte array will serve as a buffer to temporarily hold a chunk of audio data as it's read from the stream. If we knew we would be reading nothing but very short sound files, we could make this array the same length as the data in the file, by deriving the length in bytes from the length in frames, as returned by `AudioInputStream's getFrameLength` method. (Actually, we'd probably just use a `Clip` object instead.) But to avoid running out of memory in the general case, we instead read the file in chunks, one buffer at a time.

<a name="114597" id="114597"></a> The inner `try` clause contains a `while` loop, which is where we read the audio data from the `AudioInputStream` into the byte array. You should add code in this loop to handle the audio data in this array in whatever way is appropriate for your program's needs. If you're applying some kind of signal processing to the data, you'll probably need to query the `AudioInputStream's AudioFormat` further, to learn the number of bits per sample and so on.

<a name="114599" id="114599"></a> Note that the method `AudioInputStream.read(byte[])` returns the number of **bytes** read&#226;&#128;&#148;not the number of samples or frames. This method returns -1 when there's no more data to read. Upon detecting this condition, we break from the `while` loop.

<a name="114602" id="114602"></a>

## Writing Sound Files

<a name="114604" id="114604"></a> The previous section described the basics of reading a sound file, using specific methods of the `AudioSystem` and `AudioInputStream` classes. This section describes how to write audio data out to a new file.

<a name="114606" id="114606"></a> The following `AudioSystem` method creates a disk file of a specified file type. The file will contain the audio data that's in the specified `AudioInputStream`:

```

static int write(AudioInputStream in, 
  AudioFileFormat.Type fileType, File out)

```

Note that the second argument must be one of the file types supported by the system (for example, AU, AIFF, or WAV), otherwise the `write` method will throw an `IllegalArgumentException`. To avoid this, you can test whether or not a particular `AudioInputStream` may be written to a particular type of file, by invoking this `AudioSystem` method:

```

static boolean isFileTypeSupported
  (AudioFileFormat.Type fileType, AudioInputStream stream)

```

which will return `true` only if the particular combination is supported.

<a name="114618" id="114618"></a> More generally, you can learn what types of file the system can write by invoking one of these `AudioSystem` methods:

```

static AudioFileFormat.Type[] getAudioFileTypes() 
static AudioFileFormat.Type[] getAudioFileTypes(AudioInputStream stream) 

```

The first of these returns all the types of file that the system can write, and the second returns only those that the system can write from the given audio input stream.

<a name="119705" id="119705"></a> The following excerpt demonstrates one technique for creating an output file from an `AudioInputStream` using the `write` method mentioned above.

```

File fileOut = new File(someNewPathName);
AudioFileFormat.Type fileType = fileFormat.getType();
if (AudioSystem.isFileTypeSupported(fileType, 
    audioInputStream)) {
  AudioSystem.write(audioInputStream, fileType, fileOut);
}

```

The first statement above, creates a new `File` object, `fileOut`, with a user- or program-specified pathname. The second statement gets a file type from a pre-existing `AudioFileFormat` object called `fileFormat`, which might have been obtained from another sound file, such as the one that was read in [Reading Sound Files](#114527) above. (You could instead supply whatever supported file type you want, instead of getting the file type from elsewhere. For example, you might delete the second statement and replace the other two occurrences of `fileType` in the code above with `AudioFileFormat.Type.WAVE`.)

<a name="114635" id="114635"></a> The third statement tests whether a file of the designated type can be written from a desired `AudioInputStream`. Like the file format, this stream might have been derived from the sound file previously read. (If so, presumably you've processed or altered its data in some way, because otherwise there are easier ways to simply copy a file.) Or perhaps the stream contains bytes that have been freshly captured from the microphone input.

<a name="114637" id="114637"></a> Finally, the stream, file type, and output file are passed to the `AudioSystem`.`write` method, to accomplish the goal of writing the file.

<a name="114640" id="114640"></a>

## Converting File and Data Formats

<a name="114642" id="114642"></a> Recall from 
[What is Formatted Audio Data?](sampled-overview.html#formatted), that the Java Sound API distinguishes between audio **file** formats and audio **data** formats. The two are more or less independent. Roughly speaking, the data format refers to the way in which the computer represents each raw data point (sample), while the file format refers to the organization of a sound file as stored on a disk. Each sound file format has a particular structure that defines, for example, the information stored in the file's header. In some cases, the file format also includes structures that contain some form of meta-data, in addition to the actual "raw" audio samples. The remainder of this page examines methods of the Java Sound API that enable a variety of file-format and data-format conversions.

<a name="114644" id="114644"></a>

## Converting from One File Format to Another

<a name="114646" id="114646"></a> This section covers the fundamentals of converting audio file types in the Java Sound API. Once again we pose a hypothetical program whose purpose, this time, is to read audio data from an arbitrary input file and write it into a file whose type is AIFF. Of course, the input file must be of a type that the system is capable of reading, and the output file must be of a type that the system is capable of writing. (In this example, we assume that the system is capable of writing AIFF files.) The example program doesn't do any data format conversion. If the input file's data format can't be represented as an AIFF file, the program simply notifies the user of that problem. On the other hand, if the input sound file is an already an AIFF file, the program notifies the user that there is no need to convert it.

<a name="114648" id="114648"></a> The following function implements the logic just described:

```

public void ConvertFileToAIFF(String inputPath, 
  String outputPath) {
  AudioFileFormat inFileFormat;
  File inFile;
  File outFile;
  try {
    inFile = new File(inputPath);
    outFile = new File(outputPath);     
  } catch (NullPointerException ex) {
    System.out.println("Error: one of the 
      ConvertFileToAIFF" +" parameters is null!");
    return;
  }
  try {
    // query file type
    inFileFormat = AudioSystem.getAudioFileFormat(inFile);
    if (inFileFormat.getType() != AudioFileFormat.Type.AIFF) 
    {
      // inFile is not AIFF, so let's try to convert it.
      AudioInputStream inFileAIS = 
        AudioSystem.getAudioInputStream(inFile);
      inFileAIS.reset(); // rewind
      if (AudioSystem.isFileTypeSupported(
             AudioFileFormat.Type.AIFF, inFileAIS)) {
         // inFileAIS can be converted to AIFF. 
         // so write the AudioInputStream to the
         // output file.
         AudioSystem.write(inFileAIS,
           AudioFileFormat.Type.AIFF, outFile);
         System.out.println("Successfully made AIFF file, "
           + outFile.getPath() + ", from "
           + inFileFormat.getType() + " file, " +
           inFile.getPath() + ".");
         inFileAIS.close();
         return; // All done now
       } else
         System.out.println("Warning: AIFF conversion of " 
           + inFile.getPath()
           + " is not currently supported by AudioSystem.");
    } else
      System.out.println("Input file " + inFile.getPath() +
          " is AIFF." + " Conversion is unnecessary.");
  } catch (UnsupportedAudioFileException e) {
    System.out.println("Error: " + inFile.getPath()
        + " is not a supported audio file type!");
    return;
  } catch (IOException e) {
    System.out.println("Error: failure attempting to read " 
      + inFile.getPath() + "!");
    return;
  }
}

```

<a name="114706" id="114706"></a> As mentioned, the purpose of this example function, `ConvertFileToAIFF`, is to query an input file to determine whether it's an AIFF sound file, and if it isn't, to try to convert it to one, producing a new copy whose pathname is specified by the second argument. (As an exercise, you might try making this function more general, so that instead of always converting to AIFF, the function converts to the file type specified by a new function argument.) Note that the audio data format of the copy&#226;&#128;&#148;that is, the new file-mimics the audio data format of original input file.

<a name="114708" id="114708"></a> Most of this function is self-explanatory and is not specific to the Java Sound API. There are, however, a few Java Sound API methods used by the routine that are crucial for sound file-type conversions. These method invocations are all found in the second `try` clause, above, and include the following:

- <a name="114710" id="114710"></a>`AudioSystem.getAudioFileFormat`: used here to determine whether the input file is already an AIFF type. If so, the function quickly returns; otherwise the conversion attempt proceeds.
- <a name="114712" id="114712"></a>`AudioSystem.isFileTypeSupported`: Indicates whether the system can write a file of the specified type that contains audio data from the specified `AudioInputStream.` In our example, this method returns `true` if the specified audio input file can be converted to AIFF audio file format. If `AudioFileFormat.Type.AIFF` isn't supported, `ConvertFileToAIFF` issues a warning that the input file can't be converted, then returns.
- <a name="114714" id="114714"></a>`AudioSystem.write`: used here to write the audio data from the AudioInputStream `inFileAIS` to the output file `outFile`.

<a name="114716" id="114716"></a> The second of these methods, `isFileTypeSupported`, helps to determine, in advance of the write, whether a particular input sound file can be converted to a particular output sound file type. In the next section we will see how, with a few modifications to this `ConvertFileToAIFF` sample routine, we can convert the audio data format, as well as the sound file type.

<a name="114718" id="114718"></a>

## Converting Audio between Different Data Formats

<a name="114720" id="114720"></a> The previous section showed how to use the Java Sound API to convert a file from one **file** format (that is, one type of sound file) to another. This section explores some of the methods that enable audio **data** format conversions.

<a name="114722" id="114722"></a> In the previous section, we read data from a file of an arbitrary type, and saved it in an AIFF file. Note that although we changed the type of file used to store the data, we didn't change the format of the audio data itself. (Most common audio file types, including AIFF, can contain audio data of various formats.) So if the original file contained CD-quality audio data (16-bit sample size, 44.1-kHz sample rate, and two channels), so would our output AIFF file.

<a name="114724" id="114724"></a> Now suppose that we want to specify the **data** format of the output file, as well as the file type. For example, perhaps we are saving many long files for use on the Internet, and are concerned about the amount of disk space and download time required by our files. We might choose to create smaller AIFF files that contain lower-resolution data-for example, data that has an 8-bit sample size, an 8-kHz sample rate, and a single channel.

<a name="114726" id="114726"></a> Without going into as much coding detail as before, let's explore some of the methods used for data format conversion, and consider the modifications that we would need to make to the `ConvertFileToAIFF` function to accomplish the new goal.

<a name="118599" id="118599"></a> The principal method for audio data conversion is, once again, found in the `AudioSystem` class. This method is a variant of `getAudioInputStream`:

```

AudioInputStream getAudioInputStream(AudioFormat
    format, AudioInputStream stream)

```

This function returns an `AudioInputStream` that is the result of converting the `AudioInputStream`, `stream`, using the indicated `AudioFormat`, `format`. If the conversion isn't supported by `AudioSystem`, this function throws an `IllegalArgumentException`.

<a name="114734" id="114734"></a> To avoid that, we can first check whether the system can perform the required conversion by invoking this `AudioSystem` method:

```

boolean isConversionSupported(AudioFormat targetFormat,
    AudioFormat sourceFormat)

```

In this case, we'd pass `stream.getFormat()` as the second argument.

<a name="119726" id="119726"></a> To create a specific `AudioFormat` object, we use one of the two `AudioFormat` constructors shown below, either:

```

AudioFormat(float sampleRate, int sampleSizeInBits,
    int channels, boolean signed, boolean bigEndian)

```

which constructs an `AudioFormat` with a linear PCM encoding and the given parameters, or:

```

AudioFormat(AudioFormat.Encoding encoding, 
    float sampleRate, int sampleSizeInBits, int channels,
    int frameSize, float frameRate, boolean bigEndian) 

```

which also constructs an `AudioFormat`, but lets you specify the encoding, frame size, and frame rate, in addition to the other parameters.

<a name="114751" id="114751"></a> Now, armed with the methods above, let's see how we might extend our `ConvertFileToAIFF` function to perform the desired "low-res" audio data format conversion. First, we would construct an `AudioFormat` object describing the desired output audio data format. The following statement would suffice and could be inserted near the top of the function:

```

AudioFormat outDataFormat = new AudioFormat((float) 8000.0,
(int) 8, (int) 1, true, false);

```

Since the `AudioFormat` constructor above is describing a format with 8-bit samples, the last parameter to the constructor, which specifies whether the samples are big or little endian, is irrelevant. (Big versus little endian is only an issue if the sample size is greater than a single byte.)

<a name="114758" id="114758"></a> The following example shows how we would use this new `AudioFormat` to convert the `AudioInputStream`, `inFileAIS`, that we created from the input file:

```

AudioInputStream lowResAIS;         
  if (AudioSystem.isConversionSupported(outDataFormat,   
    inFileAIS.getFormat())) {
    lowResAIS = AudioSystem.getAudioInputStream
      (outDataFormat, inFileAIS);
  }

```

It wouldn't matter too much where we inserted this code, as long as it was after the construction of `inFileAIS`. Without the `isConversionSupported` test, the call would fail and throw an `IllegalArgumentException` if the particular conversion being requested was unsupported. (In this case, control would transfer to the appropriate `catch` clause in our function.)

<a name="114769" id="114769"></a> So by this point in the process, we would have produced a new `AudioInputStream`, resulting from the conversion of the original input file (in its `AudioInputStream` form) to the desired low-resolution audio data format as defined by `outDataFormat`.

<a name="114771" id="114771"></a> The final step to produce the desired low-resolution, AIFF sound file would be to replace the `AudioInputStream` parameter in the call to `AudioSystem.write` (that is, the first parameter) with our converted stream, `lowResAIS`, as follows:

```

AudioSystem.write(lowResAIS, AudioFileFormat.Type.AIFF, 
  outFile);

```

These few modifications to our earlier function produce something that converts both the audio data and the file format of any specified input file, assuming of course that the system supports the conversion. <a name="114777" id="114777"></a>

## Learning What Conversions Are Available

<a name="114779" id="114779"></a> Several `AudioSystem` methods test their parameters to determine whether the system supports a particular data format conversion or file-writing operation. (Typically, each method is paired with another that performs the data conversion or writes the file.) One of these query methods, `AudioSystem.isFileTypeSupported`, was used in our example function, `ConvertFileToAIFF`, to determine whether the system was capable of writing the audio data to an AIFF file. A related `AudioSystem` method, `getAudioFileTypes(AudioInputStream)`, returns the complete list of supported file types for the given stream, as an array of `AudioFileFormat.Type` instances. The method: BEGINCODE boolean isConversionSupported(AudioFormat.Encoding encoding,<br />
AudioFormat format) 
