---
title: Using Advanced Sequencer Features
author: lijiabao
date: 2020-12-06 01:47:57.140459000 +0800
category: Sound
categories: Sound
tags: Sound
---

# Using Advanced Sequencer Features

<a name="124685" id="124685"></a> So far, we've focused on simple playback and recording of MIDI data. This section will briefly describe some of the more advanced features available through methods of the `Sequencer` interface and the `Sequence` class.

<a name="124686" id="124686"></a>

## Moving to an Arbitrary Position in the Sequence

<a name="124687" id="124687"></a> There are two `Sequencer` methods that obtain the sequencer's current position in the sequence. The first of these:

```

long getTickPosition()

```

<a name="124689" id="124689"></a> returns the position measured in MIDI ticks from the beginning of the sequence. The second method:

```

long getMicrosecondPosition()

```

returns the current position in microseconds. This method assumes that the sequence is being played at the default rate as stored in the MIDI file or in the `Sequence`. It does **not** return a different value if you've changed the playback speed as described below.

<a name="124692" id="124692"></a> You can similarly set the sequencer's current position according to one unit or the other:

```

void setTickPosition(long tick)

```

or

```

void setMicrosecondPosition(long microsecond)

```

<a name="124696" id="124696"></a>

## Changing the Playback Speed

<a name="124697" id="124697"></a> As indicated earlier, a sequence's speed is indicated by its tempo, which can vary over the course of the sequence. A sequence can contain events that encapsulate standard MIDI tempo-change messages. When the sequencer processes such an event, it changes the speed of playback to reflect the indicated tempo. In addition, you can programmatically change the tempo by invoking any of these `Sequencer` methods:

```

    public void setTempoInBPM(float bpm)
    public void setTempoInMPQ(float mpq)
    public void setTempoFactor(float factor)

```

The first two of these methods set the tempo in beats per minute or microseconds per quarter note, respectively. The tempo will stay at the specified value until one of these methods is invoked again, or until a tempo-change event is encountered in the sequence, at which point the current tempo is overridden by the newly specified one.

<a name="124702" id="124702"></a> The third method, `setTempoFactor`, is different in nature. It scales whatever tempo is set for the sequencer (whether by tempo-change events or by one of the first two methods above). The default scalar is 1.0 (no change). Although this method causes the playback or recording to be faster or slower than the nominal tempo (unless the factor is 1.0), it doesn't alter the nominal tempo. In other words, the tempo values returned by `getTempoInBPM` and `getTempoInMPQ` are unaffected by the tempo factor, even though the tempo factor does affect the actual rate of playback or recording. Also, if the tempo is changed by a tempo-change event or by one of the first two methods, it still gets scaled by whatever tempo factor was last set. If you load a new sequence, however, the tempo factor is reset to 1.0.

<a name="124703" id="124703"></a> Note that all these tempo-change directives are ineffectual when the sequence's division type is one of the SMPTE types, instead of PPQ.

<a name="124704" id="124704"></a>

## Muting or Soloing Individual Tracks in the Sequence

<a name="124705" id="124705"></a> It's often convenient for users of sequencers to be able to turn off certain tracks, to hear more clearly exactly what is happening in the music. A full-featured sequencer program lets the user choose which tracks should sound during playback. (Speaking more precisely, since sequencers don't actually create sound themselves, the user chooses which tracks will contribute to the stream of MIDI messages that the sequencer produces.) Typically, there are two types of graphical controls on each track: a **mute** button and a **solo** button. If the mute button is activated, that track will not sound under any circumstances, until the mute button is deactivated. Soloing is a less well-known feature. It's roughly the opposite of muting. If the solo button on any track is activated, only tracks whose solo buttons are activated will sound. This feature lets the user quickly audition a small number of tracks without having to mute all the other tracks. The mute button typically takes priority over the solo button: if both are activated, the track doesn't sound.

