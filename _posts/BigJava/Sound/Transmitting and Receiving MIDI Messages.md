
# Transmitting and Receiving MIDI Messages

<a name="120461" id="120461"></a>

## Understanding Devices, Transmitters, and Receivers

The Java Sound API specifies a message-routing architecture for MIDI data that's flexible and easy to use, once you understand how it works. The system is based on a module-connection design: distinct modules, each of which performs a specific task, can be interconnected (networked), enabling data to flow from one module to another.

<a name="121778" id="121778"></a> The base module in the Java Sound API's messaging system is the 
[`MidiDevice`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiDevice.html) interface. `MidiDevices` include sequencers (which record, play, load, and edit sequences of time-stamped MIDI messages), synthesizers (which generate sounds when triggered by MIDI messages), and MIDI input and output ports, through which data comes from and goes to external MIDI devices. The functionality typically required of MIDI ports is described by the base `MidiDevice` interface. The 
[`Sequencer`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Sequencer.html) and 
[`Synthesizer`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Synthesizer.html) interfaces extend the `MidiDevice` interface to describe the additional functionality characteristic of MIDI sequencers and synthesizers, respectively. Concrete classes that function as sequencers or synthesizers should implement these interfaces.

<a name="121816" id="121816"></a> A `MidiDevice` typically owns one or more ancillary objects that implement the `Receiver` or `Transmitter` interfaces. These interfaces represent the "plugs" or "portals" that connect devices together, permitting data to flow into and out of them. By connecting a `Transmitter` of one `MidiDevice` to a `Receiver` of another, you can create a network of modules in which data flows from one to another.

<a name="121833" id="121833"></a> The `MidiDevice` interface includes methods for determining how many transmitter and receiver objects the device can support concurrently, and other methods for accessing those objects. A MIDI output port normally has at least one `Receiver` through which the outgoing messages may be received; similarly, a synthesizer normally responds to messages sent to its `Receiver` or `Receivers`. A MIDI input port normally has at least one `Transmitter`, which propagates the incoming messages. A full-featured sequencer supports both `Receivers`, which receive messages during recording, and `Transmitters`, which send messages during playback.

<a name="120470" id="120470"></a> The `Transmitter` interface includes methods for setting and querying the receivers to which the transmitter sends its `MidiMessages`. Setting the receiver establishes the connection between the two. The `Receiver` interface contains a method that sends a `MidiMessage` to the receiver. Typically, this method is invoked by a `Transmitter`. Both the `Transmitter` and `Receiver` interfaces include a `close` method that frees up a previously connected transmitter or receiver, making it available for a different connection.

<a name="120474" id="120474"></a> We'll now examine how to use transmitters and receivers. Before getting to the typical case of connecting two devices (such as hooking a sequencer to a synthesizer), we'll examine the simpler case where you send a MIDI message directly from your application program to a device. Studying this simple scenario should make it easier to understand how the Java Sound API arranges for sending MIDI messages between two devices.

<a name="sending" id="sending"></a>

## Sending a Message to a Receiver without Using a Transmitter

<a name="121187" id="121187"></a> Let's say you want to create a MIDI message from scratch and then send it to some receiver. You can create a new, blank `ShortMessage` and then fill it with MIDI data using the following `ShortMessage` method:

```

void setMessage(int command, int channel, int data1,
         int data2) 

```

Once you have a message ready to send, you can send it to a `Receiver` object, using this `Receiver` method:

```

void send(MidiMessage message, long timeStamp)

```

The time-stamp argument will be explained momentarily. For now, we'll just mention that its value can be set to -1 if you don't care about specifying a precise time. In this case, the device receiving the message will try to respond to the message as soon as possible.

