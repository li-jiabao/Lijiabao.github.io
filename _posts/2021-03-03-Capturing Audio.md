---
title: Capturing Audio
author: lijiabao
date: 2020-12-06 01:47:40.267294500 +0800
category: Sound
categories: Sound
tags: Sound
---

# Capturing Audio

<a name="114180" id="114180"></a> **Capturing** refers to the process of obtaining a signal from outside the computer. A common application of audio capture is recording, such as recording the microphone input to a sound file. However, capturing isn't synonymous with recording, because recording implies that the application always saves the sound data that's coming in. An application that captures audio doesn't necessarily store the audio. Instead it might do something with the data as it's coming in &#8212; such as transcribe speech into text &#8212; but then discard each buffer of audio as soon as it's finished with that buffer.

<a name="114181" id="114181"></a> As discussed in 
[Overview of the Sampled Package](sampled-overview.html), a typical audio-input system in an implementation of the Java Sound API consists of:

1. <a name="113952" id="113952"></a>An input port, such as a microphone port or a line-in port, which feeds its incoming audio data into:
1. <a name="115691" id="115691"></a>A mixer, which places the input data in:
1. <a name="115692" id="115692"></a>One or more target data lines, from which an application can retrieve the data.

<a name="113956" id="113956"></a> Commonly, only one input port can be open at a time, but an audio-input mixer that mixes audio from multiple ports is also possible. Another scenario consists of a mixer that has no ports but instead gets its audio input over a network.

