---
layout:       single
title:        "PhysComp Week 4: Servo w/ Disk Gauge"
date:         2018-10-02 20:11:20 -04:00
categories:   PhysComp
---

First things first: the code for this week's build borrows heavily from the [Love-O-Meter](https://www.youtube.com/watch?v=RJPKuGF4lRE) project in the Arduino Projects Book and [Project 7: Control a servo motor with a FSR](https://itp.nyu.edu/physcomp/labs/labs-arduino-digital-and-analog/servo-motor-control-with-an-arduino/#Project_7_Control_a_servo_motor_with_a_FSR) from the ITP Physical Computing website.

My intent was to juxtapose two different types of visual gauge to represent the amount of pressure applied to a force sensing resister:

- a row of LEDs that communicate force based on the number of LEDs illuminated

- a disk-gauge with a cutout window that rotates in proportion to the amount of force applied

When at rest, the cutout in the disk-gauge reveals an invitation to "SQUEEZE ME" (which, now that I think of it, doesn't make it what exactly is meant to be squeezed) and the first LED blinks (more on that later).

As more force is applied, the LEDs illuminate in sequence (three green, two yellow, one red) and the disk-gauge rotates clock-wise through 180 degrees of motion.

When the amount of pressure applied to the sensor is at it's maximum, the cutout window in the disk-gauge reveals the message "OUCH!" and all the LEDs are illuminated with the red LED blinking rapidly.

Video below:

<iframe width="640" height="480" src="https://drive.google.com/file/d/10kZh0xaVQAj7Rjji4U3JF3TBkrJs9Qxm/preview" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

The part of this exercise I found most befuddling was the rapidly blinking green LED when the project is at risk, and the rapidly blinking red LED when the maximum amount of pressure is applied to the force sensing resistor. The unintended blinking goes away when the servo is disconnected from the breadboard so I have to imagine it has something to do with the pulse width modulation that drives the servo, but I have no idea how to identify the actually source of the problem.

Lastly, early on in Intro to Computational Media we're taught recurring patterns are good indicators that code can be better optimized. The code as it's written sends LOW and HIGH outputs to a row of six LEDs for each additional 1/6th of the total range that measures force applied to the sensor.

```
// if pressure sensor value is 0, turn off all LEDs
if (analogValue <= 0) {
  digitalWrite(2, LOW);
  digitalWrite(3, LOW);
  digitalWrite(4, LOW);
  digitalWrite(5, LOW);
  digitalWrite(6, LOW);
  digitalWrite(7, LOW);
}

// if pressure sensor value increases, turn one LED on
else if (analogValue >= 10 && analogValue <= 150) {
  digitalWrite(2, HIGH);
  digitalWrite(3, LOW);
  digitalWrite(4, LOW);
  digitalWrite(5, LOW);
  digitalWrite(6, LOW);
  digitalWrite(7, LOW);
}

// if pressure sensor value increases, turn a second LED on
else if (analogValue >= 150 * 2 && analogValue <= 150 * 3) {
  digitalWrite(2, HIGH);
  digitalWrite(3, HIGH);
  digitalWrite(4, LOW);
  digitalWrite(5, LOW);
  digitalWrite(6, LOW);
  digitalWrite(7, LOW);
}

// if pressure sensor value increases, turn third LEDs on
else if (analogValue >= 150 * 3 && analogValue <= 150 * 4) {
  digitalWrite(2, HIGH);
  digitalWrite(3, HIGH);
  digitalWrite(4, HIGH);
  digitalWrite(5, LOW);
  digitalWrite(6, LOW);
  digitalWrite(7, LOW);
}

// if pressure sensor value increases, turn a fourth LED on
else if (analogValue >= 150 * 4 && analogValue <= 150 * 5) {
  digitalWrite(2, HIGH);
  digitalWrite(3, HIGH);
  digitalWrite(4, HIGH);
  digitalWrite(5, HIGH);
  digitalWrite(6, LOW);
  digitalWrite(7, LOW);
}

// if pressure sensor value increases, turn a fifth LED on
else if (analogValue >= 150 * 5 && analogValue <= 150 * 6) {
  digitalWrite(2, HIGH);
  digitalWrite(3, HIGH);
  digitalWrite(4, HIGH);
  digitalWrite(5, HIGH);
  digitalWrite(6, HIGH);
}

// if pressure sensor value increases, turn all LEDs on
else if (analogValue >= 150 * 6 && analogValue <= 150 * 7) {
  digitalWrite(2, HIGH);
  digitalWrite(3, HIGH);
  digitalWrite(4, HIGH);
  digitalWrite(5, HIGH);
  digitalWrite(6, HIGH);
  digitalWrite(7, HIGH);
}
```

My guess is there's a smarter way to go about this but I'm unclear what that might be.
