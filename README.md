H9 Remote v 1.0.0
=================

Copyright 2018 Daniel Collins. Use of this software, the provided plugin, and any additional resources provided, unless listed below under "Credit", is permitted under the conditions of the [Creative Commons Attribution-NonCommercial-ShareAlike](https://creativecommons.org/licenses/by-nc-sa/4.0/) license. All other rights are reserved. This software comes with NO WARRANTY, including suitability for purpose, and by using this software you waive any and all claims against the author or his assignees for any consequences, real or imagined, arising from, out of, or in conjunction with, said use.

# Description

The H9 Remote is a Max For Live (M4L) Audio Effect plugin which, while it does not alter your audio in any way internally, uses MIDI as a sideband control for an Eventide H9 stompbox.

The intended use case is either as a standalone audio effect on a track, to visually represent where the H9 would be in the signal chain, either in combination with an External Audio Effect plugin to support the actual audio routing, or to manage an in-line H9 attached to the instrument directly. *Note: This plugin processes no audio directly. It only controls an attached H9.*

The plugin is specifically designed to allow track automation of H9 parameters as if the user were manipulating the knobs in realtime on the front panel, using MIDI CC.

Basic features include:

* Does not need the device to contain the required preset - can set any algorithm and module directly.
* Operates entirely independently of the device's preset list - Does not overwrite any presets on the device.
* Supports Ableton M4L presets, so can store a completely separate set of presets than what is on the device.
* Saves the device configuration in the preset - no accidentally configuring the wrong H9 if you have multiple in a chain!
* Receives and transmits knob, expression, and hotswitch changes from the pedal, for track automation recording, live performance, etc.
* Supports Push and Live MIDI mapping to extend the controls for your H9 into the physical realm.
* Remembers which H9 each instance of the plugin was connected to, and restores the saved configuration from your Live file on open - ensuring your project loads with the right effects!
* Allows you to send expression to the H9 even if you don't have an expression pedal connected.


# Compatibility and Credit

H9 Remote is compatible with Ableton Live 9 and 10. No guarantees are made of compatiblity with any other version, although it might work.
It has only been tested on Mac, and no guarantee of compatiblity with Windows is offered nor is planned to be.

A basic undestanding of MIDI, channels, CC numbers, and device configuration/operation is assumed. This is not a tutorial.

The H9 Remote includes the imp.MIDI toolkit from The Impersonal Stereo (https://www.theimpersonalstereo.com/max-externals/) with gratefulness to David Butler. No ownership of this external is in any way implied or asserted.

# H9 Configuration

Before the plugin can correctly connect to your H9, you must set a few parameters on the H9 itself. Unfortunately, some of them cannot be configured through the iPod/Android App or the H9 Control software, but must be configured directly on the device.

Using the [H9 User Guide](https://www.eventideaudio.com/downloader/28), page 31 explains how to enter System Mode, and the following pages explain how to navigate through these modes. The necessary settings are entirely under the MIDI menu and are as follows:

[RCV CH] : Set this to whatever MIDI channel you want. If you'll be using this H9 via USB, you can set this to 1, but if you'll be using it over DIN MIDI you'll need to ensure that this channel is unique to this device.
[XMT CH] : Ideally this should be set to the same thing as RCV CH. Only make them different if you are dead certain you know what you're doing.
[RCV.CTL] : This sets up the H9 to receive the knob and expression changes from the remote. Set up as follows:
  [PSW] : 71 (or choose your own value, but you'll need to manually configure the plugin then)
  [EXP] : 11 (the default MIDI CC for expression)
  Ensure KB0-KB9 map to 22..31 - this is the default but if you or a previous owner customized these values, you'll need to set them back to this range, and sequential.
  You can set the other controls to receive CCs as well, but as of v 1.0.0 the H9 Remote does not support them. It won't hurt anything, though.
[XMT.CTL] : This sets up the H9 to send it's own Knob and Expression changes (e.g. when you use an attached physical expression pedal, the H9 Remote will track it and can send to Live's automation).
  [PSW] : 71. Same as under [RCV.CTL]
  [EXP] : 11.
  Ensure KB0-KB9 map to 22..31 just as for the RCV.CTL settings.
  You can set the other contorls to send CC as well, but as of v 1.0.0 the H9 Remote does not support them. It won't hurt anything though. Best to make sure these values match with the ones set in RVC.CTL.
[XMT.PC] : Does not matter as far as the plugin is concerned, but OFF can prevent MIDI loops in some cases.
[CTL.XMT] : Set to ON. This is essential.
[PGM.XMT] : Set to ON. This may be supported in a near-future version.
[SYS ID] : Leave at 1 unless you have more than one H9, then see the cautions under the following section "Multiple H9s".
[OUTPUT] : Merge is generally safe, but the proper value here depends on if you're using DIN MIDI and what your routing looks like. You'll need to use your own judgement here.
[CLK IN] : ON
[CLK OUT] : Doesn't matter unless you want Live to follow the H9's tempo, then set this to ON.

With these settings, minimal configuration of the plugin will be necessary. If you have only one H9, you can skip straight on to the Plugin Configuration.

## Multiple H9s

If you have more than one H9, there are two important things to consider:

* Under USB: Each H9 must have a uniquely named MIDI port
* Under DIN MIDI: Each H9 must have a unique SysEx ID and MIDI channel.

### USB

If you are using USB for your H9 connectivity, you can go into the Mac OS's Audio Midi Setup utility, hit Command-2 if the Studio View of your devices isn't visible, and if your H9s are all connected and powered on, you should be able to quickly see each one. They will all be named "H9 Pedal" at this point. Click each one and hit the (i) icon in the toolbar to open the preferences. Give each of them a unique, short Device Name such as Guitar and Bass or MoogSynth1 and MoogSynth2 for series-connected units. Make sure you hit Apply. Now when the devices are enumerated, they will have a unique USB MIDI port name. No further configuration is needed - they can even be all on Channel 1 on this configuration.

### DIN MIDI

If you are using DIN MIDI and you are putting all of your H9s in series on the same physical MIDI I/O pair, you need to use a unique SysEx ID for each unit, and set a unique MIDI channel. Use the same channel for TX and RX unless you have a very specific reason not to. **NOTE: This can cause problems connecting the units with SysEx ID higher than 1 to the Eventide H9 Control software or apps. This is a known bug with their software, and is not a problem with your H9 or the H9 Remote.** To reconnect these units to H9 Control, just reset their individual SysEx ID to 1.

If you are using DIN MIDI but the devices are on different ports of a multi-port MIDI host (like the MOTU MIDI Express), you don't need to do anything! But if it helps, renaming the ports as mentioned in the USB section above will help you remember which one belongs to which unit.

# Plugin Configuration and Setup

Insert the plugin after powering on and connecting your H9 to your computer. The plugin will initially load in Live with no configuration and a default preset. This preset will not initially be transmitted to the H9. In order to establish communication between the plugin and your H9, you must do the following:

* Choose the right MIDI port in the dropdown. If the software successfully opens the port, the button that initially says "Rescan" will switch to "Connected". This does not mean it found an H9! It just means the MIDI port was opened and it can now send and receive MIDI data to that port.
* Ensure that the MIDI RX and TX channels match the configuration of your H9. If you left them at 1, then you should not need to change these at all.
* Ensure that the CC settings match the configuration you chose. If you followed the setup directions above, you should not need to change any of these either.
* If you set a custom SysEx id for the unit you wish to correspond with, set it here as well. Otherwise leave it at 1.

To test that the plugin is communicating with your H9, you can now press the "Up Arrow" icon to load the current state of your H9 into the plugin and synchronize. The plugin should update to reflect the chosen algorithm, preset name, and knob settings of your H9. If nothing happens, make sure your settings are correct, that you have chosen the right MIDI port, and that the link status box says "Connected". If you loaded the plugin before powering on your H9 you can either remove and re-insert the plugin, or try clicking the link status box to initiate a rescan of the MIDI ports, then select the H9 when it appears.

# Basic Operation

