---
layout:       single
title:        "CAtN » Final"
date:         2019-12-12 09:34:11 -05:00
categories:   Narrative
---

For my final, it was a toss up between refining either [my Inform sketch](https://itp.nopivnick.com/narrative/first-inform/) or [my Twine sketch](https://itp.nopivnick.com/narrative/first-twine/).

My intention for the Inform piece was to have the interactor (to use the IF parlance) enact a  sequence of steps required to open a cabin during dead of winter, put a child to bed, and start a fire to heat the house. A kind of internal narrative tied to the plodding steps of routine getting situated after a long drive.

I opted instead for the Twine.

Arguably, the Twine piece puts more emphasis on narrative than it does on computation. It's not a branching narrative. There is no web of links between and among various passages. Looking at the story map it's plain to see it's just a loop, albeit stylized.

![Twine's Story Map view](/assets/catn/2019-12-12/luna_story-map.png)

The Twine piece is meant to evoke the withering of a relationship over time using cycles of repetition punctuated by shift in tone, and to have that shift seep through the screen subliminally each time the reader passes through the loop. I like the idea of the loop as a kind of narrative resignation.

The change in tone is sinusoidal. It oscillates between euphoric, mundane, somber, mundane then back to euphoric. The transition happens after every complete cycle of the narrative loop using modulo.

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

This has been an interesting exercise given my focus on hypertext narrative in undergrad. The authoring environment we used at the time offered no programmatic way to change the text within a given lexia (or node, or passage) over the course of the reading. Yes, there were conditionals, but they were assigned exclusively to links and therefor navigation. The text in any given lexia was fixed.

If one wanted to affect a subtle change based on what came before, you had two choices: either maintain separate lexia that contain slight variations on a given passage, or write in such a way as to accommodate different readings of the same text depending on context.

Like StorySpace, Twine makes easy enough work of the first option. Unlike StorySpace, Twine makes it possible to programmatically alter the content of a given passage over the course of a reading through the use of variables, conditionals, loops, or any combination thereof.

The second option, however, is less about computation than it is about artistry. It employs a particular kind of composition, a carefully crafted fluidity of interpretation (an inexactness, perhaps) that magically conforms to any and all lexia from which the reader might possibly arrive.

From a workability standpoint, this second approach was the more practicable. Easier to keep track of the shape of the text. Easier to 'debug'. But from a narrative standpoint, it was actually [the more interesting challenge](https://www.wired.com/2013/04/hypertext/). It required one to think about one's writing -- structurally, aesthetically, and thematically -- in a different way. It was, I would argue, at the core of what imbued hypertext with the promise of a new literary form.

Twine is a more robust tool than StorySpace, no doubt. And it's worth emphasizing the unlimited potential that comes with open standards and [extensibility](https://twinelab.net/custom-macros-for-sugarcube-2/). And yet, I can't help but feel Twine's syntax is somehow disruptive, if not destructive, to the text itself. On the one hand, it's possible to accomplish great variation with using syntax that is approachable. On the other hand, that very same syntax does a kind of violence to the writing, making it difficult to actually compose in the editor.

My final is [here](http://j.mp/2PfpTgk).
