---
title: Using Sequencer Methods
author: lijiabao
date: 2020-12-06 01:47:55.110965500 +0800
category: Sound
categories: Sound
tags: Sound
---

# Using Sequencer Methods

The 
[`Sequencer`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Sequencer.html) interface provides methods in several categories:

- <a name="124593" id="124593"></a>Methods to load sequence data from a MIDI file or a `Sequence` object, and to save the currently loaded sequence data to a MIDI file.
- <a name="124594" id="124594"></a>Methods analogous to the transport functions of a tape recorder, for stopping and starting playback and recording, enabling and disabling recording on specific tracks, and shuttling the current playback or recording position in a `Sequence`.
- <a name="124595" id="124595"></a>Advanced methods for querying and setting the synchronization and timing parameters of the object. A `Sequencer` may play at different tempos, with some `Tracks` muted, and in various synchronization states with other objects.
- <a name="124596" id="124596"></a>Advanced methods for registering "listener" objects that are notified when the `Sequencer` processes certain kinds of MIDI events.

<a name="124597" id="124597"></a> Regardless of which `Sequencer` methods you'll invoke, the first step is to obtain a `Sequencer` device from the system and reserve it for your program's use.

<a name="124598" id="124598"></a>

## Obtaining a Sequencer

<a name="124599" id="124599"></a> An application program doesn't instantiate a `Sequencer`; after all, `Sequencer` is just an interface. Instead, like all devices in the Java Sound API's MIDI package, a `Sequencer` is accessed through the static `MidiSystem` object. As previously mentioned in 
[Accessing MIDI System Resources](accessing-MIDI.html), the following `MidiSystem` method can be used to obtain the default `Sequencer`:

```

static Sequencer getSequencer()

```

<a name="124604" id="124604"></a> The following code fragment obtains the default `Sequencer`, acquires any system resources it needs, and makes it operational:

```

Sequencer sequencer;
// Get default sequencer.
sequencer = MidiSystem.getSequencer(); 
if (sequencer == null) {
    // Error -- sequencer device is not supported.
    // Inform user and return...
} else {
    // Acquire resources and make operational.
    sequencer.open();
}

```

<a name="124615" id="124615"></a> The invocation of `open` reserves the sequencer device for your program's use. It doesn't make much sense to imagine sharing a sequencer, because it can play only one sequence at a time. When you're done using the sequencer, you can make it available to other programs by invoking `close`.

<a name="124616" id="124616"></a> Non-default sequencers can be obtained as described in 
[Accessing MIDI System Resources](accessing-MIDI.html).

<a name="124620" id="124620"></a>

## Loading a Sequence

<a name="124621" id="124621"></a> Having obtained a sequencer from the system and reserved it, you then need load the data that the sequencer should play. There are three typical ways of accomplishing this:

- <a name="124622" id="124622"></a>Reading the sequence data from a MIDI file
- <a name="124623" id="124623"></a>Recording it in real time by receiving MIDI messages from another device, such as a MIDI input port
- <a name="124624" id="124624"></a>Building it programmatically "from scratch" by adding tracks to an empty sequence and adding `MidiEvent` objects to those tracks

[Recording and Saving Sequences](#124654) and [Editing a Sequence](#124674), respectively.) This first way actually encompasses two slightly different approaches. One approach is to feed MIDI file data to an `InputStream` that you then read directly to the sequencer by means of `Sequencer.setSequence(InputStream)`. With this approach, you don't explicitly create a `Sequence` object. In fact, the `Sequencer` implementation might not even create a `Sequence` behind the scenes, because some sequencers have a built-in mechanism for handling data directly from a file.

<a name="124632" id="124632"></a> The other approach is to create a `Sequence` explicitly. You'll need to use this approach if you're going to edit the sequence data before playing it. With this approach, you invoke `MidiSystem's` overloaded method `getSequence`. The method is able to get the sequence from an `InputStream`, a `File`, or a `URL`. The method returns a `Sequence` object that can then be loaded into a `Sequencer` for playback. Expanding on the previous code excerpt, here's an example of obtaining a `Sequence` object from a `File` and loading it into our `sequencer`:

```

try {
    File myMidiFile = new File("seq1.mid");
    // Construct a Sequence object, and
    // load it into my sequencer.
    Sequence mySeq = MidiSystem.getSequence(myMidiFile);
    sequencer.setSequence(mySeq);
} catch (Exception e) {
   // Handle error and/or return
}

```

<a name="124642" id="124642"></a> Like `MidiSystem's` `getSequence` method, `setSequence` may throw an `InvalidMidiDataException`&#226;&#128;&#148;and, in the case of the `InputStream` variant, an `IOException`&#226;&#128;&#148;if it runs into any trouble.

<a name="124643" id="124643"></a>

## Playing a Sequence

<a name="124644" id="124644"></a> Starting and stopping a `Sequencer` is accomplished using the following methods:

```

    void start()

```

<a name="124646" id="124646"></a> and

```

    void stop()

```

<a name="124648" id="124648"></a> The `Sequencer.start` method begins playback of the sequence. Note that playback starts at the current position in a sequence. Loading an existing sequence using the `setSequence` method, described above, initializes the sequencer's current position to the very beginning of the sequence. The `stop` method stops the sequencer, but it does not automatically rewind the current `Sequence`. Starting a stopped `Sequence` without resetting the position simply resumes playback of the sequence from the current position. In this case, the `stop` method has served as a pause operation. However, there are various `Sequencer` methods for setting the current sequence position to an arbitrary value before playback is started. (We'll discuss these methods below.)

