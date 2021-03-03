---
title: Accessing MIDI System Resources
author: lijiabao
date: 2020-12-06 01:47:48.666868300 +0800
category: Sound
categories: Sound
tags: Sound
---

# Accessing MIDI System Resources

The Java Sound API offers a flexible model for MIDI system configuration, just as it does for configuration of the sampled-audio system. An implementation of the Java Sound API can itself provide different sorts of MIDI devices, and additional ones can be supplied by service providers and installed by users. You can write your program in such a way that it makes few assumptions about which specific MIDI devices are installed on the computer. Instead, the program can take advantage of the MIDI system's defaults, or it can allow the user to select from whatever devices happen to be available.

<a name="119705" id="119705"></a> This section shows how your program can learn what MIDI resources have been installed, and how to get access to the desired ones. After you've accessed and opened the devices, you can connect them to each other, as discussed later in 
[Transmitting and Receiving MIDI Messages](MIDI-messages.html).

## The MidiSystem Class

<a name="119713" id="119713"></a> The role of the 
[`MidiSystem`](https://docs.oracle.com/javase/8/docs/api/javax/sound/midi/MidiSystem.html) class in the Java Sound API's MIDI package is directly analogous to the role of 
[`AudioSystem`](https://docs.oracle.com/javase/8/docs/api/javax/sound/sampled/AudioSystem.html) in the sampled-audio package. `MidiSystem` acts as a clearinghouse for accessing the installed MIDI resources.

<a name="119718" id="119718"></a> You can query the `MidiSystem` to learn what sorts of devices are installed, and then you can iterate over the available devices and obtain access to the desired ones. For example, an application program might start out by asking the `MidiSystem` what synthesizers are available, and then display a list of them, from which the user can select one. A simpler application program might just use the system's default synthesizer.

<a name="119726" id="119726"></a> The `MidiSystem` class also provides methods for translating between MIDI files and `Sequences`. It can report the file format of a MIDI file and can write files of different types.

An application program can obtain the following resources from the `MidiSystem`:

- Sequencers
- Synthesizers
- Transmitters (such as those associated with MIDI input ports)
- Receivers (such as those associated with MIDI output ports)
- Data from standard MIDI files
- Data from soundbank files

This page focuses on the first four of these types of resource. The others are discussed later in this tutorial. <a name="119749" id="119749"></a>

## Obtaining Default Devices

<a name="119751" id="119751"></a> A typical MIDI application program that uses the Java Sound API begins by obtaining the devices it needs, which can consist of one or more sequencers, synthesizers, input ports, or output ports.

<a name="121015" id="121015"></a> There is a default synthesizer device, a default sequencer device, a default transmitting device, and a default receiving device. The latter two devices normally represent the MIDI input and output ports, respectively, if there are any available on the system. (It's easy to get confused about the directionality here. Think of the ports' transmission or reception in relation to the software, not in relation to any external physical devices connected to the physical ports. A MIDI input port **transmits** data from an external device to a Java Sound API `Receiver`, and likewise a MIDI output port **receives** data from a software object and relays the data to an external device.)

<a name="121012" id="121012"></a> A simple application program might just use the default instead of exploring all the installed devices. The `MidiSystem` class includes the following methods for retrieving default resources:

```

static Sequencer getSequencer()
static Synthesizer getSynthesizer()
static Receiver getReceiver()
static Transmitter getTransmitter()

```

<a name="120991" id="120991"></a> The first two of these methods obtain the system's default sequencing and synthesis resources, which either represent physical devices or are implemented wholly in software. The `getReceiver` method obtains a `Receiver` object that takes MIDI messages sent to it and relays them to the default receiving device. Similarly, the getTransmitter method obtains a Transmitter object that can send MIDI messages to some receiver on behalf of the default transmitting device.

<a name="119787" id="119787"></a>

## Learning What Devices Are Installed

<a name="119789" id="119789"></a> Instead of using the default devices, a more thorough approach is to select the desired devices from the full set of devices that are installed on the system. An application program can select the desired devices programmatically, or it can display a list of available devices and let the user select which ones to use. The `MidiSystem` class provides a method for learning which devices are installed, and a corresponding method to obtain a device of a given type.

<a name="119794" id="119794"></a> Here is the method for learning about the installed devices:

```

 tatic MidiDevice.Info[] getMidiDeviceInfo()

```

As you can see, it returns an array of information objects. Each of these returned `MidiDevice.Info` objects identifies one type of sequencer, synthesizer, port, or other device that is installed. (Usually a system has at most one instance of a given type. For example, a given model of synthesizer from a certain vendor will be installed only once.) The `MidiDevice.Info` includes the following strings to describe the device:

- Name
- Version number
- Vendor (the company that created the device)
- A description of the device

You can display these strings in your user interface to let the user select from the list of devices.

However, to use the strings programmatically to select a device (as opposed to displaying the strings to the user), you need to know in advance what they might be. The company that provides each device should include this information in its documentation. An application program that requires or prefers a particular device can use this information to locate that device. This approach has the drawback of limiting the program to device implementations that it knows about in advance.

Another, more general, approach is to go ahead and iterate over the `MidiDevice.Info` objects, obtaining each corresponding device, and determining programmatically whether it's suitable to use (or at least suitable to include in a list from which the user can choose). The next section describes how to do this.

## Obtaining a Desired Device

Once an appropriate device's info object is found, the application program invokes the following `MidiSystem` method to obtain the corresponding device itself:

```

static MidiDevice getMidiDevice(MidiDevice.Info info)

```

<a name="120804" id="120804"></a> You can use this method if you've already found the info object describing the device you need. However, if you can't interpret the info objects returned by `getMidiDeviceInfo` to determine which device you need, and if you don't want to display information about all the devices to the user, you might be able to do the following instead: Iterate over all the `MidiDevice.Info` objects returned by `getMidiDeviceInfo`, get the corresponding devices using the method above, and test each device to see whether it's suitable. In other words, you can query each device for its class and its capabilities before including it in the list that you display to the user, or as a way to decide upon a device programmatically without involving the user. For example, if your program needs a synthesizer, you can obtain each of the installed devices, see which are instances of classes that implement the `Synthesizer` interface, and then display them in a list from which the user can choose one, as follows:

```

// Obtain information about all the installed synthesizers.
Vector synthInfos;
MidiDevice device;
MidiDevice.Info[] infos = MidiSystem.getMidiDeviceInfo();
for (int i = 0; i &lt; infos.length; i++) {
    try {
        device = MidiSystem.getMidiDevice(infos[i]);
    } catch (MidiUnavailableException e) {
          // Handle or throw exception...
    }
    if (device instanceof Synthesizer) {
        synthInfos.add(infos[i]);
    }
}
// Now, display strings from synthInfos list in GUI.    

```

<a name="119876" id="119876"></a> As another example, you might choose a device programmatically, without involving the user. Let's suppose you want to obtain the synthesizer that can play the most notes simultaneously. You iterate over all the MidiDevice.Info objects, as above, but after determining that a device is a synthesizer, you query its capabilities by invoking the `getMaxPolyphony` method of `Synthesizer`. You reserve the synthesizer that has the greatest polyphony, as described in the next section. Even though you're not asking the user to choose a synthesizer, you might still display strings from the chosen `MidiDevice.Info` object, just for the user's information.

<a name="119891" id="119891"></a>

## Opening Devices

<a name="119893" id="119893"></a> The previous section showed how to get an installed device. However, a device might be installed but unavailable. For example, another application program might have exclusive use of it. To actually reserve a device for your program, you need to use the `MidiDevice` method `open`:

```

if (!(device.isOpen())) {
    try {
      device.open();
  } catch (MidiUnavailableException e) {
          // Handle or throw exception...
  }
}

```

<a name="119916" id="119916"></a> Once you've accessed a device and reserved it by opening it, you'll probably want to connect it to one or more other devices to let MIDI data flow between them. This procedure is described in later in 
[Transmitting and Receiving MIDI Messages](MIDI-messages.html).

<a name="121197" id="121197"></a> When done with a device, you release it for other programs' use by invoking the `close` method of `MidiDevice`.
