Changelog
=========

* 1.0.0 : Initial Release
  * 1.0.1 : Fix small bug where initial (unconfigured) plugin instances would not enable cc xmit.
* 1.1.0 : Added expression and performance switch mapping in advanced configuration.
          Enabled output gain adjustment.
          Fixed a bug where the tempo switch did not trigger preset dump.
  * 1.1.1 : Don't send old mknob values with SysEx. It seems to confuse the unit sometimes. ¯\_(ツ)_/¯
  * 1.1.2 : Support enable/disable switch on device in Live.
            Auto-disable other instances using the same H9 port and channel - so that multiple tracks can use the same H9 or multiple presets can be switched between in the same set.
            Send double-precision 14-bit MIDI values for primary knobs (currently restricted to 10-bits worth of precision).
  * 1.1.3 : Add Hotsawz and Harmadillo Algorithms
  * 1.1.4 : Add Tricerachorus algorithm. Fix a nasty bug with multiple instances sharing the same H9 under some specific conditions.