<a name="124649" id="124649"></a> As mentioned earlier, a `Sequencer` typically has one or more `Transmitter` objects, through which it sends `MidiMessages` to a `Receiver`. It is through these `Transmitters` that a `Sequencer` plays the `Sequence`, by emitting appropriately timed `MidiMessages` that correspond to the `MidiEvents` contained in the current `Sequence`. Therefore, part of the setup procedure for playing back a `Sequence` is to invoke the `setReceiver` method on the `Sequencer's` `Transmitter` object, in effect wiring its output to the device that will make use of the played-back data. For more details on `Transmitters` and `Receivers`, refer back to 
[Transmitting and Receiving MIDI Messages](MIDI-messages.html).

<a name="124654" id="124654"></a>

## Recording and Saving Sequences

<a name="124655" id="124655"></a> To capture MIDI data to a `Sequence`, and subsequently to a file, you need to perform some additional steps beyond those described above. The following outline shows the steps necessary to set up for recording to a `Track` in a `Sequence`:

1. <a name="124656" id="124656"></a>Use `MidiSystem.getSequencer` to get a new sequencer to use for recording, as above.
1. <a name="124657" id="124657"></a>Set up the "wiring" of the MIDI connections. The object that is transmitting the MIDI data to be recorded should be configured, through its `setReceiver` method, to send data to a `Receiver` associated with the recording `Sequencer`.
<li><a name="124658" id="124658"></a>Create a new `Sequence` object, which will store the recorded data. When you create the `Sequence` object, you must specify the global timing information for the sequence. For example:
<pre><code>
      Sequence mySeq;
      try{
          mySeq = new Sequence(Sequence.PPQ, 10);
      } catch (Exception ex) { 
          ex.printStackTrace(); 
      }
</code></pre>
<a name="124666" id="124666"></a> The constructor for `Sequence` takes as arguments a `divisionType` and a timing resolution. The `divisionType` argument specifies the units of the resolution argument. In this case, we've specified that the timing resolution of the `Sequence` we're creating will be 10 pulses per quarter note. An additional optional argument to the `Sequence` constructor is a number of tracks argument, which would cause the initial sequence to begin with the specified number of (initially empty) `Tracks`. Otherwise the `Sequence` will be created with no initial `Tracks`; they can be added later as needed.</li>
1. <a name="124667" id="124667"></a>Create an empty `Track` in the `Sequence`, with `Sequence.createTrack`. This step is unnecessary if the `Sequence` was created with initial `Tracks`.
1. <a name="124668" id="124668"></a>Using `Sequencer.setSequence`, select our new `Sequence` to receive the recording. The `setSequence` method ties together an existing `Sequence` with the `Sequencer`, which is somewhat analogous to loading a tape onto a tape recorder.
1. <a name="124669" id="124669"></a>Invoke `Sequencer.recordEnable` for each `Track` to be recorded. If necessary, get a reference to the available `Tracks` in the `Sequence` by invoking `Sequence.getTracks`.
1. <a name="124670" id="124670"></a>Invoke `startRecording` on the `Sequencer`.
1. <a name="124671" id="124671"></a>When done recording, invoke `Sequencer.stop` or `Sequencer.stopRecording`.
1. <a name="124672" id="124672"></a>Save the recorded `Sequence` to a MIDI file with `MidiSystem.write`. The `write` method of `MidiSystem` takes a `Sequence` as one of its arguments, and will write that `Sequence` to a stream or file.

<a name="124674" id="124674"></a>

## Editing a Sequence

<a name="124675" id="124675"></a> Many application programs allow a sequence to be created by loading it from a file, and quite a few also allow a sequence to be created by capturing it from live MIDI input (that is, recording). Some programs, however, will need to create MIDI sequences from scratch, whether programmatically or in response to user input. Full-featured sequencer programs permit the user to manually construct new sequences, as well as to edit existing ones.

<a name="124676" id="124676"></a> These data-editing operations are achieved in the Java Sound API not by `Sequencer` methods, but by methods of the data objects themselves: `Sequence`, `Track`, and `MidiEvent`. You can create an empty sequence using one of the `Sequence` constructors, and then add tracks to it by invoking the following `Sequence` method:

```

    Track createTrack() 

```

If your program allows the user to edit sequences, you'll need this `Sequence` method to remove tracks:

```

    boolean deleteTrack(Track track) 

```

<a name="124680" id="124680"></a> Once the sequence contains tracks, you can modify the contents of the tracks by invoking methods of the `Track` class. The `MidiEvents` contained in the `Track` are stored as a `java.util.Vector` in the `Track` object, and `Track` provides a set of methods for accessing, adding, and removing the events in the list. The methods `add` and `remove` are fairly self-explanatory, adding or removing a specified `MidiEvent` from a `Track`. A `get` method is provided, which takes an index into the `Track's` event list and returns the `MidiEvent` stored there. In addition, there are `size` and `tick` methods, which respectively return the number of `MidiEvents` in the track, and the track's duration, expressed as a total number of `Ticks`.

<a name="124681" id="124681"></a> To create a new event before adding it to the track, you'll of course use the `MidiEvent` constructor. To specify or modify the MIDI message embedded in the event, you can invoke the `setMessage` method of the appropriate `MidiMessage` subclass (`ShortMessage`, `SysexMessage`, or `MetaMessage`). To modify the time that the event should occur, invoke `MidiEvent.setTick`.

<a name="124682" id="124682"></a> In combination, these low-level methods provide the basis for the editing functionality needed by a full-featured sequencer program.

<a name="124684" id="124684"></a>