<a name="121753" id="121753"></a> An application program can obtain a receiver for a `MidiDevice` by invoking the device's `getReceiver` method. If the device can't provide a receiver to the program (typically because all the device's receivers are already in use), a `MidiUnavailableException` is thrown. Otherwise, the receiver returned from this method is available for immediate use by the program. When the program has finished using the receiver, it should call the receiver's `close` method. If the program attempts to invoke methods on a receiver after calling `close`, an `IllegalStateException` may be thrown.

<a name="121858" id="121858"></a> As a concrete simple example of sending a message without using a transmitter, let's send a Note On message to the default receiver, which is typically associated with a device such as the MIDI output port or a synthesizer. We do this by creating a suitable `ShortMessage` and passing it as an argument to `Receiver's` `send` method:

```

  ShortMessage myMsg = new ShortMessage();
  // Start playing the note Middle C (60), 
  // moderately loud (velocity = 93).
  myMsg.setMessage(ShortMessage.NOTE_ON, 0, 60, 93);
  long timeStamp = -1;
  Receiver       rcvr = MidiSystem.getReceiver();
  rcvr.send(myMsg, timeStamp);

```

This code uses a static integer field of `ShortMessage`, namely, `NOTE_ON`, for use as the MIDI message's status byte. The other parts of the MIDI message are given explicit numeric values as arguments to the `setMessage` method. The zero indicates that the note is to be played using MIDI channel number 1; the 60 indicates the note Middle C; and the 93 is an arbitrary key-down velocity value, which typically indicates that the synthesizer that eventually plays the note should play it somewhat loudly. (The MIDI specification leaves the exact interpretation of velocity up to the synthesizer's implementation of its current instrument.) This MIDI message is then sent to the receiver with a time stamp of -1. We now need to examine exactly what the time stamp parameter means, which is the subject of the next section. <a name="understanding_time" id="understanding_time"></a>

## Understanding Time Stamps

<a name="120509" id="120509"></a> As you already know, the MIDI specification has different parts. One part describes MIDI "wire" protocol (messages sent between devices in real time), and another part describes Standard MIDI Files (messages stored as events in "sequences"). In the latter part of the specification, each event stored in a standard MIDI file is tagged with a timing value that indicates when that event should be played. By contrast, messages in MIDI wire protocol are always supposed to be processed immediately, as soon as they're received by a device, so they have no accompanying timing values.

<a name="120511" id="120511"></a> The Java Sound API adds an additional twist. It comes as no surprise that timing values are present in the `MidiEvent` objects that are stored in sequences (as might be read from a MIDI file), just as in the Standard MIDI Files specification. But in the Java Sound API, even the messages sent between devices&#226;&#128;&#148;in other words, the messages that correspond to MIDI wire protocol&#226;&#128;&#148;can be given timing values, known as **time stamps**. It is these time stamps that concern us here.

### Time Stamps on Messages Sent to Devices

<a name="120519" id="120519"></a> The time stamp that can optionally accompany messages sent between devices in the Java Sound API is quite different from the timing values in a standard MIDI file. The timing values in a MIDI file are often based on musical concepts such as beats and tempo, and each event's timing measures the time elapsed since the previous event. In contrast, the time stamp on a message sent to a device's `Receiver` object always measures absolute time in microseconds. Specifically, it measures the number of microseconds elapsed since the device that owns the receiver was opened.

<a name="120521" id="120521"></a> This kind of time stamp is designed to help compensate for latencies introduced by the operating system or by the application program. It's important to realize that these time stamps are used for minor adjustments to timing, not to implement complex queues that can schedule events at completely arbitrary times (as `MidiEvent` timing values do).

<a name="122050" id="122050"></a> The time stamp on a message sent to a device (through a `Receiver`) can provide precise timing information to the device. The device might use this information when it processes the message. For example, it might adjust the event's timing by a few milliseconds to match the information in the time stamp. On the other hand, not all devices support time stamps, so the device might completely ignore the message's time stamp.

<a name="122051" id="122051"></a> Even if a device supports time stamps, it might not schedule the event for exactly the time that you requested. You can't expect to send a message whose time stamp is very far in the future and have the device handle it as you intended, and you certainly can't expect a device to correctly schedule a message whose time stamp is in the past! It's up to the device to decide how to handle time stamps that are too far off in the future or are in the past. The sender doesn't know what the device considers to be too far off, or whether the device had any problem with the time stamp. This ignorance mimics the behavior of external MIDI hardware devices, which send messages without ever knowing whether they were received correctly. (MIDI wire protocol is unidirectional.)

<a name="120527" id="120527"></a> Some devices send time-stamped messages (via a `Transmitter`). For example, the messages sent by a MIDI input port might be stamped with the time the incoming message arrived at the port. On some systems, the event-handling mechanisms cause a certain amount of timing precision to be lost during subsequent processing of the message. The message's time stamp allows the original timing information to be preserved.

<a name="122114" id="122114"></a> To learn whether a device supports time stamps, invoke the following method of `MidiDevice`:

```

    long getMicrosecondPosition()

```

This method returns -1 if the device ignores time stamps. Otherwise, it returns the device's current notion of time, which you as the sender can use as an offset when determining the time stamps for messages you subsequently send. For example, if you want to send a message with a time stamp for five milliseconds in the future, you can get the device's current position in microseconds, add 5000 microseconds, and use that as the time stamp. Keep in mind that the `MidiDevice's` notion of time always places time zero at the time the device was opened.

<a name="120533" id="120533"></a> Now, with all that explanation of time stamps as a background, let's return to the `send` method of `Receiver`:

```

void send(MidiMessage message, long timeStamp)

```

The `timeStamp` argument is expressed in microseconds, according to the receiving device's notion of time. If the device doesn't support time stamps, it simply ignores the `timeStamp` argument. You aren't required to time-stamp the messages you send to a receiver. You can use -1 for the `timeStamp` argument to indicate that you don't care about adjusting the exact timing; you're just leaving it up to the receiving device to process the message as soon as it can. However, it's not advisable to send -1 with some messages and explicit time stamps with other messages sent to the same receiver. Doing so is likely to cause irregularities in the resultant timing. <a name="122132" id="122132"></a>

## Connecting Transmitters to Receivers

<a name="120542" id="120542"></a> We've seen how you can send a MIDI message directly to a receiver, without using a transmitter. Now let's look at the more common case, where you aren't creating MIDI messages from scratch, but are simply connecting devices together so that one of them can send MIDI messages to the other.

<a name="120544" id="120544"></a>

### Connecting to a Single Device

<a name="120546" id="120546"></a> The specific case we'll take as our first example is connecting a sequencer to a synthesizer. After this connection is made, starting the sequencer running will cause the synthesizer to generate audio from the events in the sequencer's current sequence. For now, we'll ignore the process of loading a sequence from a MIDI file into the sequencer. Also, we won't go into the mechanism of playing the sequence. Loading and playing sequences is discussed in detail in 
[Playing, Recording, and Editing MIDI Sequences](MIDI-seq-intro.html). Loading instruments into the synthesizer is discussed in 
[Synthesizing Sound](MIDI-synth.html). For now, all we're interested in is how to make the connection between the sequencer and the synthesizer. This will serve as an illustration of the more general process of connecting one device's transmitter to another device's receiver.

<a name="121529" id="121529"></a> For simplicity, we'll use the default sequencer and the default synthesizer.

```

    Sequencer           seq;
    Transmitter         seqTrans;
    Synthesizer         synth;
    Receiver         synthRcvr;
    try {
          seq     = MidiSystem.getSequencer();
          seqTrans = seq.getTransmitter();
          synth   = MidiSystem.getSynthesizer();
          synthRcvr = synth.getReceiver(); 
          seqTrans.setReceiver(synthRcvr);      
    } catch (MidiUnavailableException e) {
          // handle or throw exception
    }

```

An implementation might actually have a single object that serves as both the default sequencer and the default synthesizer. In other words, the implementation might use a class that implements both the `Sequencer` interface and the `Synthesizer` interface. In that case, it probably wouldn't be necessary to make the explicit connection that we did in the code above. For portability, though, it's safer not to assume such a configuration. If desired, you can test for this condition, of course:

```

if (seq instanceof Synthesizer)

```

<a name="122187" id="122187"></a> although the explicit connection above should work in any case.

<a name="120569" id="120569"></a>

### Connecting to More than One Device

<a name="121563" id="121563"></a> The previous code example illustrated a one-to-one connection between a transmitter and a receiver. But, what if you need to send the same MIDI message to multiple receivers? For example, suppose you want to capture MIDI data from an external device to drive the internal synthesizer while simultaneously recording the data to a sequence. This form of connection, sometimes referred to as "fan out" or as a "splitter," is straightforward. The following statements show how to create a fan-out connection, through which the MIDI messages arriving at the MIDI input port are sent to both a `Synthesizer` object and a `Sequencer` object. We assume you've already obtained and opened the three devices: the input port, sequencer, and synthesizer. (To obtain the input port, you'll need to iterate over all the items returned by `MidiSystem.getMidiDeviceInfo`.)

