---
layout:       single
title:        "CAtN Assignment 03: Inform7"
date:         2019-10-17 10:19:15 -04:00
categories:   Narrative
---

The Inform IDE is utterly overwhelming. There's just so much there and I found it challenging to wrap my head around the complexity of the UI. I appreciate how sophisticated the IDE is, that it does all those things a good IDE is meant to do, but as an interface I found it frustrating and unintuitive. I get that the UI is meant to evoke a book with 'pages' facing one another but combining horizontal tabs along the top of the pane with vertical tabs along the side makes for a kind of cognitive dissonance. It reads like something built in Hypercard before we settled into some of the more ubiquitous UI design conventions.

It doesn't help that the built-in documentation is difficult to navigate and its efficacy further hindered by Nelson's verbose, 'clever' writing style. Most of the time I felt as though I was fighting the tool rather than it helping me to make the code, its syntax, and its potential accessible.

I wrote previously about a bias concerning literature vs. game. And while I was skeptical about Twine-like IF conventions (be it links embedded in the text -- I've always felt one of the challenges that faced hypertext as a literary form was how to interpolate navigation without embedding links directly into the text itself -- or worse, an explicit menu of choices at the bottom of the screen) I admit I was even more wary of the text adventure as literary form. But it was a reading from last week, The Fire Tower by Jacqueline A. Lott, that opened my mind to the literary possibility of a meditative, and thereby immersive, shared narrative experience.

I struggled with the station wagon. The obvious choice would be to make it a vehicle, especially since I'd like to have the player negotiate the last of the journey by road and make their way up the driveway but in this first draft the story begins at the top of a driveway so there's no need to drive any further. The car needs to contain items (and animals and persons) but should not be drivable up the front stairs and into the cabin. The compromise was to make it a container.

Likewise, I wrestled with how to have a person be carry-able. In this particular case, the person is a small child and so it seemed like a perfectly reasonable expectation but by default, Inform was having none of it:

```
>carry Eli
I don't suppose Eli would care for that.
```

Googling various permutations of "inform7 how to carry a person" got me nowhere. Eventually I did a search on the Interactive Fiction Community Forum using the query "[carry a person](https://intfiction.org/search?q=carry%20a%20person) and lo, [the solution](https://intfiction.org/t/carrying-a-person-verbose-debugging-output/2050) popped up as the first hit."

> I kind of like Inform 7, but I find the Natural Language “squishiness” kind of hard to reason about and to know what all the rules are.

I'll second that.
