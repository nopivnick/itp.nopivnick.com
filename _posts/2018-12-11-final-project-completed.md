---
layout:       single
title:        "ICM & Physcomp » Final: Completed"
date:         2018-12-11 21:11:43 -05:00
categories:   ICM PhysComp
---

# Intention

Creaturely Life explores the winding of yarn as a tactile, tangible interface for reading electronic text.


# Concept

The piece takes it's name from a collection of poems by Michael Joyce, written in stream of consciousness from the perspective of a woman keeping vigil over her dying husband. In the final poem of the series she recounts having found solace in knitting beside her husband’s hospice bed.

Winding yarn with a ball winder is an intrinsically satisfying interaction. The crank evokes the passage of time. The wobble of the spool, spinning off-axis, conjures visions of an orrery, the cycle of life, and what it means to be immaterial.


# Presentation

<iframe src="https://drive.google.com/file/d/1UEiZmZLYAcsOGVfixQodvq5hfN59wyf5/preview" width="640" height="480"></iframe>

<br>

An installation at heart, Creaturely Life is both intimate and humble in presentation. A table and chair in the center of a modest sized room. A screen, a glass jar containing a ball of yarn, a knob for making selections, and a crank-driven yarn ball winder clamped to the edge of the table. The user enters the space, preferably alone, and takes a seat at the table. On the screen, a short bit of text inviting the user to "Turn the crank on the yarn ball winder to begin ...".

Turning the crank of a yarn ball winder, poems unfold at first in fragments on the screen as a length of yarn (the poem as object) passes through the user's fingers. The ball in the glass jar diminishes in size in proportion to the growing mass on the spindle of the winder. The poem fragments run their course and the poem is revealed in it's entirety only once the original ball of yarn has disappeared and recreated in identical form on on the winder's spindle.

The newly wound ball is placed back in the glass jar, the user selects another in the collection of poems, and the interactive reading begins anew.


# Fabrication

