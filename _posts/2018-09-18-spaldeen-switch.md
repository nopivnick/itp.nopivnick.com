---
layout:       single
title:        "PhysComp Week 2: Spaldeen Switch"
date:         2018-09-18 19:02:01 -04:00
categories:   PhysComp
---

*Full disclosure: this assignment may have been a bit of a cheat (?) in that it makes use of a switch cannibalized from a project I made during ITP Camp 2018.*

The switch I used for this week's assignment is constructed from conductive fabric and a classic pink Spalding stickball (a.k.a. a 'Spaldeen').

![image-title-here](/assets/images/spaldeen.jpg)

I cut the ball in half and glued two pieces of non-contiguous conductive fabric to the Spaldeen's surface using [Barge](http://www.bargeadhesive.com/products.html) contact cement. Barge is a close cousin to rubber cement and a staple of shoe repair because it provides a sturdy bond and maintains it's flexibility after it dries.

![image-title-here](/assets/images/IMG_3443.jpg)

Once the cement dried I inverted the hemisphere so the two pieces of conductive fabric oppose one another and voila: a momentary pushbutton (or squooshbutton) switch!

![image-title-here](/assets/images/IMG_3446.jpg)

The circuit is a classic green LED preceded by a 220 Ohm resistor drawing 5 volts from an Arduino Uno.

![image-title-here](/assets/images/IMG_3448.jpg)

When the opposing sides of the Spaldeen 'squooshbutton' are squeezed together, the two pieces of conductive fabric make contact, the circuit completes, and the LED illuminates.

![image-title-here](/assets/images/IMG_3450.jpg)

Note: I've found it helpful to put a 'dog leg' (a.k.a an offset bend) in the anode leg of an LED. Doing so helps 1.) easily identify the anode and 2.) both legs of the LED bottom out evenly in a breadboard.

![image-title-here](/assets/images/IMG_3447.jpg)

And finally, a schematic of the circuit in question:

![image-title-here](/assets/images/IMG_3452.jpg)
