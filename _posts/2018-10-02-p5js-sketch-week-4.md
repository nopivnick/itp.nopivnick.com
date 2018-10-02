---
layout:       single
title:        "ICM Week 4: My Fourth P5js Sketch"
date:         2018-10-02 08:25:56 -04:00
categories:   ICM
---

[This week's sketch](https://editor.p5js.org/nopivnick/sketches/r1p1qu6tm) was an exercise in object-oriented programming.

At the root of the project directory is `Balloon.js`, the file that contains the code that defines the balloon object:

```javascript
class Balloon {
  constructor(balloonX, balloonY, balloonWidth, balloonHeight) {
    this.balloonX = balloonX;
    this.balloonY = balloonY;
    this.balloonWidth = balloonWidth;
    this.balloonHeight = balloonHeight;
  }

  run() {
    this.render();
  }

  render() {
    //draw a balloon
    push(); // start balloon position translation
    translate(this.balloonX, this.balloonY)
    // draw a string
    stroke('white');
    line(0, 0 + this.balloonHeight * 0.5, 0, 0 + 150);
    // draw a bag of gas
    fill('red');
    noStroke();
    ellipse(0, 0, this.balloonWidth, this.balloonHeight);
    // draw a knot
    triangle(0, 0 + this.balloonHeight * 0.5, 0 + 5, this.balloonHeight * 0.5 + 5, 0 - 5, this.balloonHeight * 0.5 + 5);
    pop(); // conclude balloon position translation
  }
}
```

Currently the Balloon object has only four properties, two of which in combination define its location on the canvas (`this.balloonX, this.balloonY`), the other two its dimensions (`this.balloonWidth, this.balloonHeight`).

Any self-respecting Balloon object ought to have additional properties that define its balloony-ness. Here are a few that come to mind:

- `color`
- `durability`
- `static charge`
- `airtight-ness`
- `inflated-ness`
- `weight`

`Inflated-ness` and `weight` are interesting propositions. In the balloon world they are closely related to one another, as are they both to size (dimensions). The more inflated the balloon, the larger its size. A balloon behaves differently depending upon what the balloon is inflated with (i.e., air vs helium), which in turn determines its buoyancy but not its weight. I'm guessing buoyancy is probably better described as behavior and therefor a method, but I'm not sure.

If the code is meant to be a simulation, how complex does one make ones object in the interest of fidelity? Perhaps the more important question is how to determine the degree of complexity required to provide a satisfactory interaction? I suppose you could model the molecular integrity of the polymers in the rubber from which the balloon is made but short of modeling the manufacturing process to determine cost benefit analyses, that would be ridiculous.

What determines when an object has become sufficiently object-ified? Presumably as it becomes more complex a greater number of it's properties can be extrapolated as objects in and of themselves (a Balloon-object is made of rubber which, as a material, might just as well constitute a separate object). I get that object-oriented coding facilitates modularity and re-use but are there performance gains and if so, is there a point of diminishing returns?

I'd like to turn this sketch into a game. The game begins with a number of balloons distributed on the canvas and a mouse cursor with a 'static charge'. Once the game starts, the player is required to keep their cursor within the confines of the canvas at all times, circumnavigating balloons which have varying degrees of attraction. Perhaps the cursor is a reluctant bee with a stinger. Perhaps there's one elusive balloon that actually requires popping.

A simple game like the one described above probably ought to have a random distribution of balloons (and their respective properties) at the start of the game. As I suspected working on last week's sketch, I now know that requires an array so building on this sketch ought to mesh nicely with this coming week's assignment.