```

    Synthesizer  synth;
    Sequencer    seq;
    MidiDevice   inputPort;
    // [obtain and open the three devices...]
    Transmitter   inPortTrans1, inPortTrans2;
    Receiver            synthRcvr;
    Receiver            seqRcvr;
    try {
          inPortTrans1 = inputPort.getTransmitter();
          synthRcvr = synth.getReceiver(); 
          inPortTrans1.setReceiver(synthRcvr);
          inPortTrans2 = inputPort.getTransmitter();
          seqRcvr = seq.getReceiver(); 
          inPortTrans2.setReceiver(seqRcvr);
    } catch (MidiUnavailableException e) {
          // handle or throw exception
    }

```

This code introduces a dual invocation of the `MidiDevice.getTransmitter` method, assigning the results to `inPortTrans1` and `inPortTrans2`. As mentioned earlier, a device can own multiple transmitters and receivers. Each time `MidiDevice.getTransmitter()` is invoked for a given device, another transmitter is returned, until no more are available, at which time an exception will be thrown.

<a name="122228" id="122228"></a> To learn how many transmitters and receivers a device supports, you can use the following `MidiDevice` method:

```

    int getMaxTransmitters()
    int `getMaxReceivers`()

```

These methods return the total number owned by the device, not the number currently available.

