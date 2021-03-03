---
title: Playing Back Audio
author: lijiabao
date: 2020-12-06 01:47:38.469916600 +0800
category: Sound
categories: Sound
tags: Sound
---

# Playing Back Audio

Playback is sometimes referred to as **presentation** or **rendering**. These are general terms that are applicable to other kinds of media besides sound. The essential feature is that a sequence of data is delivered somewhere for eventual perception by a user. If the data is time-based, as sound is, it must be delivered at the correct rate. With sound even more than video, it's important that the rate of data flow be maintained, because interruptions to sound playback often produce loud clicks or irritating distortion. The Java Sound API is designed to help application programs play sounds smoothly and continuously, even very long sounds.

<a name="113599" id="113599"></a> Earlier you saw how to obtain a line from the audio system or from a mixer. Here you will learn how to play sound through a line.

<a name="113601" id="113601"></a> As you know, there are two kinds of line that you can use for playing sound: a 
[`Clip`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/Clip.html) and a 
[`SourceDataLine`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/SourceDataLine.html). The primary difference between the two is that with a `Clip` you specify all the sound data at one time, before playback, whereas with a `SourceDataLine` you keep writing new buffers of data continuously during playback. Although there are many situations in which you could use either a `Clip` or a `SourceDataLine`, the following criteria help identify which kind of line is better suited for a particular situation:

<li><a name="113603" id="113603"></a>Use a `Clip` when you have non-real-time sound data that can be preloaded into memory. <a name="115889" id="115889"></a>
For example, you might read a short sound file into a clip. If you want the sound to play back more than once, a `Clip` is more convenient than a `SourceDataLine`, especially if you want the playback to loop (cycle repeatedly through all or part of the sound). If you need to start the playback at an arbitrary position in the sound, the `Clip` interface provides a method to do that easily. Finally, playback from a `Clip` generally has less latency than buffered playback from a `SourceDataLine`. In other words, because the sound is preloaded into a clip, playback can start immediately instead of having to wait for the buffer to be filled.
</li>
<li><a name="113605" id="113605"></a>Use a `SourceDataLine` for streaming data, such as a long sound file that won't all fit in memory at once, or a sound whose data can't be known in advance of playback. <a name="115858" id="115858"></a>
As an example of the latter case, suppose you're monitoring sound input&#226;&#128;&#148;that is, playing sound back as it's being captured. If you don't have a mixer that can send input audio right back out an output port, your application program will have to take the captured data and send it to an audio-output mixer. In this case, a `SourceDataLine` is more appropriate than a `Clip`. Another example of sound that can't be known in advance occurs when you synthesize or manipulate the sound data interactively in response to the user's input. For example, imagine a game that gives aural feedback by "morphing" from one sound to another as the user moves the mouse. The dynamic nature of the sound transformation requires the application program to update the sound data continuously during playback, instead of supplying it all before playback starts.
</li>

<a name="113609" id="113609"></a>

## Using a Clip