<a name="113958" id="113958"></a> The `TargetDataLine` interface was introduced briefly under 
[The Line Interface Hierarchy](sampled-overview.html#lineHierarchy). `TargetDataLine` is directly analogous to the `SourceDataLine` interface, which was discussed extensively in 
[Playing Back Audio](playing.html). Recall that the `SourceDataLine` interface consists of:

- <a name="113960" id="113960"></a>A `write` method to send audio to the mixer
- <a name="113961" id="113961"></a>An `available` method to determine how much data can be written to the buffer without blocking

<a name="113963" id="113963"></a> Similarly, `TargetDataLine` consists of:

- <a name="113965" id="113965"></a>A `read` method to get audio from the mixer
- <a name="113966" id="113966"></a>An `available` method to determine how much data can be read from the buffer without blocking

## <a name="113969" id="113969"></a>Setting Up a TargetDataLine

<a name="113971" id="113971"></a> The process of obtaining a target data line was described in 
[Accessing Audio System Resources](accessing.html) but we repeat it here for convenience:

```

TargetDataLine line;
DataLine.Info info = new DataLine.Info(TargetDataLine.class, 
    format); // format is an AudioFormat object
if (!AudioSystem.isLineSupported(info)) {
    // Handle the error ... 

}
// Obtain and open the line.
try {
    line = (TargetDataLine) AudioSystem.getLine(info);
    line.open(format);
} catch (LineUnavailableException ex) {
    // Handle the error ... 
}

```

<a name="113992" id="113992"></a> You could instead invoke `Mixer's` `getLine` method, rather than `AudioSystem's`.

<a name="113994" id="113994"></a> As shown in this example, once you've obtained a target data line, you reserve it for your application's use by invoking the `SourceDataLine` method `open`, exactly as was described in the case of a source data line in 
[Playing Back Audio](playing.html). The single-parameter version of the `open` method causes the line's buffer to have the default size. You can instead set the buffer size according to your application's needs by invoking the two-parameter version:

```

void open(AudioFormat format, int bufferSize)

```

<a name="115741" id="115741"></a>

## Reading the Data from the TargetDataLine

<a name="114004" id="114004"></a> Once the line is open, it is ready to start capturing data, but it isn't active yet. To actually commence the audio capture, use the `DataLine` method `start`. This begins delivering input audio data to the line's buffer for your application to read. Your application should invoke start only when it's ready to begin reading from the line; otherwise a lot of processing is wasted on filling the capture buffer, only to have it overflow (that is, discard data).

<a name="114006" id="114006"></a> To start retrieving data from the buffer, invoke `TargetDataLine's` read method:

```

int read(byte[] b, int offset, int length)

```

This method attempts to read `length` bytes of data into the array `b`, starting at the byte position `offset` in the array. The method returns the number of bytes actually read.

<a name="114011" id="114011"></a> As with `SourceDataLine's write` method, you can request more data than actually fits in the buffer, because the method blocks until the requested amount of data has been delivered, even if you request many buffers' worth of data.

<a name="115205" id="115205"></a> To avoid having your application hang during recording, you can invoke the read method within a loop, until you've retrieved all the audio input, as in this example:

```

// Assume that the TargetDataLine, line, has already
// been obtained and opened.
ByteArrayOutputStream out  = new ByteArrayOutputStream();
int numBytesRead;
byte[] data = new byte[line.getBufferSize() / 5];

// Begin audio capture.
line.start();

// Here, stopped is a global boolean set by another thread.
while (!stopped) {
   // Read the next chunk of data from the TargetDataLine.
   numBytesRead =  line.read(data, 0, data.length);
   // Save this chunk of data.
   out.write(data, 0, numBytesRead);
}     

```

Notice that in this example, the size of the byte array into which the data is read is set to be one-fifth the size of the line's buffer. If you instead make it as big as the line's buffer and try to read the entire buffer, you need to be very exact in your timing, because data will be dumped if the mixer needs to deliver data to the line while you are reading from it. By using some fraction of the line's buffer size, as shown here, your application will be more successful in sharing access to the line's buffer with the mixer.

<a name="114034" id="114034"></a> The `read` method of `TargetDataLine` takes three arguments: a byte array, an offset into the array, and the number of bytes of input data that you would like to read. In this example, the third argument is simply the length of your byte array. The `read` method returns the number of bytes that were actually read into your array.

<a name="114036" id="114036"></a> Typically, you read data from the line in a loop, as in this example. Within the `while` loop, each chunk of retrieved data is processed in whatever way is appropriate for the application&#226;&#128;&#148;here, it's written to a `ByteArrayOutputStream`. Not shown here is the use of a separate thread to set the boolean `stopped`, which terminates the loop. This boolean's value might be set to `true` when the user clicks a Stop button, and also when a listener receives a `CLOSE` or `STOP` event from the line. The listener is necessary for `CLOSE` events and recommended for `STOP` events. Otherwise, if the line gets stopped somehow without stopped being set to `true`, the `while` loop will capture zero bytes on each iteration, running fast and wasting CPU cycles. A more thorough code example would show the loop being re-entered if capture becomes active again.

<a name="114039" id="114039"></a> As with a source data line, it's possible to drain or flush a target data line. For example, if you're recording the input to a file, you'll probably want to invoke the `drain` method when the user clicks a Stop button. The `drain` method will cause the mixer's remaining data to get delivered to the target data line's buffer. If you don't drain the data, the captured sound might seem to be truncated prematurely at the end.

<a name="114041" id="114041"></a> There might be some cases where you instead want to flush the data. In any case, if you neither flush nor drain the data, it will be left in the mixer. This means that when capture recommences, there will be some leftover sound at the beginning of the new recording, which might be undesirable. It can be useful, then, to flush the target data line before restarting the capture.

<a name="114432" id="114432"></a>

## Monitoring the Line's Status

<a name="114046" id="114046"></a> Because the `TargetDataLine` interface extends `DataLine`, target data lines generate events in the same way source data lines do. You can register an object to receive events whenever the target data line opens, closes, starts, or stops. For more information, see the previous discussion of 
[Monitoring a Line's Status](playing.html#113711).

<a name="114049" id="114049"></a>

## Processing the Incoming Audio

<a name="114051" id="114051"></a> Like some source data lines, some mixers' target data lines have signal-processing controls, such as gain, pan, reverb, or sample-rate controls. The input ports might have similar controls, especially gain controls. In the next section, you'll learn how to determine whether a line has such controls, and how to use them if it does.
