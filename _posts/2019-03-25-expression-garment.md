---
layout:       single
title:        "Wearables Week 4: Self Expression Garment"
date:         2019-03-24 14:12:13 -04:00
categories:   Wearables
---

# Inspiration

This assignment was a collaboration with Idit Barak. Idit's blog post describing the project's inspiration and conceptual development can be found [here](https://wp.nyu.edu/iditbarak/2019/03/22/wearables-3/).


# Process

![soldering leads](/assets/wearables/week-4/images/IMG_4373.jpeg)

<iframe src="https://drive.google.com/file/d/1rBCkmmZQNka7Omq7acpLTADMmtXwFDO7/preview" width="640" height="480"></iframe>

![yoke mockup](/assets/wearables/week-4/images/IMG_4388.jpeg)

![silicon wire rail test](/assets/wearables/week-4/images/IMG_4389.jpeg)

![silicon wire red](/assets/wearables/week-4/images/IMG_4392.jpeg)

![silicon wire black](/assets/wearables/week-4/images/IMG_4396.jpeg)

![soldering haptic driver leads](/assets/wearables/week-4/images/IMG_4393.jpeg)

![charging LiPo battery](/assets/wearables/week-4/images/IMG_4394.jpeg)

![Flora powered by LiPo battery](/assets/wearables/week-4/images/IMG_4395.jpeg)

![yoke passing I2C bus scan](/assets/wearables/week-4/images/IMG_4400.jpeg)

<iframe src="https://drive.google.com/file/d/1rKAcYPsRwajeyBDqfAa0n4q_jhXaf27h/preview" width="640" height="480"></iframe>

<iframe src="https://drive.google.com/file/d/1rOliII2soH8vJEQ1aaXq1JxKWy5VOEjY/preview" width="640" height="480"></iframe>


# Circuit

![Fritzing breadboard diagram](/assets/wearables/week-4/images/erm_drv_fritz_3.3v.png)


# Code

The Arduino code running on the [Adafruit Flora v3](https://www.adafruit.com/product/659) at the time the assignment was presented can be found in [this commit](https://github.com/nopivnick/itp_team_bell-dress/blob/46f6afd499f4f4655aacc97997c6951eda346a99/arduino/bell-dress/bell-dress.ino) on the project's [GitHub repository](https://github.com/nopivnick/itp_team_bell-dress).


# Bill of Materials

## Dress

- flora print fabric
- flora print fabric, chiffon
- jingle bells, red
- embroidered heart, gold


## Electronics

### Plan A

- 1 x [Adafruit Flora v3](https://www.adafruit.com/product/659)
- 1 x [Adafruit TCA9548A I2C Multiplexer](https://www.adafruit.com/product/2717)
- 8 x [Adafruit DRV2605L Haptic Motor Controller](https://www.adafruit.com/product/2305)
- 8 x [Vibrating Mini Motor Disc](https://www.adafruit.com/product/1201)
- 1 x Catalex TTP233B Touch Sensor (possibly a knock-off)
- 1 x [Adafruit Lithium Ion Polymer Battery - 3.7v 500mAh](https://www.adafruit.com/product/1578)

### Plan A

- 1 x [Adafruit Flora v3](https://www.adafruit.com/product/659)
- 2 x [LilyPad Vibe Board](https://www.sparkfun.com/products/11008)
- 1 x Catalex TTP233B Touch Sensor (possibly a knock-off)
- 1 x [Adafruit Lithium Ion Polymer Battery - 3.7v 500mAh](https://www.adafruit.com/product/1578)

# Final Image(s)

![yoke circuit completed](/assets/wearables/week-4/images/IMG_4402.jpeg)


# Conclusions

My post on the Adafruit Support Forums is [here](https://forums.adafruit.com/viewtopic.php?f=19&t=149470).
