---
layout:       single
title:        "Haptics » Final"
date:         2018-11-03 17:14:37 -04:00
categories:   Haptics
---

Our group from the Haptic Buffet session had a blast and it was clear for the Final Project assignment everyone was keen to further refine our [cat-petting simulation](https://itp.nopivnick.com/haptics/haptics-experiment-4/).

There were a number of particulars everyone agreed needed attention, the first being a change in the arrangement of sensors and vibration motors:

<iframe src="https://drive.google.com/file/d/1cspl0I_DWWgoFYuR_mHStVvITEcUGoBe/preview" width="640" height="480"></iframe>

Our thinking was the interaction was best if the sensors were in a position to take their readings prior to any vibrating motors.

We also thought it would be better to embed the vibrating motors flush with the surface of the cardboard on which they were mounted, if for no other reason than the user would be less distracted by their profile while administering the petting:

<iframe src="https://drive.google.com/file/d/1oMGOoFJFR1geTDEmMHULVVoml9QtVALm/preview" width="640" height="480"></iframe>

It was a nice idea, and certainly made for a cleaner presentation, but unfortunately it significantly diminished the intensity of the vibration traveling through the cardboard substrate.

Perhaps if the fit had been more precise it would have been different but I suspect given their design it's better for the motors to be surface mounted rather than embedded. If I had to guess, I'd say it's probably something to do with the way the actuator moves inside the motor.

Next, in an attempt to clean up our wiring, we borrowed a wire wrapper and went to town:

<iframe src="https://drive.google.com/file/d/1-96NVsDlwuNyzBkvb54RLCwhOUElJ1eT/preview" width="640" height="480"></iframe>

Converts, all!

Inspired by our new found respect for wire wrapping elegance, and loath to try soldering headers to the delicate leads of the vibrating motors, I had a flash of inspiration and managed to wrap the motor leads themselves directly to the header pins. The world of haptics will never be the same.

<iframe src="https://drive.google.com/file/d/1dHRkqfYMUzeKSnDxtKLyV24nrMCR7Rxs/preview" width="640" height="480"></iframe>

Here's our final version of the code:

```c++
//vibrating motor pins
#define PINA 11
#define PINB 10
#define PINC 9

//force sensor pins
int FSRA = A0;
int FSRB = A1;


int buzzThreshold = 255;      //max strength of cat purr
int petThreshold = 40;
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


  //Easy option: nested if
  if (fsrAreading >= petThreshold) {
    Serial.println("first sensor stroked");
    if (fsrBreading >= petThreshold) {
      Serial.println("second sensor stroked too");
      pettingNow = true;
      if (pettingNow == true) {
        purr(); purr(); purr();
        pettingNow = false;
      }
    }
  }
}

void purr() {
  for (int fadeValue = 0 ; fadeValue <= buzzThreshold; fadeValue += 4) {
    // sets the value (range from 0 to 255):
    analogWrite(PINA, fadeValue);
    analogWrite(PINB, fadeValue);
    analogWrite(PINC, fadeValue);
    // wait for 30 milliseconds to see the dimming effect
    delay(20);
  }

  // fade out from max to min in increments of 5 points:
  for (int fadeValue = buzzThreshold ; fadeValue >= 0; fadeValue -= 4) {
    // sets the value (range from 0 to 255):
    analogWrite(PINA, fadeValue);
    analogWrite(PINB, fadeValue);
    analogWrite(PINC, fadeValue);
    // wait for 30 milliseconds to see the dimming effect
    delay(20);
  }
}
```

Caleb's first pass from the Haptic Buffet session involved measuring a threshold based on a combined reading from the two force sensing resistors however because we wanted to limit the triggered vibration to a stroke administered in only one direction, he collapsed the two conditionals into a nested `if` statement.

In retrospect, it occurred to me I had assumed we'd used one of the effects defined by the Arduino library that accompanies the [Adafruit DRV2605L Haptic Motor Controllers](https://www.adafruit.com/product/2305) but in actuality Caleb had written code that simulated the rise and fall of a cat's purr.

And lastly, I insisted we add a tail so Beverly made fast work at the soft lab and demonstrated some fine sewing skills:

<iframe src="https://drive.google.com/file/d/1ri4b6KOORObG4nSoWMHYWtsEZoucNkez/preview" width="640" height="480"></iframe>

I'm convinced the tail and the cleaner wiring adds to the anthropomorphization and as a consequence improves the interaction.

On that note, I would like to point out -- for the record -- this project received the Tom Igoe stamp of approval (replete with smile, no less) but tragically I was not fast enough to capture it on camera.