<a name="120602" id="120602"></a> A transmitter can transmit MIDI messages to only one receiver at a time. (Every time you call `Transmitter's setReceiver` method, the existing `Receiver`, if any, is replaced by the newly specified one. You can tell whether the transmitter currently has a receiver by invoking `Transmitter.getReceiver`.) However, if a device has multiple transmitters, it can send data to more than one device at a time, by connecting each transmitter to a different receiver, as we saw in the case of the input port above.

<a name="122286" id="122286"></a> Similarly, a device can use its multiple receivers to receive from more than one device at a time. The multiple-receiver code that's required is straightforward, being directly analogous to the multiple-transmitter code above. It's also possible for a single receiver to receive messages from more than one transmitter at a time.

<a name="120605" id="120605"></a>

### Closing Connections

<a name="120607" id="120607"></a> Once you're done with a connection, you can free up its resources by invoking the `close` method for each transmitter and receiver that you've obtained. The `Transmitter` and `Receiver` interfaces each have a `close` method. Note that invoking `Transmitter.setReceiver` doesn't close the transmitter's current receiver. The receiver is left open, and it can still receive messages from any other transmitter that's connected to it.

<a name="122314" id="122314"></a> If you're also done with the devices, you can similarly make them available to other application programs by invoking `MidiDevice.close()`. Closing a device automatically closes all its transmitters and receivers.
