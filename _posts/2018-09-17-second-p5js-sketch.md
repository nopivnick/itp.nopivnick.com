---
layout:       single
title:        "ICM » Week 2: Second p5 Sketch"
date:         2018-09-17 22:02:01 -04:00
categories:   ICM
---

The specifics of the code aside, I feel like I'm beginning to think about how best to break down intention into smaller chunks of code that can be tested incrementally without getting lost only to then resort to deleting an entire block of code and starting again.

In that respect, it was the second question of this week's quiz more so than the week's assignment that got me thinking about efficiency as opposed to 'just make it work'.

# Quiz 2, Question 2

> 2. Move a circle from the middle of the screen to the right of the screen.
>   - How do I make it move to the left of the screen?
>   - Towards each of the 4 corners?
>   - 10 times faster?

The second question on Quiz 2 seemed like an opportunity to try working with javascript objects. I thought perhaps objects would be a more elegant solution for creating a sketch that could be run *once* and demonstrate the different variations of the exercise simultaneously without asking the viewer to comment out one block of code, un-comment another, and run the sketch again.

```javascript
let circleToLeft = {
  x: 200,
  y: 200,
  size: 50,
  speed: 1,

};

let circleToRight = {
  x: 200,
  y: 200,
  size: 50,
  speed: 1,

};

let circleToTopLeft = {
  x: 200,
  y: 200,
  size: 50,
  speed: -10,

};
```

I realized after the fact there really isn't any direct correlation between javascript objects and a sketch that demonstrates multiple variations of the same task. Turning a circle into an object is a worthy enough exercise but I suspect I made (at least) two fundamentally flawed assumptions:

1. I probably should have incorporated all aspects of behavior (direction as well as speed) into the object.

2. Constructing a separate object for each ball based on it's behavior sort of belies the whole point of object oriented code (I think?). That is to say, there probably should have been just the one object 'circle', not an object for each variation on direction + speed.

# Assignment

I opted to continue to work on a [duplicate](https://editor.p5js.org/nopivnick/sketches/ryCRpUad7) of last week's self-portrait.

It's crude and unsophisticated but I like the idea of iterating on the original sketch, making tweaks, adding new elements, improving code, etc., and having there be a clear progression with an obvious trajectory over the course of the semester.

For the moment, the sun only rises and drifts off into the heavens, never to return. I know what's needed is some conditional logic, at which point the sun will rise and set in a loop, ad infinitum.

Using `map();` was interesting from the stand point of animation. Mapping the height of the sun to the ambient light of the sky and the ground was fairly straight forward but the 'transition lenses' effect was more effective when mapped to the latter quarter of the `sunHeight` range.

```javascript
// transition lenses
lensTint = map(sunHeight, 100, 0, 255, 0);
lensTint = lensTint + 1;

// sky color
skyBlue = map(sunHeight, height, 0, 0, 255);

// earth color
earthGreen = map(sunHeight, height, 0, 0, 255);
```

It's a small thing but I like how it more effectively communicates cause and effect. It's a tiny little thing but it helps inform the narrative (simple story though it may be).
