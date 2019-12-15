---
layout:       single
title:        "Discomfort » Final: Proposal"
date:         2019-12-05 16:16:12 -05:00
categories:   Discomfort
---

Aaron and I are proposing to build an installation that uses glass as a catalyst for discomfort.

# Presentation

{% include video id="1UBuf19rRfRjopx-wqiFFuhnW4rMm5ApSNC-dbTgSD8o" provider="google-drive" %}

# Themes

Glass is a material rich with symbolic meaning.

The symbolism of shattered glass is perhaps most powerfully evoked by [Kristallnacht](https://encyclopedia.ushmm.org/content/en/article/kristallnacht), The Night of Broken Glass, during which the Nazis perpetrated systematic attacks on Jewish people and property.

We also drew inspiration from the idiom "people who live in a glass house should not throw stones", an admonition to refrain from judging others least we be judged ourselves, drawing a correlation between the materiality of glass (transparent frangible barriers separating inside from outside, private from public) and flaws in human nature.

# Concept

Aaron and I each identify three thoughts or memories, two of which are shameful, one of which is precious, all of which we prefer to keep private.

Each of the three pieces of information are recorded in a plain text file containing no more than 140 characters. The file is then encrypted from the terminal using OpenSSL:

`$ openssl aes-256-cbc -a -salt -in input.txt -out output.txt`

and the cipher text is then used to generate a [QR Code](https://www.qrcode-monkey.com/#text).

Lastly, each QR Code is etched onto a piece of 1/4" thick 8.5" x 11" plate glass.

Two guests are presented with the set of three and invited to choose one pane of glass each.

The host retrieves the first selection on behalf of the first guest and enters the installation alone.

He then emerges, asks the first guest to join him behind the curtain, explains the premise of the piece (there are three panes, two of which are shameful one of which is precious and he has no way of knowing which is which).

He hands the stone to the participant and asks that the glass be shattered. The first participant is then escorted out from behind the curtain and the experience is repeated.

![Wall of glass.](/assets/discomfort/2019-12-05/concept.jpg)

# Prototyping

In keeping with Nick’s feedback, we incorporated a single piece of identifying information (our photos) into the etch.

![Stack of etched panes of glass.](/assets/discomfort/2019-12-05/IMG_5377.JPG)

We’ve also prototyped the interaction to determine whether the experience can be run effectively in a confined space.

{% include video id="1gAsYWbZpPgm7IbUV1VFGlg7yGTx-sPn1" provider="google-drive" %}

We still have a few logistics to work out: finalizing our safety protocol; how best to document; and how best to conclude the experience.

The plan is to run the piece Thursday evening when we’re both free and the floor is generally quiet.

Our biggest question is how best to close the magic circle once the glass has been shattered.
