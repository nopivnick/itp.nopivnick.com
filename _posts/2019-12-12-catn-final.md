---
layout:       single
title:        "CAtN » Final"
date:         2019-12-12 09:34:11 -05:00
categories:   Narrative
---

For my final, it was a toss up between refining either the [Inform](https://itp.nopivnick.com/narrative/first-inform/) or the [Twine](https://itp.nopivnick.com/narrative/first-twine/) assignment.

My intention for the Inform piece was to have the interactor (to use the parlance) enact an incremental sequence of actions required to open a cabin during dead of winter, put a child to bed, and start a fire to heat the house. A kind of internal narrative tied to the procedural steps in a fairly mundane process.

I opted instead for the Twine.

Admittedly, the Twine piece puts more emphasis on narrative than it does on computation. Ironically, it's not even a branching narrative. At least not explicitly. There is no web of links between and among various lexia. Looking at the story map, it's plain to see it's just a loop.

![Twine's Story Map view](/assets/catn/2019-12-12/luna_story-map.png)

My intention for the Twine piece was to describe the withering of a relationship over time using cycles of repetition punctuated by shift in tone, and to have that shift seep through each time the reader passes through the loop. I like the idea of the loop as a kind of narrative resignation.

The change in tone is sinusoidal, oscillating between euphoric, monotonous, somber, monotonous then back to euphoric. The transition happens after every complete cycle of the narrative loop using modulo.

```
<<if visited() % 4 is 3>>
/* Bad */
"Bad"
<<elseif visited() % 2 is 0>>
/* Blah */
"Blah"
<<elseif visited() % 4 is 1>>
/* Bliss */
"Bliss"
<</if>>
```

This has been an interesting exercise for me given my focus on hypertext narrative in undergrad. At the time there was no programatic way to change the text within a given node (or lexia, or passage) over the course of the reading. Yes, there were conditionals, but they were assigned to links. The text in any given node was static.

If one wanted to affect a subtle shift in either tone or meaning depending on where in the text the reader had arrived from, you had two choices: either maintain separate lexia that portray  variations on a given node, or write in such a way as to accommodate different readings of the same text depending on it's context (where you've come from, where you land next).

Like any good tool, Twine makes easy work of the first option through the use of variables, conditionals, loops, or some combination thereof. Unlike before, it's now possible to morph the content of a passage over the course of a reading procedurally.

The second option employed a kind of carefully crafted ambiguity that accommodated any and all nodes from which the reader might possibly arrive. From a purely practical standpoint, this was the better choice. Easier to keep track of the shape of the text and certainly easier to 'debug'. But in many regards, it was in fact the more interesting challenge. It made one think about one's writing -- structurally, aesthetically, and thematically -- in a different way.

Twine is the more powerful tool than StorySpace, no doubt. But I wonder whether it's significant (if not somehow symbolic) that Twine's syntax is actually disruptive to the writing process itself. On the one hand it's easier to accomplish complexity using syntax that is approachable. On the other hand that very same syntax does a kind of violence to the text, making it difficult to actually compose the writing in the editor.

Final is [here](http://j.mp/2PfpTgk).