<a name="117625" id="117625"></a> You obtain a `Clip` as described earlier under 
[Getting a Line of a Desired Type](accessing.html#113154); Construct a `DataLine.Info` object with `Clip.class` for the first argument, and pass this `DataLine.Info` as an argument to the `getLine` method of `AudioSystem` or `Mixer`.

<a name="113615" id="113615"></a> Obtaining a line just means you've gotten a way to refer to it; `getLine` doesn't actually reserve the line for you. Because a mixer might have a limited number of lines of the desired type available, it can happen that after you invoke `getLine` to obtain the clip, another application program jumps in and grabs the clip before you're ready to start playback. To actually use the clip, you need to reserve it for your program's exclusive use by invoking one of the following `Clip` methods:

```

void open(AudioInputStream stream)
void open(AudioFormat format, byte[] data, int offset, int bufferSize)

```

Despite the `bufferSize` argument in the second `open` method above, `Clip` (unlike `SourceDataLine`) includes no methods for writing new data to the buffer. The `bufferSize` argument here just specifies how much of the byte array to load into the clip. It's not a buffer into which you can subsequently load more data, as you can with a `SourceDataLine's` buffer.

<a name="113623" id="113623"></a> After opening the clip, you can specify at what point in the data it should start playback, using `Clip's` `setFramePosition` or `setMicroSecondPosition` methods. Otherwise, it will start at the beginning. You can also configure the playback to cycle repeatedly, using the `setLoopPoints` method.

<a name="113628" id="113628"></a> When you're ready to start playback, simply invoke the `start` method. To stop or pause the clip, invoke the `stop` method, and to resume playback, invoke `start` again. The clip remembers the media position where it stopped playback, so there's no need for explicit pause and resume methods. If you don't want it to resume where it left off, you can "rewind" the clip to the beginning (or to any other position, for that matter) using the frame- or microsecond-positioning methods mentioned above.

<a name="113630" id="113630"></a> A `Clip's` volume level and activity status (active versus inactive) can be monitored by invoking the `DataLine` methods `getLevel` and `isActive`, respectively. An active `Clip` is one that is currently playing sound.

<a name="113634" id="113634"></a>

## Using a SourceDataLine

<a name="116360" id="116360"></a> Obtaining a `SourceDataLine` is similar to obtaining a `Clip`. <a name="116365" id="116365"></a> Opening the `SourceDataLine` is also similar to opening a `Clip`, in that the purpose is once again to reserve the line. However, you use a different method, inherited from `DataLine`:

```

void open(AudioFormat format)

```

Notice that when you open a `SourceDataLine`, you don't associate any sound data with the line yet, unlike opening a `Clip`. Instead, you just specify the format of the audio data you want to play. The system chooses a default buffer length.

<a name="113646" id="113646"></a> You can also stipulate a certain buffer length in bytes, using this variant:

<a name="113648" id="113648"></a>

```

void open(AudioFormat format, int bufferSize)

```

For consistency with similar methods, the `bufferSize` argument is expressed in bytes, but it must correspond to an integral number of frames.

<a name="113660" id="113660"></a> Instead of using the open method described above, it's also possible to open a `SourceDataLine` using `Line's` `open()` method, without arguments. In this case, the line is opened with its default audio format and buffer size. However, you can't change these later. If you want to know the line's default audio format and buffer size, you can invoke `DataLine's` `getFormat` and `getBufferSize` methods, even before the line has ever been opened.

<a name="113663" id="113663"></a> Once the `SourceDataLine` is open, you can start playing sound. You do this by invoking `DataLine's` start method, and then writing data repeatedly to the line's playback buffer.

<a name="113667" id="113667"></a> The start method permits the line to begin playing sound as soon as there's any data in its buffer. You place data in the buffer by the following method:

```

int write(byte[] b, int offset, int length)

```

The offset into the array is expressed in bytes, as is the array's length.

[Monitoring a Line's Status](#113711). The line is now considered active, so the `isActive` method of `DataLine` will return `true`. Notice that all this happens only once the buffer contains data to play, not necessarily right when the start method is invoked. If you invoked `start` on a new `SourceDataLine` but never wrote data to the buffer, the line would never be active and a `START` event would never be sent. (However, in this case, the `isRunning` method of `DataLine` would return `true`.)

<a name="113675" id="113675"></a> So how do you know how much data to write to the buffer, and when to send the second batch of data? Fortunately, you don't need to time the second invocation of write to synchronize with the end of the first buffer! Instead, you can take advantage of the `write` method's blocking behavior:

- <a name="113676" id="113676"></a>The method returns as soon as the data has been written to the buffer. It doesn't wait until all the data in the buffer has finished playing. (If it did, you might not have time to write the next buffer without creating a discontinuity in the audio.)
- <a name="113677" id="113677"></a>It's all right to try to write more data than the buffer will hold. In this case, the method blocks (doesn't return) until all the data you requested has actually been placed in the buffer. In other words, one buffer's worth of your data at a time will be written to the buffer and played, until the remaining data all fits in the buffer, at which point the method returns. Whether or not the method blocks, it returns as soon as the last buffer's worth of data from this invocation can be written. Again, this means that your code will in all likelihood regain control before playback of the last buffer's worth of data has finished.
- <a name="113678" id="113678"></a>While in many contexts it is fine to write more data than the buffer will hold, if you want to be certain that the next write issued does not block, you can limit the number of bytes you write to the number that `DataLine's` `available` method returns.

<a name="113680" id="113680"></a> Here's an example of iterating through chunks of data that are read from a stream, writing one chunk at a time to the `SourceDataLine` for playback:

```

// read chunks from a stream and write them to a source data 
line 
line.start();
while (total &lt; totalToRead &amp;&amp; !stopped)}
    numBytesRead = stream.read(myData, 0, numBytesToRead);
    if (numBytesRead == -1) break;
    total += numBytesRead; 
    line.write(myData, 0, numBytesRead);

}

```

If you don't want the `write` method to block, you can first invoke the `available` method (inside the loop) to find out how many bytes can be written without blocking, and then limit the `numBytesToRead` variable to this number, before reading from the stream. In the example given, though, blocking won't matter much, since the write method is invoked inside a loop that won't complete until the last buffer is written in the final loop iteration. Whether or not you use the blocking technique, you'll probably want to invoke this playback loop in a separate thread from the rest of the application program, so that your program doesn't appear to freeze when playing a long sound. On each iteration of the loop, you can test whether the user has requested playback to stop. Such a request needs to set the `stopped` boolean, used in the code above, to `true`.

<a name="113696" id="113696"></a> Since `write` returns before all the data has finished playing, how do you learn when the playback has actually completed? One way is to invoke the `drain` method of `DataLine` after writing the last buffer's worth of data. This method blocks until all the data has been played. When control returns to your program, you can free up the line, if desired, without fear of prematurely cutting off the playback of any audio samples:

```

line.write(b, offset, numBytesToWrite); 
//this is the final invocation of write
line.drain();
line.stop();
line.close();
line = null;

```

You can intentionally stop playback prematurely, of course. For example, the application program might provide the user with a Stop button. Invoke `DataLine's stop` method to stop playback immediately, even in the middle of a buffer. This leaves any unplayed data in the buffer, so that if you subsequently invoke `start`, the playback resumes where it left off. If that's not what you want to happen, you can discard the data left in the buffer by invoking `flush`.

<a name="113708" id="113708"></a> A `SourceDataLine` generates a `STOP` event whenever the flow of data has been stopped, whether this stoppage was initiated by the drain method, the stop method, or the flush method, or because the end of a playback buffer was reached before the application program invoked `write` again to provide new data. A `STOP` event doesn't necessarily mean that the `stop` method was invoked, and it doesn't necessarily mean that a subsequent invocation of `isRunning` will return `false`. It does, however, mean that `isActive` will return `false`. (When the `start` method has been invoked, the `isRunning` method will return `true`, even if a `STOP` event is generated, and it will begin to return `false` only once the `stop` method is invoked.) It's important to realize that `START` and `STOP` events correspond to `isActive`, not to `isRunning`.

<a name="113711" id="113711"></a>

## Monitoring a Line's Status

<a name="113713" id="113713"></a> Once you have started a sound playing, how do you find when it's finished? We saw one solution above, invoking the `drain` method after writing the last buffer of data, but that approach is applicable only to a `SourceDataLine`. Another approach, which works for both `SourceDataLines` and `Clips`, is to register to receive notifications from the line whenever the line changes its state. These notifications are generated in the form of `LineEvent` objects, of which there are four types: `OPEN`, `CLOSE`, `START`, and `STOP`.

<a name="113715" id="113715"></a> Any object in your program that implements the `LineListener` interface can register to receive such notifications. To implement the `LineListener` interface, the object simply needs an update method that takes a `LineEvent` argument. To register this object as one of the line's listeners, you invoke the following `Line` method:

<a name="113717" id="113717"></a>

```

public void addLineListener(LineListener listener)

```

Whenever the line opens, closes, starts, or stops, it sends an `update` message to all its listeners. Your object can query the `LineEvent` that it receives. First you might invoke `LineEvent.getLine` to make sure the line that stopped is the one you care about. In the case we're discussing here, you want to know if the sound is finished, so you see whether the `LineEvent` is of type `STOP`. If it is, you might check the sound's current position, which is also stored in the `LineEvent` object, and compare it to the sound's length (if known) to see whether it reached the end and wasn't stopped by some other means (such as the user's clicking a Stop button, although you'd probably be able to determine that cause elsewhere in your code).

<a name="113721" id="113721"></a> Along the same lines, if you need to know when the line is opened, closed, or started, you use the same mechanism. `LineEvents` are generated by different kinds of lines, not just `Clips` and `SourceDataLines`. However, in the case of a `Port` you can't count on getting an event to learn about a line's open or closed state. For example, a `Port` might be initially open when it's created, so you don't invoke the `open` method and the `Port` doesn't ever generate an `OPEN` event. (See the previous discussion of 
[Selecting Input and Output Ports](accessing.html#113216).)

<a name="113725" id="113725"></a>

## Synchronizing Playback on Multiple Lines

<a name="113727" id="113727"></a> If you're playing back multiple tracks of audio simultaneously, you probably want to have them all start and stop at exactly the same time. Some mixers facilitate this behavior with their `synchronize` method, which lets you apply operations such as `open`, `close`, `start`, and `stop` to a group of data lines using a single command, instead of having to control each line individually. Furthermore, the degree of accuracy with which operations are applied to the lines is controllable.

<a name="113729" id="113729"></a> To find out whether a particular mixer offers this feature for a specified group of data lines, invoke the `Mixer` interface's `isSynchronizationSupported` method:

<a name="113731" id="113731"></a>

```

boolean isSynchronizationSupported(Line[] lines, boolean  maintainSync)

```

The first parameter specifies a group of specific data lines, and the second parameter indicates the accuracy with which synchronization must be maintained. If the second parameter is `true`, the query is asking whether the mixer is capable of maintaining sample-accurate precision in controlling the specified lines **at all times**; otherwise, precise synchronization is required only during start and stop operations, not throughout playback.

<a name="113736" id="113736"></a>

## Processing the Outgoing Audio

<a name="113738" id="113738"></a> Some source data lines have signal-processing controls, such as gain, pan, reverb, and sample-rate controls. Similar controls, especially gain controls, might be present on the output ports as well. For more information on how to determine whether a line has such controls, and how to use them if it does, see 
[Processing Audio with Controls](controls.html).
