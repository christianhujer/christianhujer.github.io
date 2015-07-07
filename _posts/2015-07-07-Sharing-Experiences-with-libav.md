---
layout: post
title: Sharing Experiences with `libav`
---

I've been trying to get `libav` running for screen casts for quite some time now - with gaps due to travelling and customer engagements.
Here's my parts, my requirements, my experiences, what I've achieved so far, and what's still missing.
I write this for myself to use it for future reference.
But if you find it interesting and helpful, I'd be delighted to know your feedback.


# Intro


## Goal / Requirements

Recording excellent Screencasts.

* Full HD Screen Resolution
* Full HD cameras from different angles
* Very good audio quality
* Encoding the video in a format that's preferred by YouTube.
  Best would be MP4 container with 48 kHz Stereo AAC-LC audio and Full-HD H.264 video as described on [https://support.google.com/youtube/answer/1722171?hl=en].


## Parts List

* A machine with an AMD A8-3850 CPU
* Two Full-HD (1920×1080) monitors
* 3 Logitech C920 USB webcams
* A Logitech G430 USB Headset
* A Kinobo "Natsuki" Mono USB Microphone with CM108 audio controller
* Kubuntu 14.10
* `libav`
* audacity
* Quassel IRC client (to communicate with `libav` developers)
* `git`, `gcc`, `make` etc. to build `libav` from git.


### On the machine

The machine with an AMD A8-3850 is simply the stationary machine that I have.
It can easily control two Full-HD monitors with Full HD resolution.
One monitor would be used for script and control, the other for "acting".


### On the cameras

I want to use three webcams and would put one on top of my screen, one to one side of the keyboard and one to the other side of the keyboard.
This way I can speak to three different cameras, showing myself from different angles.
The idea is that switching between different camera angles will make the video more interesting for the viewers.
I'm even considering a fourth camera below the monitor.


### On the microphones

I've already tried the audio quality of both, the G430 Headset and the C920 Webcam.
While both have good audio quality for their specific purpose, the audio quality is not sufficient for my purpose of a screencast.
The C920 records too much background noise, the G430 not enough bass.
So I got a USB microphone which was not so expensive but had quite good reviews on Amazon: The Kinobo "Natsuki".
Now that I have experience with all three of them, I would actually like to mix.


# Initial problems and their solutions


## Learning how to use `avconv`

The interface of `avconv` is not trivial.
The proper sequence of command line arguments and the mapping mechanism were things which took me a bit of time to figure out.


## `avconv` wouldn't record H.264 from a Logitech C920 - now it does

Originally, `avconv` wasn't recording H.264 from a Logitech C920.
It was using MJPEG, which the C920 cannot deliver in Full HD at 24 FPS, and then recoding that MJPEG as H.264, which eats 90% CPU (almost 1 core).

I got help and a patch from the `libav` developers that made it work.
Luca Barbato implemented this on 2015-02-27 in commit 9a26ba971387a348e14f363ddcdcb5bba0b413d1 "v4l2: Add support for h264".

Now, the H.264 stream from the webcam can be directly stored, which eats ~3% CPU.


## `avconv` record video and audio out-of-sync

The audio is usually between 1 and 4 seconds delayed as compared to the video.


## Webcam video from Logitech C920 was "rolling"

Initially, the Webcam video from the Logitech C920 had a "rolling" effect when using tube lights.
I found out that this can be eliminated easily by telling the camera to synchronize with the power line frequency.
This can be done using the `v4l-ctl` command.

The following command lists the controls available for a specific camera:

    v4l-ctl -d /dev/video0 -l

The following command enables power line frequency synchronization for a Logitech C920:

    v4l-ctl -d /dev/video0 -c power_line_frequency=2


## Finding out about the pulse device names

The following command lists the pulse device names:

    pactl list sources

The output is quite long, so I don't post it here.


## Finding out about the ALSA device names

The following command lists the ALSA devices:

    arecord -l

Here's a sample run of `arecord -l`:

    christian@armor01:~$ arecord -l
    **** List of CAPTURE Hardware Devices ****
    card 1: Device [USB PnP Sound Device], device 0: USB Audio [USB Audio]
      Subdevices: 1/1
      Subdevice #0: subdevice #0
    card 2: Generic_1 [HD-Audio Generic], device 0: ALC889 Analog [ALC889 Analog]
      Subdevices: 1/1
      Subdevice #0: subdevice #0
    card 2: Generic_1 [HD-Audio Generic], device 2: ALC889 Alt Analog [ALC889 Alt Analog]
      Subdevices: 2/2
      Subdevice #0: subdevice #0
      Subdevice #1: subdevice #1
    card 3: Juli [ESI Juli@], device 0: ICE1724 [ICE1724]
      Subdevices: 1/1
      Subdevice #0: subdevice #0
    card 3: Juli [ESI Juli@], device 1: ICE1724 IEC958 [ICE1724 IEC958]
      Subdevices: 1/1
      Subdevice #0: subdevice #0
    card 4: C920 [HD Pro Webcam C920], device 0: USB Audio [USB Audio]
      Subdevices: 1/1
      Subdevice #0: subdevice #0
    card 5: C920_1 [HD Pro Webcam C920], device 0: USB Audio [USB Audio]
      Subdevices: 1/1
      Subdevice #0: subdevice #0
    card 6: Headset [Logitech G430 Gaming Headset], device 0: USB Audio [USB Audio]
      Subdevices: 1/1
      Subdevice #0: subdevice #0
    card 7: C920_2 [HD Pro Webcam C920], device 0: USB Audio [USB Audio]
      Subdevices: 1/1
      Subdevice #0: subdevice #0

This means that my devices for recording audio are:

* `hw:1` to record from USB PnP Sound Device, which is the Kinobo "Natsuki" microphone
* `hw:4`, `hw:5` and `hw:7` are the webcams
* `hw:6` is the headset

**Caveat!** The device is certainly not stable.
It will surely change whenever you reconnect things.
Also, I don't know how the USB is scanned during boot.
To be on the safe side, I currently assume that rebooting changes the order.


## Recording ALSA from Microphone and Headset failed

My first attempts recording from Microphone or Headset via ALSA failed.

This is the failing command line, along with the error printed by `avconv`:

    christian@armor01:~$ avconv -f alsa -i hw:1 test.mkv
    avconv version v12_dev0-1521-gf046c3b, Copyright (c) 2000-2015 the Libav developers
      built on Jul  7 2015 13:04:56 with gcc 4.9.1 (Ubuntu 4.9.1-16ubuntu6)
    [alsa @ 0x2980080] cannot set channel count to 2 (Invalid argument)
    hw:1: Input/output error

As you can see, the `avconv` error message tells precisely what's the problem: Setting the number of audio-channels to 2.
The microphone is not a stereo device, it's a mono device.
But `avconv` defaults to stereo.
Adding a command parameter `-ac 1` that sets the number of audio channels for that device to 1 helps:

    christian@armor01:~$ avconv -f alsa -ac 1 -i hw:1 test.mkv


## No video from one of the cameras

When I had all three Logitech C920 cameras attached and tried to record H.264 video from all three of them, one of them failed.
It would not open, giving an `ioctl` error `No space left on device`.

This could be solved by ensuring that each Logitech C920 is running on its own USB root hub and doesn't share its USB with another camera
The `lsusb` command can be used to verify this.
The output of `lsusb` seems rather random, so it's best to `| sort` it.

Here's a sample run of `lsusb` where the setup is okay.

    christian@armor01:~$ lsusb | sort
    Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 001 Device 005: ID 046d:082d Logitech, Inc. HD Pro Webcam C920
    Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 002 Device 004: ID 046d:082d Logitech, Inc. HD Pro Webcam C920
    Bus 003 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 003 Device 005: ID 06a3:0ccb Saitek PLC 
    Bus 003 Device 007: ID 046d:0a4d Logitech, Inc. 
    Bus 003 Device 008: ID 045e:00db Microsoft Corp. Natural Ergonomic Keyboard 4000 V1.0
    Bus 004 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 005 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 006 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 007 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
    Bus 008 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 008 Device 002: ID 046d:082d Logitech, Inc. HD Pro Webcam C920
    Bus 008 Device 005: ID 0d8c:013c C-Media Electronics, Inc. CM108 Audio Controller
    Bus 009 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub

As you can see, each "Logitech, Inc. HD Pro Webcam C920" entry is on a unique bus.
No two "Logitech, Inc. HD Pro Webcam C920" share the same bus.
This setup works.

If you have two webcams on the same Bus, there might be trouble.

Sharing the bus with devices like mouse, keyboard or Audio devices usually is not a problem.
They do not need much bandwidth, therefore clashes with the webcam are not expected.


# A few `avconv` Recording Command Lines

## Single Audio

### From ALSA, Mono, as WAVE file

    avconv -f alsa -ac 1 -i hw:1 output.wav

### From ALSA, Mono, as AC3 file

    avconv -f alsa -ac 1 -i hw:1 output.ac3

### From ALSA, Stereo, as WAVE file

    avconv -f alsa -i hw:4 output.wav

### From ALSA, Stereo, as AC3 file

    avconv -f alsa -i hw:4 output.ac3

### From Pulse Audio, as WAVE file

    avconv -f pulse -i alsa_input.usb-C-Media_Electronics_Inc._USB_PnP_Sound_Device-00-Device.analog-mono output.wav

### From Pulse Audio, as AC3 file

    avconv -f pulse -i alsa_input.usb-C-Media_Electronics_Inc._USB_PnP_Sound_Device-00-Device.analog-mono output.ac3

## Multiple Audio

### From ALSA, as Matroska file, in AC3 format

    avconv -f alsa -ac 1 -i hw:1 -f alsa -i hw:4 -f alsa -i hw:5 -f alsa -ac 1 -i hw:6 -f alsa -i hw:7 -map 0:0 -map 1:0 -map 2:0 -map 3:0 -map 4:0 output.mkv

### From ALSA, as MP4 file, in AC3 format

    avconv -f alsa -ac 1 -i hw:1 -f alsa -i hw:4 -f alsa -i hw:5 -f alsa -ac 1 -i hw:6 -f alsa -i hw:7 -c ac3 -map 0:0 -map 1:0 -map 2:0 -map 3:0 -map 4:0 output.mp4

## Single Video

### From first Screen 24 FPS Full HD as H.264 in an MP4 file

    avconv -f x11grab -r 24 -s 1920x1080 -i :0.0 output.mp4

### From second screen 24 FPS Full HD as H.264 in a Matroska file

    avconv -f x11grab -r 24 -s 1920x1080 -i :0.0+1920,0 output.mkv

### From first Webcam 24 FPS Full HD as H.264 in an MP4 file

    avconf -f video4linux2 -input_format h264 -r 24 -s 1920x1080 -i /dev/video0 output.mp4

### From second Webcam 24 FPS Full HD as H.264 in a Matroska file

    avconf -f video4linux2 -input_format h264 -r 24 -s 1920x1080 -i /dev/video1 output.mkv

## Multiple Videos

### From second screen and third Webcam 24 FPS Full HD as H.264 in a Matroska file

    avconv -f x11grab -r 24 -s 1920x1080 -i :0.0+1920,0 -f video4linux2 -input_format h264 -r 24 -s 1920x1080 -i /dev/video2 -map 0:0 -map 1:0 output.mkv

### Both screens 24 FPS Full HD as H.264 in a Matroska file

    avconv -f x11grab -r 24 -s 1920x1080 -i 0:0+1920,0 -f x11grab -r 24 -s 1920x1080 -i :0.0 -map 0:0 -map 1:0 output.mkv

## Recoring 1 Audio + 1 Video

### From Microphone AC3 and Second Screen H.264 in a Matroska file

    avconv -f alsa -ac 1 -i hw:1 -f x11grab -r 24 -s 1920x1080 -i :0.0+1920x0 output.mkv

### From Third Webcam Microphone AC3 and Camera H.264 in a Matroska file

    avconv -f alsa -i hw:7 -f video4linux2 -input_format h264 -r 24 -s 1920x1080 -i /dev/video2 output.mkv

## Recording multiple audio as AC3 and video streams as H.264 in a Matroska file, with labels and language

Records left screen, right screen, both screens, three webcams and two microphones into one Matroska file

    avconv \
        -f x11grab -r 24 -s 1920x1080 -i :0.0 \
        -f x11grab -r 24 -s 1920x1080 -i :0.0+1920,0 \
        -f x11grab -r 24 -s 3840x1080 -i :0.0 \
        -f video4linux2 -input_format h264 -s 1920x1080 -r 24 -i /dev/video0 \
        -f video4linux2 -input_format h264 -s 1920x1080 -r 24 -i /dev/video1 \
        -f video4linux2 -input_format h264 -s 1920x1080 -r 24 -i /dev/video2 \
        -f alsa -ac 1 -i hw:1 \
        -f alsa -i hw:4 \
        -f alsa -i hw:5 \
        -f alsa -ac 1 -i hw:6 \
        -f alsa -i hw:7 \
        \
        -metadata title="Crazy Screencasting" \
        -metadata:s:0 title="Left Screen" -metadata:s:0 language=eng \
        -metadata:s:1 title="Right Screen" -metadata:s:1 language=eng \
        -metadata:s:2 title="Both Screens" -metadata:s:2 language=eng \
        -metadata:s:3 title="First Webcam" -metadata:s:3 language=eng \
        -metadata:s:4 title="Second Webcam" -metadata:s:4 language=eng \
        -metadata:s:5 title="Third Webcam" -metadata:s:5 language=eng \
        -metadata:s:6 title="Microphone" -metadata:s:6 language=eng \
        -metadata:s:7 title="First Webcam" -metadata:s:7 language=eng \
        -metadata:s:8 title="Second Webcam" -metadata:s:8 language=eng \
        -metadata:s:9 title="Headset" -metadata:s:9 language=eng \
        -metadata:s:10 title="Third Webcam" -metadata:s:10 language=eng \
        \
        -f matroska \
        -c:s:3 copy \
        -c:s:4 copy \
        -c:s:5 copy \
        -map 0:0 -map 1:0 -map 2:0 -map 3:0 -map 4:0 -map 5:0 -map 6:0 -map 7:0 -map 8:0 -map 9:0 -map 10:0 \
        output.mkv


# Remaining Issues


## Mapping between ALSA/Pulse and Video4Linux

I haven't yet checked whether I'm actually numbering the webcams consistently.
In my setup, that simply doesn't matter (yet?).


## Delay between Audio and Video

It seems that the Audio and Video streams are not in sync.
They start recording with offsets to each other.
This is a serious issue, as it would mean that video editing involves sound synchronization.
Sound synchronization is a step which I would like to skip when editing the videos.
Cutting is enough.

With the help of the `libav` developers, I've been trying various things, including a few edits to the source code.
However, I haven't succeeded yet in getting audio and video recorded synchronously.


# Conclusion

`libav` is a very powerful tool.
And the developers are very helpful.
Right now, I cannot yet achieve with `libav` what I would like to achieve: Record top quality screen casts.
But I think that I am getting closer.
