
# Overview of the MIDI Package

<a name="118771" id="118771"></a> The 
[introduction](index.html) gave a glimpse into the MIDI capabilities of the Java Sound API. The discussion that follows provides a more detailed introduction to the Java Sound API's MIDI architecture, which is accessed through the `javax.sound.midi` package. Some basic features of MIDI itself are explained, as a refresher or introduction, to place the Java Sound API's MIDI features in context. It then goes on to discuss the Java Sound API's approach to MIDI, as a preparation for the programming tasks that are explained in subsequent sections. The following discussion of the MIDI API is divided into two main areas: data and devices.

## A MIDI Refresher: Wires and Files

<a name="118776" id="118776"></a> The Musical Instrument Digital Interface (MIDI) standard defines a communication protocol for electronic music devices, such as electronic keyboard instruments and personal computers. MIDI data can be transmitted over special cables during a live performance, and can also be stored in a standard type of file for later playback or editing.

<a name="118778" id="118778"></a> This section reviews some MIDI basics, without reference to the Java Sound API. The discussion is intended as a refresher for readers acquainted with MIDI, and as a brief introduction for those who are not, to provide background for the subsequent discussion of the Java Sound API's MIDI package. If you have a thorough understanding of MIDI, you can safely skip this section. Before writing substantial MIDI applications, programmers who are unfamiliar with MIDI will probably need a fuller description of MIDI than can be included in this tutorial. See the Complete MIDI 1.0 Detailed Specification, which is available only in hard copy from 
[http://www.midi.org](http://www.midi.org) (although you might find paraphrased or summarized versions on the Web).

<a name="118780" id="118780"></a> MIDI is both a hardware specification and a software specification. To understand MIDI's design, it helps to understand its history. MIDI was originally designed for passing musical events, such as key depressions, between electronic keyboard instruments such as synthesizers. Hardware devices known as sequencers stored sequences of notes that could control a synthesizer, allowing musical performances to be recorded and subsequently played back. Later, hardware interfaces were developed that connected MIDI instruments to a computer's serial port, allowing sequencers to be implemented in software. More recently, computer sound cards have incorporated hardware for MIDI I/O and for synthesizing musical sound. Today, many users of MIDI deal only with sound cards, never connecting to external MIDI devices. CPUs have become fast enough that synthesizers, too, can be implemented in software. A sound card is needed only for audio I/O and, in some applications, for communicating with external MIDI devices.

<a name="119058" id="119058"></a> The brief hardware portion of the MIDI specification prescribes the pinouts for MIDI cables and the jacks into which these cables are plugged. This portion need not concern us. Because devices that originally required hardware, such as sequencers and synthesizers, are now implementable in software, perhaps the only reason for most programmers to know anything about MIDI hardware devices is simply to understand the metaphors in MIDI. However, external MIDI hardware devices are still essential for some important music applications, and so the Java Sound API supports input and output of MIDI data.

<a name="118784" id="118784"></a> The software portion of the MIDI specification is extensive. This portion concerns the structure of MIDI data and how devices such as synthesizers should respond to that data. It is important to understand that MIDI data can be **streamed** or **sequenced**. This duality reflects two different parts of the Complete MIDI 1.0 Detailed Specification:

- <a name="118786" id="118786"></a>MIDI 1.0
- <a name="118787" id="118787"></a>Standard MIDI Files

<a name="118789" id="118789"></a>We'll explain what's meant by streaming and sequencing by examining the purpose of each of these two parts of the MIDI specification.

<a name="118791" id="118791"></a>

## Streaming Data in the MIDI Wire Protocol

<a name="118793" id="118793"></a> The first of these two parts of the MIDI specification describes what is known informally as "MIDI wire protocol." MIDI wire protocol, which is the original MIDI protocol, is based on the assumption that the MIDI data is being sent over a MIDI cable (the "wire"). The cable transmits digital data from one MIDI device to another. Each of the MIDI devices might be a musical instrument or a similar device, or it might be a general-purpose computer equipped with a MIDI-capable sound card or a MIDI-to-serial-port interface.

<a name="118795" id="118795"></a> MIDI data, as defined by MIDI wire protocol, is organized into messages. The different kinds of message are distinguished by the first byte in the message, known as the **status byte**. (Status bytes are the only bytes that have the highest-order bit set to 1.) The bytes that follow the status byte in a message are known as **data bytes**. Certain MIDI messages, known as **channel** messages, have a status byte that contains four bits to specify the kind of channel message and another four bits to specify the channel number. There are therefore 16 MIDI channels; devices that receive MIDI messages can be set to respond to channel messages on all or only one of these virtual channels. Often each MIDI channel (which shouldn't be confused with a channel of audio) is used to send the notes for a different instrument. As an example, two common channel messages are Note On and Note Off, which start a note sounding and then stop it, respectively. These two messages each take two data bytes: the first specifies the note's pitch and the second its "velocity" (how fast the key is depressed or released, assuming a keyboard instrument is playing the note).

<a name="118799" id="118799"></a> MIDI wire protocol defines a streaming model for MIDI data. A central feature of this protocol is that the bytes of MIDI data are delivered in real time&#226;&#128;&#148;in other words, they are streamed. The data itself contains no timing information; each event is processed as it's received, and it's assumed that it arrives at the correct time. That model is fine if the notes are being generated by a live musician, but it's insufficient if you want to store the notes for later playback, or if you want to compose them out of real time. This limitation is understandable when you realize that MIDI was originally designed for musical performance, as a way for a keyboard musician to control more than one synthesizer, back in the days before many musicians used computers. (The first version of the specification was released in 1984.)

<a name="119349" id="119349"></a>

## Sequenced Data in Standard MIDI Files

<a name="119350" id="119350"></a> The Standard MIDI Files part of the MIDI specification addresses the timing limitation in MIDI wire protocol. A standard MIDI file is a digital file that contains MIDI **events**. An event is simply a MIDI message, as defined in the MIDI wire protocol, but with an additional piece of information that specifies the event's timing. (There are also some events that don't correspond to MIDI wire protocol messages, as we'll see in the next section.) The additional timing information is a series of bytes that indicates when to perform the operation described by the message. In other words, a standard MIDI file specifies not just which notes to play, but exactly when to play each of them. It's a bit like a musical score.

<a name="118806" id="118806"></a> The information in a standard MIDI file is referred to as a **sequence**. A standard MIDI file contains one or more **tracks**. Each track typically contains the notes that a single instrument would play if the music were performed by live musicians. A sequencer is a software or hardware device that can read a sequence and deliver the MIDI messages contained in it at the right time. A sequencer is a bit like an orchestra conductor: it has the information for all the notes, including their timings, and it tells some other entity when to perform the notes.

<a name="118810" id="118810"></a>

## The Java Sound API's Representation of MIDI Data

<a name="118812" id="118812"></a> Now that we've sketched the MIDI specification's approach to streamed and sequenced musical data, let's examine how the Java Sound API represents that data.

<a name="119522" id="119522"></a>

### MIDI Messages

<a name="119523" id="119523"></a> 
[`MidiMessage`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiMessage.html) is an abstract class that represents a "raw" MIDI message. A "raw" MIDI message is usually a message defined by the MIDI wire protocol. It can also be one of the events defined by the Standard MIDI Files specification, but without the event's timing information. There are three categories of raw MIDI message, represented in the Java Sound API by these three respective `MidiMessage` subclasses:

<li><a name="118819" id="118819"></a> 
[`ShortMessages`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/ShortMessage.html) are the most common messages and have at most two data bytes following the status byte. The channel messages, such as Note On and Note Off, are all short messages, as are some other messages.</li>
<li><a name="118820" id="118820"></a> 
[`SysexMessages`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/SysexMessage.html) contain **system-exclusive** MIDI messages. They may have many bytes, and generally contain manufacturer-specific instructions.</li>
<li><a name="118821" id="118821"></a> 
[`MetaMessages`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MetaMessage.html) occur in MIDI files, but not in MIDI wire protocol. Meta messages contain data, such as lyrics or tempo settings, that might be useful to sequencers but that are usually meaningless for synthesizers.</li>

<a name="118823" id="118823"></a>

### MIDI Events

<a name="119393" id="119393"></a> As we've seen, standard MIDI files contain events that are wrappers for "raw" MIDI messages along with timing information. An instance of the Java Sound API's 
[`MidiEvent`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiEvent.html) class represents an event such as might be stored in a standard MIDI file.

<a name="118827" id="118827"></a> The API for `MidiEvent` includes methods to set and get the event's timing value. There's also a method to retrieve its embedded raw MIDI message, which is an instance of a subclass of `MidiMessage`, discussed next. (The embedded raw MIDI message can be set only when constructing the `MidiEvent`.)

<a name="118829" id="118829"></a>

### Sequences and Tracks

<a name="118831" id="118831"></a> As mentioned earlier, a standard MIDI file stores events that are arranged into tracks. Usually the file represents one musical composition, and usually each track represents a part such as might have been played by a single instrumentalist. Each note that the instrumentalist plays is represented by at least two events: a Note On that starts the note, and a Note Off that ends it. The track may also contain events that don't correspond to notes, such as meta-events (which were mentioned above).

<a name="118833" id="118833"></a> The Java Sound API organizes MIDI data in a three-part hierarchy:

<li><a name="118834" id="118834"></a> 
[`Sequence`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Sequence.html)</li>
<li><a name="118835" id="118835"></a> 
[`Track`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Track.html)</li>
<li><a name="118836" id="118836"></a> 
[`MidiEvent`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiEvent.html)<a name="118837" id="118837"></a>
</li>

A `Track` is a collection of `MidiEvents`, and a `Sequence` is a collection of `Tracks`. This hierarchy reflects the files, tracks, and events of the Standard MIDI Files specification. (Note: this is a hierarchy in terms of containment and ownership; it's **not** a class hierarchy in terms of inheritance. Each of these three classes inherits directly from `java.lang.Object`.)

<a name="118839" id="118839"></a> `Sequences` can be read from MIDI files, or created from scratch and edited by adding `Tracks` to the `Sequence` (or removing them). Similarly, `MidiEvents` can be added to or removed from the tracks in the sequence.

<a name="118842" id="118842"></a>

## The Java Sound API's Representation of MIDI Devices

<a name="118844" id="118844"></a> The previous section explained how MIDI messages are represented in the Java Sound API. However, MIDI messages don't exist in a vacuum. They're typically sent from one device to another. A program that uses the Java Sound API can generate MIDI messages from scratch, but more often the messages are instead created by a software device, such as a sequencer, or received from outside the computer through a MIDI input port. Such a device usually sends these messages to another device, such as a synthesizer or a MIDI output port.

<a name="118846" id="118846"></a>

### The MidiDevice Interface

<a name="118848" id="118848"></a> In the world of external MIDI hardware devices, many devices can transmit MIDI messages to other devices and also receive messages from other devices. Similarly, in the Java Sound API, software objects that implement the 
[`MidiDevice`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiDevice.html) interface can transmit and receive messages. Such an object can be implemented purely in software, or it can serve as an interface to hardware such as a sound card's MIDI capabilities. The base `MidiDevice` interface provides all the functionality generally required by a MIDI input or output port. Synthesizers and sequencers, however, further implement one of the subinterfaces of `MidiDevice`: 
[`Synthesizer`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Synthesizer.html) or 
[`Sequencer`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Sequencer.html) , respectively.

<a name="118850" id="118850"></a> The `MidiDevice` interface includes an API for opening and closing a device. It also includes an inner class called `MidiDevice.Info` that provides textual descriptions of the device, including its name, vendor, and version. If you've read the sampled-audio portion of this tutorial, this API will probably sound familiar, because its design is similar to that of the `javax.sampled.Mixer` interface, which represents an audio device and which has an analogous inner class, `Mixer.Info`.

<a name="118852" id="118852"></a>

### Transmitters and Receivers

<a name="118854" id="118854"></a> Most MIDI devices are capable of sending `MidiMessages`, receiving them, or both. The way a device sends data is via one or more transmitter objects that it "owns." Similarly, the way a device receives data is via one or more of its receiver objects. The transmitter objects implement the 
[`Transmitter`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Transmitter.html) interface, and the receivers implement the 
[`Receiver`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Receiver.html) interface.

<a name="118856" id="118856"></a> Each transmitter can be connected to only one receiver at a time, and vice versa. A device that sends its MIDI messages to multiple other devices simultaneously does so by having multiple transmitters, each connected to a receiver of a different device. Similarly, a device that can receive MIDI messages from more than one source at a time must do so via multiple receivers.

<a name="118858" id="118858"></a>

### Sequencers

<a name="118860" id="118860"></a> A sequencer is a device for capturing and playing back sequences of MIDI events. It has transmitters, because it typically sends the MIDI messages stored in the sequence to another device, such as a synthesizer or MIDI output port. It also has receivers, because it can capture MIDI messages and store them in a sequence. To its superinterface, `MidiDevice`, `Sequencer` adds methods for basic MIDI sequencing operations. A sequencer can load a sequence from a MIDI file, query and set the sequence's tempo, and synchronize other devices to it. An application program can register an object to be notified when the sequencer processes certain kinds of events.

<a name="118862" id="118862"></a>

### Synthesizers

<a name="119601" id="119601"></a> A `Synthesizer` is a device for generating sound. It's the only object in the `javax.sound.midi` package that produces audio data. A synthesizer device controls a set of MIDI channel objects &#8212; typically 16 of them, since the MIDI specification calls for 16 MIDI channels. These MIDI channel objects are instances of a class that implements the 
[`MidiChannel`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiChannel.html) interface, whose methods represent the MIDI specification's "channel voice messages" and "channel mode messages."

<a name="118866" id="118866"></a> An application program can generate sound by directly invoking methods of a synthesizer's MIDI channel objects. More commonly, though, a synthesizer generates sound in response to messages sent to one or more of its receivers. These messages might be sent by a sequencer or MIDI input port, for example. The synthesizer parses each message that its receivers get, and usually dispatches a corresponding command (such as `noteOn` or `controlChange`) to one of its `MidiChannel` objects, according to the MIDI channel number specified in the event.

<a name="118868" id="118868"></a> The `MidiChannel` uses the note information in these messages to synthesize music. For example, a `noteOn` message specifies the note's pitch and "velocity" (volume). However, the note information is insufficient; the synthesizer also requires precise instructions on how to create the audio signal for each note. These instructions are represented by an 
[`Instrument`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/Instrument.html). Each `Instrument` typically emulates a different real-world musical instrument or sound effect. The `Instruments` might come as presets with the synthesizer, or they might be loaded from soundbank files. In the synthesizer, the `Instruments` are arranged by bank number (these can be thought of as rows) and program number (columns).

<a name="118870" id="118870"></a> This section has provided a background for understanding MIDI data, and it has introduced some of the important interfaces and classes related to MIDI in the Java Sound API. Subsequent sections show how you can access and use these objects in your application programs.
