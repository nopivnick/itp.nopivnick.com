---
layout:       single
title:        "Haptics Experiment 4: Haptics Buffet"
date:         2018-11-03 17:14:30 -04:00
categories:   Haptics
---

#

Beverly Chou, Caleb Cloitre-Ferguson, Heng Tang, Lin Zhang, and I gathered on Sunday to explore the Touch Triggered Vibration prompt.

As I recall the group struggled a bit coming up with a concept, which was surprising since touch and haptics are such close kin. Force Sensing Resistor (the [Interlink FSP 406](https://www.interlinkelectronics.com/fsr-406)) seemed an obvious choice as a means to trigger, that much was clear at least:

<iframe src="https://drive.google.com/file/d/1JVZhsjAuTHD57mCWQZbBe8ClNmy77lv3/preview" width="640" height="480"></iframe>

Eventually someone brought up petting, which led to cats and the inescapable correlation between purring and vibration. Finally, a scrap of faux fur I scrounged from someone's cast-off Halloween-themed PhysComp midterm sealed the deal.

Caleb was quick to pull together some code but the part of the build that required the most time was wiring, specifically the bare leads on the vibrating motors and the similarly fragile leads on the force sensing resistors:

<iframe src="https://drive.google.com/file/d/1sJixCtp2RH2TJ1MNjt2ekyVkRICH9nI6/preview" width="640" height="480"></iframe>

On the fabrication side, our initial assumption (or perhaps it's more accurate to say *my* initial assumption since I was first to start fabricating) was to use an even distribution of sensors and motors alternating at regular intervals and 'petting' would be measured as a function of readings taken from two FSRs:

<iframe src="https://drive.google.com/file/d/1kayRywAHwLp4LEHkhfzC83cgmA2z_t47/preview" width="640" height="480"></iframe>

I seem to recall it was important to everyone that the technology be as visibly 'out of the way' as possible and that required getting the Arduino and breadboard some distance from the interface. If I had to venture a guess I'd say it was anthropomorphization at play, which is perhaps a testament to how quickly one can cathect to an object based solely on a swath of (faux) fur.

<iframe src="https://drive.google.com/file/d/1agi8_TtXTvwlart1HuP82K-eCA9QO7lm/preview" width="640" height="480"></iframe>

From a sensory standpoint the interaction is undeniably pleasant but what I found most satisfying about the exercise is it seemed to hit multiple notes: it was tactile (the fur), haptic (the vibrating motors), and triggered by touch (the force sensing resistors).

Speaking only for myself, it was especially rewarding to see Antonio check out the piece, since these are ways of perceiving that play such a prominent role in how he negotiates the world around him:

<iframe src="https://drive.google.com/file/d/1CAqCyP_koEEysTQl8mz5jbC2RrCjEaky/preview" width="640" height="480"></iframe>

Below is the code as it was written by end of the weekend session:

```C++
//vibrating motor pins
#define PINA 11
#define PINB 10
#define PINC 9

//force sensor pins
int FSRA = A0;
int FSRB = A1;


int threshold = 255;      //max strength of cat purr
int fsrAreading;          // the analog reading from the FSR resistor divider
int fsrBreading;          // t  he analog reading from the FSR resistor divider
bool pettingNow = false;

void setup() {
  Serial.begin(9600);   // We'll send debugging information via the Serial monitor

}

void loop() {
  fsrAreading = analogRead(FSRA);
  fsrBreading = analogRead(FSRB);
  Serial.print("A reading: ");
  Serial.print(fsrAreading);
  Serial.print("  | B reading: ");
  Serial.println(fsrBreading);

  if (fsrAreading + fsrBreading >=70) {
    pettingNow = true;
    Serial.println("combo sensor threshold");
  }
  if (pettingNow == true) {
    purr();
    pettingNow = false;
  }
}

void purr() {
  for (int fadeValue = 0 ; fadeValue <= threshold; fadeValue += 10) {
    // sets the value (range from 0 to 255):
    analogWrite(PINA, fadeValue);
    analogWrite(PINB, fadeValue);
    analogWrite(PINC, fadeValue);
    // wait for 30 milliseconds to see the dimming effect
    delay(20);
  }

  // fade out from max to min in increments of 5 points:
  for (int fadeValue = threshold ; fadeValue >= 0; fadeValue -= 10) {
    // sets the value (range from 0 to 255):
    analogWrite(PINA, fadeValue);
    analogWrite(PINB, fadeValue);
    analogWrite(PINC, fadeValue);
    // wait for 30 milliseconds to see the dimming effect
    delay(20);
  }
}
```

Go team!

<iframe src="https://drive.google.com/file/d/1BN610GyVH4g9SUBjH46S9guB3kv11i6o/preview" width="640" height="480"></iframe>
