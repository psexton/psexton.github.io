---
layout: post
title:  "Technote: JACK DBus Multiple Devices"
date:   2017-05-01
link-state: notebook
description: DBus exception when trying to start JACK
---

Using JACK2 with DBus and the handy `jack_control`, this weird error happened to me today:

```
$ ./scarlett_jack_start.sh 
--- driver select "alsa"
--- driver param set "device" -> "hw:USB"
--- driver param set "rate" -> "48000"
--- driver param set "period" -> "128"
--- driver param set "nperiods" -> "3"
--- engine param set "realtime" -> "true"
--- start
DBus exception: org.jackaudio.Error.Generic: Failed to open server
```

I've run this script before hundreds of times. I haven't modified it, and I know it's not buggy. Let's check that the device is recognized:

```
$ aplay -l
**** List of PLAYBACK Hardware Devices ****
card 0: PCH [HDA Intel PCH], device 0: CX20590 Analog [CX20590 Analog]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: PCH [HDA Intel PCH], device 3: HDMI 0 [HDMI 0]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: PCH [HDA Intel PCH], device 7: HDMI 1 [HDMI 1]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: PCH [HDA Intel PCH], device 8: HDMI 2 [HDMI 2]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 2: USB [Scarlett 2i2 USB], device 0: USB Audio [USB Audio]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```

Yep, it's there. `card2: USB`. Hmm. Let's tail the log while running that.

```
$ tail -f ~/.log/jack/jackdbus.log
Mon May  1 11:52:52 2017: Starting jack server...
Mon May  1 11:52:52 2017: JACK server starting in realtime mode with priority 10
Mon May  1 11:52:52 2017: self-connect-mode is "Don't restrict self connect requests"
Mon May  1 11:52:52 2017: ERROR: control open "hw:UCX23766685" (No such device)
Mon May  1 11:52:52 2017: ERROR: control open "hw:UCX23766685" (No such device)
Mon May  1 11:52:52 2017: creating alsa driver ... hw:UCX23766685|hw:UCX23766685|128|3|48000|0|0|nomon|swmeter|-|32bit
Mon May  1 11:52:52 2017: ERROR: control open "hw:UCX23766685" (No such device)
Mon May  1 11:52:52 2017: ERROR: ALSA: Cannot open PCM device alsa_pcm for playback. Falling back to capture-only mode
Mon May  1 11:52:52 2017: ERROR: Cannot initialize driver
Mon May  1 11:52:52 2017: ERROR: JackServer::Open failed with -1
Mon May  1 11:52:52 2017: ERROR: Failed to open server
Mon May  1 11:52:54 2017: Saving settings to "/home/psexton/.config/jack/conf.xml" ...

```

Well that's interesting. `UCX23766685` is the name of my other USB Audio device. Why is it trying to connect to that? Let's look at that config file.

```
$ cat ~/.config/jack/conf.xml 
<?xml version="1.0"?>
<!--
JACK settings, as persisted by D-Bus object.
You probably don't want to edit this because
it will be overwritten next time jackdbus saves.
-->
<!-- Mon May  1 11:52:54 2017 -->
<jack>
 <engine>
  <option name="driver">alsa</option>
  <option name="realtime">true</option>
  <option name="verbose">false</option>
  <option name="client-timeout">500</option>
 </engine>
 <drivers>
  <driver name="alsa">
   <option name="device">hw:USB</option>
   <option name="capture">hw:UCX23766685</option>
   <option name="playback">hw:UCX23766685</option>
   <option name="rate">48000</option>
   <option name="period">128</option>
   <option name="nperiods">3</option>
   <option name="hwmon">false</option>
   <option name="hwmeter">false</option>
   <option name="duplex">true</option>
   <option name="softmode">false</option>
   <option name="monitor">false</option>
   <option name="dither">n</option>
   <option name="shorts">false</option>
  </driver>
  <driver name="net">
  </driver>
  <driver name="firewire">
  </driver>
  <driver name="netone">
  </driver>
  <driver name="loopback">
  </driver>
  <driver name="dummy">
  </driver>
  <driver name="alsarawmidi">
  </driver>
 </drivers>
 <internals>
  <internal name="netmanager">
  </internal>
  <internal name="audioadapter">
  </internal>
  <internal name="profiler">
  </internal>
  <internal name="netadapter">
  </internal>
 </internals>
</jack>
```

Aha! It stored "capture" and "playback" parameters as well as the "device" parameter we're setting in the script! Let's try setting those too:

```
$ ./scarlett_jack_start.sh 
--- driver select "alsa"
--- driver param set "device" -> "hw:USB"
--- driver param set "rate" -> "48000"
--- driver param set "period" -> "128"
--- driver param set "nperiods" -> "3"
--- driver param set "capture" -> "hw:USB"
--- driver param set "playback" -> "hw:USB"
--- engine param set "realtime" -> "true"
--- start
```

Bingo. The answer is (apparently) that JACK is caching more things than we set, and this only seems to be a problem when switching back and forth between different devices.

The specific fix: Change `jack_control dps device hw:YOUR_DEVICE_NAME` to `jack_control dps device hw:YOUR_DEVICE_NAME dps capture hw:YOUR_DEVICE_NAME dps playback hw:YOUR_DEVICE_NAME`.