<a name="124706" id="124706"></a> Using `Sequencer` methods, muting or soloing tracks (as well as querying a track's current mute or solo state) is easily accomplished. Let's assume we have obtained the default `Sequencer` and that we've loaded sequence data into it. Muting the fifth track in the sequence would be accomplished as follows:

```

    sequencer.setTrackMute(4, true);
    boolean muted = sequencer.getTrackMute(4);
    if (!muted) { 
        return;         // muting failed
    }

```

There are a couple of things to note about the above code snippet. First, tracks of a sequence are numbered starting with 0 and ending with the total number of tracks minus 1. Also, the second argument to `setTrackMute` is a boolean. If it's true, the request is to mute the track; otherwise the request is to unmute the specified track. Lastly, in order to test that the muting took effect, we invoke the `Sequencer getTrackMute` method, passing it the track number we're querying. If it returns `true`, as we'd expect in this case, then the mute request worked. If it returns `false`, then it failed.

<a name="124713" id="124713"></a> Mute requests may fail for various reasons. For example, the track number specified in the `setTrackMute` call might exceed the total number of tracks, or the sequencer might not support muting. By calling `getTrackMute`, we can determine if our request succeeded or failed.

<a name="124714" id="124714"></a> As an aside, the boolean that's returned by `getTrackMute` can, indeed, tell us if a failure occurred, but it can't tell us why it occurred. We could test to see if a failure was caused by passing an invalid track number to the `setTrackMute` method. To do this, we would call the `getTracks` method of `Sequence`, which returns an array containing all of the tracks in the sequence. If the track number specified in the `setTrackMute` call exceeds the length of this array, then we know we specified an invalid track number.

<a name="124715" id="124715"></a> If the mute request succeeded, then in our example, the fifth track will not sound when the sequence is playing, nor will any other tracks that are currently muted.

<a name="124716" id="124716"></a> The method and techniques for soloing a track are very similar to those for muting. To solo a track, invoke the `setTrackSolo` method of `Sequence:`

```

void setTrackSolo(int track, boolean bSolo)

```

As in `setTrackMute`, the first argument specifies the zero-based track number, and the second argument, if `true`, specifies that the track should be in solo mode; otherwise the track should not be soloed.

<a name="124719" id="124719"></a> By default, a track is neither muted nor soloed.

<a name="124721" id="124721"></a>

## Synchronizing with Other MIDI Devices

<a name="124722" id="124722"></a> `Sequencer` has an inner class called `Sequencer.SyncMode`. A `SyncMode` object represents one of the ways in which a MIDI sequencer's notion of time can be synchronized with a master or slave device. If the sequencer is being synchronized to a master, the sequencer revises its current time in response to certain MIDI messages from the master. If the sequencer has a slave, the sequencer similarly sends MIDI messages to control the slave's timing.

<a name="124723" id="124723"></a> There are three predefined modes that specify possible masters for a sequencer: `INTERNAL_CLOCK`, `MIDI_SYNC`, and `MIDI_TIME_CODE`. The latter two work if the sequencer receives MIDI messages from another device. In these two modes, the sequencer's time gets reset based on system real-time timing clock messages or MIDI time code (MTC) messages, respectively. (See the MIDI specification for more information about these types of message.) These two modes can also be used as slave modes, in which case the sequencer sends the corresponding types of MIDI messages to its receiver. A fourth mode, `NO_SYNC`, is used to indicate that the sequencer should not send timing information to its receivers.

<a name="124724" id="124724"></a> By calling the `setMasterSyncMode` method with a supported `SyncMode` object as the argument, you can specify how the sequencer's timing is controlled. Likewise, the `setSlaveSyncMode` method determines what timing information the sequencer will send to its receivers. This information controls the timing of devices that use the sequencer as a master timing source.

<a name="124725" id="124725"></a>

## Specifying Special Event Listeners

<a name="124726" id="124726"></a> Each track of a sequence can contain many different kinds of `MidiEvents`. Such events include Note On and Note Off messages, program changes, control changes, and meta events. The Java Sound API specifies "listener" interfaces for the last two of these event types (control change events and meta events). You can use these interfaces to receive notifications when such events occur during playback of a sequence.

<a name="124727" id="124727"></a> Objects that support the `ControllerEventListener` interface can receive notification when a `Sequencer` processes particular control-change messages. A control-change message is a standard type of MIDI message that represents a change in the value of a MIDI controller, such as a pitch-bend wheel or a data slider. (See the MIDI specification for the complete list of control-change messages.) When such a message is processed during playback of a sequence, the message instructs any device (probably a synthesizer) that's receiving the data from the sequencer to update the value of some parameter. The parameter usually controls some aspect of sound synthesis, such as the pitch of the currently sounding notes if the controller was the pitch-bend wheel. When a sequence is being recorded, the control-change message means that a controller on the external physical device that created the message has been moved, or that such a move has been simulated in software.

<a name="124728" id="124728"></a> Here's how the `ControllerEventListener` interface is used. Let's assume that you've developed a class that implements the `ControllerEventListener` interface, meaning that your class contains the following method:

```

    void controlChange(ShortMessage msg)

```

Let's also assume that you've created an instance of your class and assigned it to a variable called `myListener`. If you include the following statements somewhere within your program:

```

    int[] controllersOfInterest = { 1, 2, 4 };
    sequencer.addControllerEventListener(myListener,
        controllersOfInterest);

```

then your class's `controlChange` method will be invoked every time the sequencer processes a control-change message for MIDI controller numbers 1, 2, or 4. In other words, when the `Sequencer` processes a request to set the value of any of the registered controllers, the `Sequencer` will invoke your class's `controlChange` method. (Note that the assignments of MIDI controller numbers to specific control devices is detailed in the MIDI 1.0 Specification.)

<a name="124734" id="124734"></a> The `controlChange` method is passed a `ShortMessage` containing the controller number affected, and the new value to which the controller was set. You can obtain the controller number using the `ShortMessage.getData1` method, and the new setting of the controller's value using the `ShortMessage.getData2` method.

<a name="124735" id="124735"></a> The other kind of special event listener is defined by the `MetaEventListener` interface. Meta messages, according to the Standard MIDI Files 1.0 specification, are messages that are not present in MIDI wire protocol but that can be embedded in a MIDI file. They are not meaningful to a synthesizer, but can be interpreted by a sequencer. Meta messages include instructions (such as tempo change commands), lyrics or other text, and other indicators (such as end-of-track).

<a name="124736" id="124736"></a> The `MetaEventListener` mechanism is analogous to `ControllerEventListener`. Implement the `MetaEventListener` interface in any class whose instances need to be notified when a `MetaMessage` is processed by the sequencer. This involves adding the following method to the class:

```

void meta(MetaMessage msg)

```

<a name="124738" id="124738"></a> You register an instance of this class by passing it as the argument to the `Sequencer addMetaEventListener` method:

```

boolean b = sequencer.addMetaEventListener
        (myMetaListener);

```

This is slightly different from the approach taken by the `ControllerEventListener` interface, because you have to register to receive all `MetaMessages,` not just selected ones of interest. If the sequencer encounters a `MetaMessage` in its sequence, it will invoke `myMetaListener.meta`, passing it the `MetaMessage` encountered. The `meta` method can invoke `getType` on its `MetaMessage` argument to obtain an integer from 0 to 127 that indicates the message type, as defined by the Standard MIDI Files 1.0 specification.

&#160;
