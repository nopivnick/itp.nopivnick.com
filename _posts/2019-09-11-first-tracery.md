---
layout:       single
title:        "Computational Narrative Week 01: Tracery"
date:         2019-09-11 22:00:22 -04:00
categories:   Narrative
---

# Experience

We were tasked to find a story or genre we liked but I did that thing, that thing one does when one finds oneself paralyzed by choice. I defaulted to the narrative with which I am most familiar (my own).

My head is swimming from the Abbot excerpt. I'm poorly read and his text is so steeped in scholarship. It's true, what Allison said in class last week, that lingo like 'storytelling' is tossed about lazily. But so too 'narrative' and 'story'. It's discombobulating to be asked to think carefully about these terms, having used them so casually for so long.

I find plain text lovely. And tools that make use of plain text as raw material are so attractive. There's an elegance about Tracery. The rules are simple but the output rich with possibility though it seems to favor randomness which, to what little extent I understand game design, sort of complicates the notion of 'elegant'.

And on that note (randomness), I can't say I feel I addressed the meat of the assignment. I wrote a bit of narrative, stopping along the way to create lists of substitutions. But the substitutions were modest and maybe even inconsequential in so far as the extent to which they alter the discourse thereby change the shape of the story. Or vice versa.

# Source code

```javascript
{
  "origin": [
    "#subject.capitalize# #status# in a #dwelling# that floats above #whatsBelow#, built while #subject# was a #size# child in the #relativePlace# of the #industryType# industry. When the towers #undone#, #subject# #degreeOfCertainty# my life in #nycNicknames# was over. And so #subject# spends #duration# alone, #relating#, #doing# when #subject# can #gather# the energy or #doingWithTrees# trees when #subject# can't."
  ],
  "subject": [
    "I", "you", "she", "he", "they"
  ],
  "status": [
    "live", "die", "lay", "sit", "stand"
  ],
  "dwelling": [
    "room", "house", "hut", "cave", "boat", "world"
  ],
  "whatsBelow": [
    "clouds", "dirt", "ground", "sand", "water"
  ],
  "size": [
    "tiny", "small", "infant"
  ],
  "relativePlace": [
    "heart", "outskirts"
  ],
  "industryType": [
    "aerospace", "automotive", "coal", "defense", "dairy", "entertainment", "music", "pharmaceutical", "publishing", "sugar cane", "technology", "textile", "tobacco", "tourism"
  ],
  "undone": [
    "burned", "collapsed", "crumbled", "disappeared", "fell"
  ],
  "degreeOfCertainty": [
    "assumed", "knew", "realized", "suspected", "thought", "understood"
  ],
  "nycNicknames": [
    "the Big Apple", "the City of Dreams", "the City That Never Sleeps", "the Empire City", "Gotham"
  ],
  "duration": [
    "hours", "days", "weeks", "months", "years"
  ],
  "relating": [
    "talking", "singing", "whispering", "writing"
  ],
  "doing": [
    "dancing", "swimming", "running"
  ],
  "gather": [
    "summon", "find", "muster"
  ],
  "doingWithTrees": [
    "climbing", "sitting beneath", "swinging from"
  ]

};
```

# Output

<iframe src="https://editor.p5js.org/nopivnick/embed/xh6hhbOVo"></iframe>