The backbone of the piece is built on a [Knit Picks mechanical yarn ball winder](https://www.knitpicks.com/accessories/Yarn_Ball_Winder__D82500.html).

<iframe src="https://drive.google.com/file/d/1WXBPvOUwaBv0U6qMvZwiZFM50WBOxLV5/preview" width="640" height="480"></iframe>

<br>

Rotations of the crank are measured using an [Allegro A1220LUA-T](https://www.allegromicro.com/en/Products/Magnetic-Digital-Position-Sensor-ICs/Hall-Effect-Latches-Bipolar-Switches/A1220-1-2-3.aspx) latching-type hall effect sensor and a magnetic ring with eight alternating north and south poles. Both the hall effect sensor and the accompanying magnetic ring were included in Sparkfun's [Wheel Encoder Kit](https://www.sparkfun.com/products/12629).

Before permanently installing the sensor and magnetic ring, I mocked up a proof of concept by externally mounting the hall effect sensor and a pair of small neodymium magnets on a cardboard disk as a stand-in for the ring magnet.

<iframe src="https://drive.google.com/file/d/1W7E6QjFv2Srxjbh8SawNt_ZKOf0W5wRy/preview" width="640" height="480"></iframe>

<br>

After some rudimentary testing, I disassembled the winder, removed some material from the back of the base, and ever so carefully drilled a tiny hole in the rear of the crank axel.

<iframe src="https://drive.google.com/file/d/1W59SwfLXHoAdvNpH-ZeCg8maQZEH0VLe/preview" width="640" height="480"></iframe>

<br>

The ring magnet was screwed to the crank ...

<iframe src="https://drive.google.com/file/d/1VvFgzBwQGizy94fT8SctKaf8P4D2j9jP/preview" width="640" height="480"></iframe>

<br>

... and the hall effect sensor optimally positioned to read the alternating poles of the ring magnet.

<iframe src="https://drive.google.com/file/d/1W1RLQeg0Zd6t12QJpZLrvQaLx7prrTU-/preview" width="640" height="480"></iframe>

<br>

The poems are selected with a rotary encoder and the piece is reset using the encoder's built-in switch.

<iframe src="https://drive.google.com/file/d/1Wet64xqJNf1EWFaqYx6crCmQF0h-lhCt/preview" width="640" height="480"></iframe>

Lastly, [a modified yarn swift](https://itp.nopivnick.com/fab/mounting-motors/) rigged to rotate using a DC motor driving a metal gearbox and two custom wooden pulleys connected by rubber band completes the installation.


# Code

The onscreen interaction is driven by a [P5 sketch](https://editor.p5js.org/nopivnick/sketches/r146uFpkE) coded in part as a state machine with a separate state for each poem in the collection.

In lieu of the physical interaction, I've tied an 'autoplay' function to the right arrow key.

Early on I'd assumed the effect I was after could be achieved using P5.js' ```text()``` and it's related functions. I actually managed to get a good part of the way with text() by iterating through an array of each character in the entire poem and compounding textWidth() to calculate how far out to push the given substring but any substring after the first carriage return was off by a hair because -- presumably -- textWidth() doesn't play nice with whatever padding is used prior to the word wrap, as depicted in an [earlier version](https://editor.p5js.org/nopivnick/sketches/ryo0tTnR7) of the sketch.

Discussion of the [Arduino code](https://gist.github.com/nopivnick/1cbb33a58ff56b4c8073aa1d3a0c2896) to come.


# Bill of materials

* [Arduino UNO Rev.3](https://store.arduino.cc/usa/arduino-uno-rev3)
* [SparkFun Wheel Encoder Kit (ROB-12629)](https://www.sparkfun.com/products/12629)
  * [Allegro A1220LUA-T (chopper-stabilized)](https://www.allegromicro.com/en/Products/Magnetic-Digital-Position-Sensor-ICs/Hall-Effect-Latches-Bipolar-Switches/A1220-1-2-3.aspx)
* [Rotary Encoder Switch](https://www.amazon.com/gp/product/B0085I4D5C/), make / model unknown
* [ON Semiconductor TIP120](https://www.onsemi.com/PowerSolutions/product.do?id=TIP120)


# System diagram

<img src="https://docs.google.com/drawings/d/e/2PACX-1vQ-Ss9_GBY-9HBQPvG1wtp4HU21lLY-rfzbulC-5ExXQU8KTWgsiGl1DnkbiZ9J2jHEsqu-LD9lxsG2/pub?w=960&amp;h=720">

<br>

# Circuit diagram

<iframe src="https://drive.google.com/file/d/1azyYpqFkUa-d_6nx2L3aV0EgSOAYpVJv/preview" width="640" height="480"></iframe>

<br>

# Breadboard wiring

<iframe src="https://drive.google.com/file/d/1b-PHecTamGCpc9gI74c68eazYmfhv9iT/preview" width="640" height="480"></iframe>

<br>

# References

* [Machine Design » Basics of Rotary Encoders](https://www.machinedesign.com/sensors/basics-rotary-encoders-overview-and-new-technologies-0)
* [SparkFun » Assembly Guide for RedBot » Wheel Encoders (SIK)](https://learn.sparkfun.com/tutorials/assembly-guide-for-redbot-with-shadow-chassis/1-wheel-encoders-sik)
* [Allegro A1220LUA-T (chopper-stabilized) datasheet](https://www.allegromicro.com/~/media/Files/Datasheets/A1220-1-2-3-Datasheet.ashx)
* [Allegro Hall-Effect Sensor ICs](https://www.allegromicro.com/en/Design-Center/Technical-Documents/Hall-Effect-Sensor-IC-Publications/Allegro-Hall-Effect-Sensor-ICs.aspx)
* [Latching Switch Hall-Effect IC Basics](https://www.allegromicro.com/en/Design-Center/Technical-Documents/Hall-Effect-Sensor-IC-Publications/Latching-Switch-Hall-Effect-IC-Basics.aspx)
* [TIP120 datasheet](https://www.onsemi.com/pub/Collateral/TIP120-D.PDF)


# Acknowledgements

* Michael Joyce, friend and mentor, writer and poet, for sharing with me his unpublished poems
* Giwon Park for help with code and urging me to use p5.dom
* Allison Parish for sharing with me a crucial example of creating arrays of spans by character
* Brent Baily for helping me sort out logic that correctly iterated through the substring playlist
* Ashley Lewis for walking me through state machines
* Mimi Yin, Tom Igoe, and Mithru Vigneshwara for their instruction, critique, and patience
