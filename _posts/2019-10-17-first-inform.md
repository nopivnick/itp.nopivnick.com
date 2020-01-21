---
layout:       single
title:        "CAtN » Sketch 3: Inform"
date:         2019-10-17 10:19:15 -04:00
categories:   Narrative
---

The Inform IDE is utterly overwhelming. There's just so much there and I found it challenging to wrap my head around the complexity of the UI. I appreciate how sophisticated the IDE is, that it does all those things a good IDE is meant to do, but as an interface I found it frustrating and unintuitive.

I appreciate that the UI is meant to evoke a book with 'pages' facing one another but if you're going to skeuomorph [the codex as a UI](https://www.youtube.com/watch?v=pQHX-SjgQvQ) it's a strange choice to have both sides of the UI render the same features / functionality. I also found the simultaneity of horizontal tabs along the top of the pane with vertical tabs down the side makes for a kind of cognitive dissonance. I can't help but feel like the IDE 'reads' like something built in [Hypercard](https://arstechnica.com/gadgets/2019/05/25-years-of-hypercard-the-missing-link-to-the-web/) before we settled on some of the more ubiquitous UI design conventions for tools. Perhaps that was a conscious decision?

It doesn't help that the built-in documentation is difficult to navigate and I I found its usefulness further hindered by Nelson's somewhat verbose, 'clever' writing style. Most of the time I felt the tool was fighting me rather than helping me.

I wrote previously about my own bias concerning literature vs. game. I'm generally skeptical of Twine-like IF conventions, though I admit underlying most navigational schemes is the hypertext link. I've always felt one of the challenges that faces hypertext as a literary form and a reading experience is how to evaluate navigation without embedding links directly into the text itself. How to evoke a kind of narrative precognition. Admittedly, that's a kind of aesthetic principal. Even more disruptive than embedded links is the list of explicit choices at the bottom of the screen. It's jarring.

I was even more wary of the text adventure as literary form. But it was a reading from last week, [The Fire Tower](https://ifdb.tads.org/viewgame?id=fcm1ikz9ttr6i99a) by Jacqueline A. Lott, that opened my mind to the literary possibility of a meditative, and in that way immersive, narrative experience.

I struggled with the station wagon in my piece. The obvious choice was to make it a vehicle, especially since I'd like to have the interactor (to use the term formalized by Nick Montfort) negotiate making their way up the driveway by vehicle but in this first draft the story begins at the top of a driveway so there's no need to drive any further. The car needs to contain items (and animals and persons) but should not be drivable up the front stairs and into the cabin so the compromise was to make it a container.

I also wrestled with how to have a person be carry-able. In this particular case, the person is a small child and so it seemed like a perfectly reasonable expectation but Inform was having none of it:

```
>carry Eli
I don't suppose Eli would care for that.
```

Googling various permutations of "inform7 how to carry a person" got me nowhere but the Interactive Fiction Community Forum [proved an invaluable resource](https://intfiction.org/search?q=carry%20a%20person) and lo, [the solution](https://intfiction.org/t/carrying-a-person-verbose-debugging-output/2050) popped up as the first hit.

To quote the individual who posted the question:

> I kind of like Inform 7, but I find the Natural Language “squishiness” kind of hard to reason about and to know what all the rules are.

I'll second that.
