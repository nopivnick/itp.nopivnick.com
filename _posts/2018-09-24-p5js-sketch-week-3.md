---
layout:       single
title:        "ICM Week 3: My Third P5js Sketch"
date:         2018-09-24 09:27:09 -04:00
categories:   ICM
---

As was the case last week, [this week's sketch](https://editor.p5js.org/nopivnick/sketches/BkDS_fSK7) continues to build on my [first assignment](https://editor.p5js.org/nopivnick/sketches/S1hqLOEum).

The sun now rises and falls as it should, thanks to some conditional logic:

```javascript
// move sun
sunHeight -= sunSpeed;
// reverse sun at edge of canvas
if (sunHeight < 0 || sunHeight > height) sunSpeed *= -1;
```

I'd also intended to have the `sunSpeed` slow down in proportion to it's distance from the edge of the canvas but when I implement this line of code:

```javascript
// TODO: slow sun speed in proportion to distance from canvas edge
sunSpeed = sunHeight / 100
```
while the sun does slow as it approaches the top of the canvas, it doesn't abide by the conditional logic above and reverse direction. I suspect it has something to do with the fact that `sunHeight` divided by some number never actually reaches 0.

Need to figure that one out.

I created a rudimentary button (with a simple color change rollover) to reverse the direction of the sun:

```javascript
// button to reverse sunSpeed
// declare and assign button variables
let fastButtonX = 335;
let fastButtonY = 335;
let fastButtonWidth = 50;
let fastButtonHeight = 50;
push();
noStroke();
fill('red');
// make fast-forward button a rollover
if ((mouseX > fastButtonX) && (mouseX < fastButtonX + fastButtonWidth) && (mouseY > fastButtonY) && (mouseY < fastButtonY + fastButtonHeight)) {
  fill(125);
  if (mouseIsPressed) {
    sunSpeed = sunSpeed * -1
  }
}
// draw fast-forward button
rect(fastButtonX, fastButtonY, fastButtonWidth, fastButtonHeight);
pop();
}
```

But for some reason `sunSpeed` doesn't change direction with 100% reliability.

Need to figure that one out too.

I used a `for` loop to generate the star field:

```javascript
// draw star field
for (starLatitude = 0; starLatitude < width; starLatitude += random(width)) {
  for (starLongitude = 0; starLongitude < height; starLongitude += random(height)) {
    noStroke();
    fill(255);
    ellipse(starLatitude, starLongitude, 5, 5);
  }
}
```

but I'm not convinced it's the best use of a `for` loop, at least not the way I implemented things.

My intention had been to create a random distribution of stars throughout the sky but as it's written, the code is is sort of at odds with itself.

By that I mean the results of the `for` loop are generated every frame, and because their coordinates are randomized every frame, they do not appear to be fixed in place.

Thinking to myself "this is something that should be done once at the start of the sketch", it seemed one solution might be to stick the star field loop in `setup()` but clearly that won't work for objects that need to be drawn (hence belong in the `draw()` function).

The 'I meant to do that' justification in this case would be to say "yes, of course, the stars are twinkling! but I know there's a better (i.e., cheaper, more efficient) way to accomplish that effect.

I'm guessing the way to accomplish a field of stars with a random distribution but remain fixed in place has to do with arrays.

As a side note, I tried embedding the sketch in the instance of my blog that runs locally for testing purposes and the cooling fans on my laptop spun up and the machine became increasingly warm to the touch. This may be a real-world indication that my code is grossly inefficient.

And finally, I'm beginning to question the decision to implement each week's assignment by building on the prior week's sketch. The code is becoming increasingly busy and I'm left wondering how long before 'monkey patching' older code becomes more trouble than it's worth. I suppose at some point everyone has to make the decision whether to continue refactoring existing code or build from scratch.
