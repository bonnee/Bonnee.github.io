---
title: "Raspberry Pi audio player"
date: 2015-07-22T00:00:00+01:00
draft: true
---

# TODO: insert pictures

This tutorial has originally been published on [Instructables](https://www.instructables.com/Raspberry-Pi-Media-Player/)

A minimalistic music player based on Raspberry Pi model B with HifiBerry DAC and ATXRaspi power controller.
I made this with the help (also economical) of my father.Inspired by: [this](http://www.hifiberry.com/forums/topic/small-media-player-with-squeezeliteslave/) and [this](http://www.crazy-audio.com/2014/03/a-standalone-streaming-media-device-based-on-raspberry-pi-and-hifiberry-dac/t).\
Here's my first instructable!

# Step 1: Parts

1. A Raspberry Pi (I used the model B because I had one lying around the house);
2. [optional] A DAC (I reccomend HiFiberry DAC, it sounds good);
3. [optional] ATXRaspi power controller (to shut down and power up the Raspi easilly);
4. An enclosure (mine was 200mmx125mmx51mm externally);
5. A power supply (I used a MeanWell RS-15-5 5V 3A power supply from amazon);
6. A power switch (My version has a reed sensor on top of the power led that toggles if I swipe a magnet on front of it);
7. Cables to wire all the devices to the rear panel (I added a HDMI cable to use this project as a media center);
8. [optional] an IR receiver;
9. [optional] A fuse.

Some links:

- [HiFiBerry DAC](https://www.hifiberry.com/dac/)
- [Power supply](http://www.amazon.it/gp/product/B00MWQD43U?psc=1&redirect=true&ref_=oh_aui_detailpage_o04_s00)
- [Enclosure](http://www.ebay.it/itm/Aluminum-Amplifier-Cases-DAC-Enclosure-125-51-Xmm-DIY-Display-/231446512943?var&hash=item0&_uhb=1)

# Step 2: First prototype

To test and setup the functionalities I made a prototype of the project using a plastic cover. Simple and effective.

# Step 3: Assembling the final version

Assembling higly depends on the enclosure and on the components you choose.

I cutted the rear panel with a dremel and the holes with a drill press. It isn't beautiful but it works.

The enclosure I've chosen had a rail to house a matrix board on wich I placed all the components. There's no electrical contact between the enclosure and the copmponents.

# Step 4: Software

In the original project I wanted to use [runeaudio](http://www.runeaudio.com/) to power all the stuff but it was buggy and painful to set up (it took me hours to setup the IR receiver), so I decided to use [Volumio](https://volumio.org/) that is slower (raspbian based vs arch based) and the interface is not as good as runeaudio (not yet) but is more stable and reliable.

Both distros are very good to play webradios, spotify (only premium) and other streaming services, and music from network devices.

Changing distro can be easilly done because I left some space between the Raspi and the power supply to remove the sd card

[Here](https://learn.adafruit.com/using-an-ir-remote-with-a-raspberry-pi-media-center/overview) is a guide to set up an IR receiver.

# Step 5: Final Product

Here are some pictures of it mounted and fully functioning. The audio quality is pleasant and it looks amazing. It only misses the IR receiver at the moment.

Hope you enjoyed. Feel free to ask or suggest anything :)
