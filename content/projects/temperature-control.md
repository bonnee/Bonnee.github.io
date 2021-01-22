---
title: "Temperature control with Arduino and PWM fans"
date: 2017-02-23
draft: true
categories: ["projects"]
tags: ["arduino", "micro", "pwm", "homelab"]
---

# TODO: insert pictures

> Originally published on [Instructables](https://www.instructables.com/Temperature-Control-With-Arduino-and-PWM-Fans/)

Temperature control with PID on Arduino and PWM fans for DIY server/network rack cooling.

A few weeks ago I needed to setup a rack with network devices and a few servers.

The rack is placed in a closed garage, so the temperature range between winter and summer is pretty high, and also dust could be a problem.

While browsing the Internet for cooling solutions, I found out that they're pretty expensive, in my place at least, being >100€ for 4 230V ceiling-mounted fans with a thermostat control. I didn't like the thermostat drive because it sucks in a lot of dust when powered, because of the fans going full power, and gives no ventilation at all when unpowered.

So, unsatisfied with these products, I decided to go the DIY way, building something that can smoothly mantain a certain temperature.

# How it works

To make things a lot easier I went for DC fans: they're much less noisy than AC fans while being a bit less powerful, but they're still more than enough for me.

The system uses a temperature sensor to control four fans that are driven by an Arduino controller. The Arduino throttles the fans using PID logic, and drives them through PWM.

The temperature and fan speed are reported through a 8-digit 7-segment display, fitted on a rack-mounted aluminium bar. Besides the display there are two buttons for tuning the target temperature.

# Parts

Note: I tried to realize this project with things I had lying in the house, so not everything can be ideal. Budget was a concern.

Here are the components I used:

- Hardware

  - One acrylic panel: used as the base (€ 1.50);
  - Four 3.6x1cm L shaped PVC profiles (€ 4.00);
  - One aluminum panel: cut at 19" in width (€ 3.00);

- Electronics
  - Four 120mm PWM fans: I went for Arctic F12 PWM PST because of the ability to stack them in parallel (4x € 8.00);
  - One Pro Micro: Any ATMega 32u4 powered board should work fine with my code (€ 4.00);
  - One relay board: to switch off the fans when they're not needed (€ 1.50); - One 8 digit 7-segment MAX7219 display module (€ 2.00);
  - Three momentary push buttons, 1 is for reset (€ 2.00); - One 3A power switch (€ 1.50);
  - One LAN cable coupler: to easilly disconnect the main assembly to the display panel (€ 2.50);
  - One 5V and 12V dual output power supply: You can use 2 separated PSUs or a 12V with a step down converter to 5V (€ 15.00);
  - Cables, screws and other minor components (€ 5.00);

Total cost: € 74.00 (if I had to buy all the components on Ebay/Amazon).

# The case

The case is made of 4 thin L-shaped plastic profiles glued and riveted to an acrylic board.

All the components of the box are glued with epoxy.

Four 120mm holes are cut in the acrylic to fit the fans. An additional hole is cut for letting the thermometer cables pass through.

The front panel has a power switch with an indicator light. On the left, two holes let the front panel cable and the USB cable go out. An additional reset button is added for easier programming (the Pro Micro doesn't have a reset button, and sometimes it's useful in order to upload a program onto it).

The box is held up by 4 screws passing through holes the acrylic base.

The front panel is made of a brushed aluminum panel, cut at 19" in width and with a height of ~4cm. The display hole was made with a Dremel and the other 4 holes for screws and buttons were made with a drill.

# Electronics

The control board is pretty simple and compact. During the making of the project, i found out that when I supply 0% PWM to the fans, they will run at full speed. To completely stop the fans from spinning, I added a relay that shuts off the fans when they're not needed.

The front panel is connected to the board through a network cable that, using a cable coupler, can be easilly detached from the main enclosure. The back of the panel is made of a 2.5x2.5 electrical conduit and fixed to the panel with double-sided tape. The display is also fixed to the panel with tape.

As you can see in the schematics, I've used some external pullup resistors. These provide a stronger pullup than the arduino's.

The Fritzing schematics can be found on my [GitHub repo](https://github.com/Bonnee/fancontrol).

# The code

[Intel's specification](http://www.formfactors.org/developer/specs/4_Wire_PWM_Spec.pdf) for 4-pin fans suggests a 25KHz target PWM frequency and 21 kHz to 28 kHz acceptable range. The problem is that Arduino's default frequency is 488Hz or 976Hz, but the ATMega 32u4 is perfectly capable of delivering higher frequencies, so we only need to set it up correctly. I referred to [this](http://r6500.blogspot.it/2014/12/fast-pwm-on-arduino-leonardo.html) article about the Leonardo's PWM to clock the fourth timer to 23437Hz which is the closest it can get to 25KHz.

I used various libraries for the [display](https://github.com/giech/LedControl), the [temperature sensor](https://github.com/markruys/arduino-DHT) and the [PID logic](https://github.com/br3ttb/Arduino-PID-Library).

The full updated code can be found on my [GitHub repo](https://github.com/Bonnee/fancontrol).

# Conclusion

So here it is! I have to wait till this summer to actually see it in action, but I'm pretty confident it'll work fine.

I'm planning on making a program to see the temperature from the USB port that I connected to a Raspberry Pi.

I hope that everything was understandable, If not let me know and I will explain better.
