---
layout:       single
title:        "ICM Week 08: Oh, the feedback"
date:         2018-10-29 22:14:17 -04:00
categories:   ICM
---

For [this week's assignment](https://editor.p5js.org/nopivnick/sketches/BJ9MnMBhm) I wanted to try and use an example from the [Introduction to Programming for the Visual Arts with p5.js](https://www.kadenze.com/courses/introduction-to-programming-for-the-visual-arts-with-p5-js-vi/) course on Kadenze as a point of departure.

<iframe src="https://drive.google.com/file/d/1CTL_W8o98op3uC4JZbl5nwV3nUmLysxB/preview" width="640" height="480"></iframe>

I ran into a few challenges before I could get to a point where I could start playing with the sketch.

I don't entirely understand McCarthy's logic for manipulating the `darkness` variable using the following line of code:

```javascript
let darkness = (255 - webCam.pixels[i + 4]) / 255;
```

I gather she's adding 4 to each index of the pixel array in order (presumably) to work only with the pixel's alpha value but I don't understand how using the alpha value from a plain web cam capture gets you anything other than 1 (i.e., total opacity).

The feedback from the feedback from the laptop mic and the way it adjusts the size of the ellipses is sort of a happy accident. I understand that by default, `createCapture()` without arguments defaults to capturing both video and sound, and I'm able to turn the input from the mic on and off (`webMic.start`, `webMic.stop`) and therefor whether it effects the sketch, but I was unable to figure out how to stop the mic capture itself.

It seems like one might somehow toggle between `createCapture()` and `createCapture(VIDEO)`.

Lastly, even though it wouldn't actually turn off the audio capture (and therefor the feedback), nevertheless wasn't able to get my `toggleMicInput()` function to work correctly.

Were it working correctly, at the very least the button text should change.

```javascript
// A function to start and stop sound input from the mic
function toggleMicInput() {
  if (feedback == false) {
    webMic.stop();
    button.html('Gimmie Some Feedback!');

  } else {
    webMic.start();
    button.html('Stop The Madness!');

  }
  feedback = !feedback;

}
```
