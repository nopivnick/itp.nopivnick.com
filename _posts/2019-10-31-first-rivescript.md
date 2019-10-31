---
layout:       single
title:        "CAtN » Sketch 04: RiveScript"
date:         2019-10-31 10:09:19 -04:00
categories:   Narrative
classes:      wide
---

So I botched this assignment.

I opted to create a chatbot with [RiveScript](https://www.rivescript.com/) rather than explore Ren'Py.

Getting started was a grind. The p5 web editor refuses to upload .rive files. Some Googling surfaced [this issue](https://github.com/processing/p5.js-web-editor/issues/534) in which Shiffman suggested changing the file extension to .txt, reasoning that .rive files are just plain text. The solution didn't appear to work and I lost further time following a red herring (a console error related to a browser extension, of all things) until finally realizing I'd neglected to change the name of the rive script in the sketch.js to match the .txt extension workaround.

There were other issues as well. Schiffman's RiveScript chatbot tutorial repo was based on RiveScript version 1.9 and I lost a few hours troubleshooting RiveScript code inspired by the official RiveScript tutorial before eventually realizing the 2.0 release -- on which the official tutorial is based -- was a major overhaul and the p5 sketch I'd used as a point of departure was to blame.

Feh. This post reads like a list of excuses from an ICM noob.

I've been trying to think about why I dismissed Ren'Py to begin with, and then why I was so utterly and completely devoid of inspiration when trying to write the chatbot, at least in the context of this class.

I don't understand the appeal of Ren'Py visual novels. They don't feel 'interactive' in any substantive sense, though that's such a lazy statement I'm embarrassed to write it. Visual novels strike me as mindless, click-stepping through branches of dialog and picking from a list of choices at the bottom of the screen. The former feels like it lacks any engagement whatsoever and I've mentioned in earlier posts how I find the latter -- at least in the context of IF -- to be jarring and disruptive to narrative flow.

The point, I think, is I made the mistake of assuming a chatbot would be the more engaing if not 'interactive' experience. The user types input, the bot spits up output. Is that not interactive? I imagine I was seduced by some notion of something resembling an exchange between the reader and the bot but that's not at all the case. In actuality, it's just as maddening trying to write a bot than it is trying to reason with one. It's not that RiveScript syntax is inflexible, or even that it's tedious (which it is), it's that chatbots are just so ... god damn stupid in their required specificity. No doubt this has much to do with exploring the simpler model (retrieval vs generative) but the coding, to say nothing of the chatting, doesn't feel like a creative act. It's weirdly about trying to build something predicated on confinement and vaguery. The worst part of it is the temptation to resort to snark as a cover for how inflexible the bot is. Just all around unpleasant.

I choose stupidly. I should have played with Ren'Py.

Chatbot is [here](http://j.mp/2PyPRvC).
