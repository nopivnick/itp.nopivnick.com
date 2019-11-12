---
layout:       single
title:        "Textile Interfaces » Labs + Final"
date:         2019-11-10 18:31:18 -05:00
categories:   Textile
classes:      wide
---

# Lab 0: Soft Circuit

![Basic soft circuit open](/assets/textile/2019-11-10/IMG_5248.png)

![Basic soft circuit closed](/assets/textile/2019-11-10/IMG_5247.png)

# Lab 1: 3 x Digital Switches

## Fold Switch

{% include video id="1pZi6cHqrLcPdC1pLCwqjCkzPMxJ4lC69" provider="google-drive" %}

## Bridge Switch

{% include video id="1Zd0kAqH_pohl3nhbJiAP7y2Og_7vTl54" provider="google-drive" %}

## Magnetic Trip Switch

This switch is (intended to be) held closed by embedding a small neodymium magnet between iron-on conductive fabric and felt backing.

![Two small pieces of felt covered in conductive fabric held in contact with tiny magnets](/assets/textile/2019-11-10/IMG_5269.png)

There were two unforeseen consequences with this implementation.

First, it turns out the heat -- in this case the heat of the iron -- is damaging to neodymium magnets. While they retained enough magnetic attraction to hold physical contact, the strength of the attraction was significantly diminished.

Second, it turns out the fabric we used in class is coated **not** impregnated with conductive material. While playing with the switch, the coating actually wore away by mechanical abrasion, rendering it useless.

![Closeup of conductive coating worn away by mechanical abrasion](/assets/textile/2019-11-10/IMG_5270.png)

# Lab 2: Analog Textile Sensor

{% include video id="1oF_BMz-ONKhmLUhpmtx_INFXPqhLU9ao" provider="google-drive" %}

# Final: Felt Compass Dial

For my final I wanted to keep playing with the little neodymium magnets. It occurred to me they could be used to set position on a dial of some kind, not unlike the detents on a rotary encoder. a metal snap would make a perfect conductive axis of rotation ... the most humble of [slip rings](https://en.wikipedia.org/wiki/Slip_ring).

![Top halves of a felt dial](/assets/textile/2019-11-10/IMG_5261.png)

![Bottom halves of a felt dial](/assets/textile/2019-11-10/IMG_5264.png)

If the volume is high enough you can hear the sound of the two magnets snapping together, pinching the conductive tape between them and ensuring good contact.

{% include video id="1Dxvm5ZmKdvbJcLQx5jch-s8Gj3WjGxEq" provider="google-drive" %}

Technically, my final doesn't qualify as a rotary encoder. Were it an encoder, the Arduino would be reading position from one signal. As it stands, what I made is in effect four digital switches in a circular configuration.

{% include video id="1ybDoIAgeMBFA51dyMZs7bRUPSMmKmMdQ" provider="google-drive" %}

I like to think of it as a (text)ile interface for playing text adventures, famous for their use of the four cardinal points as a navigational conceit.
