---
layout:       single
title:        "ICM Week 05: My Fifth P5.js Sketch"
date:         2018-10-08 21:29:03 -04:00
categories:   ICM
---

# Something visual

My sketch code for this week can be found [here](https://editor.p5js.org/nopivnick/sketches/rydyCzKqm).

<iframe src="https://editor.p5js.org/embed/rydyCzKqm"></iframe>

# Some thoughts

I'm desperate for a single, all encompassing, P5.js resource that's authoritative, up-to-date, and provides both instruction and examples that are current and don't conflict one another.

The Coding Train videos are certainly entertaining but the thing about video is it doesn't lend itself to easy navigation and so requires repeat viewing -- often in it's entirety -- just to track down the relevant bits.

So I picked up the McCarthy text, [Getting Started with P5.js](https://www.amazon.com/Make-Interactive-Graphics-JavaScript-Processing/dp/1457186772), and found it to be a more effective learning tool but for the fact that it's examples are out of date, as are the examples on the P5.js website, and the more advanced topics are out of date. It's difficult enough to wrap my head around things as it is without having to navigate conflicting information.

I appreciate that tech, code in particular, is in a constant state of change. And I know first hand the often mandatory tedium of Googling for answers. But it sucks constantly struggling just to get a leg up when the curriculum is so accelerated.

So that's me venting in the face of abject failure.

# Some code

My intention had been to mimic -- in very simple terms -- a mechanic from the game [Osmos](https://www.osmos-game.com/) in which 'motes' absorb one another when they come into contact. Which mote is the absorber and which the absorbee depends upon relative size.

I'm not entirely clear what this part of the assignment means:

> Re-organize / break-down your classes into the "smallest functional units" possible.

but I assume the idea is to create methods (i.e., behaviors) which accomplish the smallest possible thing. Modularity, and all that.

In this case, 'absorption' requires at least three things:

1. determine whether two objects are touching
2. if touching, the larger object increases it's size by some amount
3. the smaller object disappears

I managed to get 1. working using a method I called `touch()` in the `Balls` class:

```javascript
touch(positionOtherX, positionOtherY, otherRadius) {
  if (dist(positionOtherX, positionOtherY, this.ballPositionX, this.ballPositionY) < this.ballRadius) {
    return true;

  }
}
```

and I managed to get 3. working based on a comparison of the radii of the two `ball` objects when touching:

```javascript
// have every ball in the array check if it's touching another ball
for (let j = i; j < balls.length; j++) {

  // if i and j are not the same *and* touch is true
  if (i != j && balls[i].touch(balls[j].ballPositionX, balls[j].ballPositionY, balls[j].ballRadius) == true) {
    // determine which is the larger radius
    // and delete the ball with the smaller ballRadius

    if (balls[i].ballRadius > balls[j].ballRadius) {
      // absorb needs to happen here before the splice
      // balls[i].absorb(balls[i].ballRadius, balls[j].ballRadius);
      // in addition to any other method that relies on
      // other properties of the about-to-be deleted object
      balls.splice(j, 1);

    } else {
      // absorb happens here before the splice

      // in addition to any other method that relies on
      // other properties of the about-to-be deleted object
      balls.splice(i, 1);
    }
  }
}
```

but as you can see from the comments above and when running the sketch, I haven't been able to figure out how to implement the `absorb()` method.

Whether it's because the method is coded incorrectly or the method is called incorrectly in the sketch, or some combination of the two or something else entirely, I know not.
