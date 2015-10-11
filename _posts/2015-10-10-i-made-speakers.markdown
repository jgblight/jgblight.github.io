---
layout: post
title:  "I made Speakers!"
date:   2015-10-10 18:00:00
---

![Finished Fab Speakers]({{site.url}}/assets/20151010_finished.jpg)

So I just finished making a small set of [Fab Speakers](http://diy-devices.com/devices/speakers/). I'm excited about this not because it's a particularly complicated project, nor because these are particularly high-quality speakers, but it represents the first time I've used soldering and/or 3D printing for something useful. The sound quality is decidedly OK. If you're the sort of person who plays music out of your laptop's built-in speakers, or if you regularly put your phone in a glass to make it louder, you may find that these speakers fill a gaping hole in your otherwise happy and fulfilling life; otherwise, they are nothing to write home about.

<!--more-->

The design for the speakers, including how to get the materials and detailed instructions for assembling them, are all available [here](http://diy-devices.com/devices/speakers/making-the-speakers/). The instructions are easy to follow and I won't reiterate them here. Instead, I will talk about the various ways in which I deviated from the instructions, and the various degrees of strife that it caused me.

![Speaker Circuit Board]({{site.url}}/assets/20151010_board.jpg)

First of all, I lost some of the surface mount resistors because everything is tiny and had to replace them with regular resistors, which is fine but slightly ugly. I also screwed up one of the chips on my first try, also because everything is tiny. Other than that, the board was easier than I expected.

![Speaker Drivers]({{site.url}}/assets/20151010_drivers.jpg)

Once I had finished the circuit, turning on the speakers for the first time was ... a decidedly disappointing experience. Turns out the enclosure matters a lot, a fact which I hadn't thought about at first but makes sense once you start to reason about how sound waves work. After all, the speaker drivers themselves are more or less just vibrating sheets of plastic, and vibrating plastic is useful only insofar as it can create vibrating columns of air. The enclosure suspends and secures the driver by the rim and creates air pressure behind it, both of which influence the driver's ability to push the air directly in front of it. Beyond that sort of basic intuition, real speaker design is incredibly complicated and anything that can be made in an afternoon on a kitchen table is certainly not optimal. Ultimately, the sound from the "naked" speakers served to lower my expectations appropriately. Every time I turned them on while I was assembling the enclosure, I was pleasantly surprised to find that the sound was more and more like actual music.

![3D Printed Pieces]({{site.url}}/assets/20151010_printed.jpg)

![Partially-Finished Enclosure]({{site.url}}/assets/20151010_assembled.jpg)

My biggest deviation from the original was that I printed the structural pieces for the enclosure, owing to having easier access to a 3D printer than I do to a laser cutter. I started by using the schematic and extruding each of the pieces by 6mm to create the shape files. This worked pretty much perfectly except that everything ended up being a fraction of a millimeter larger than expected once printed and none of the snap fit pieces actually fit together. Ooops. Rather than print everything all over again, I ended up inexpertly sanding things down and using Sugru to prevent the pieces from wiggling. I've included the shape files for anyone who wants to try something similar, but I would recommend adding a little clearance to each of the pegs to save yourself some misery.

![Finished Fab Speaker]({{site.url}}/assets/20151010_onespeaker.jpg)

Overall I think they turned out OK.

*Shape files:
[1]({{site.url}}/assets/speaker_part1.stl)
[2]({{site.url}}/assets/speaker_part2.stl)
[3]({{site.url}}/assets/speaker_part3.stl)
[4]({{site.url}}/assets/speaker_part4.stl)
[5]({{site.url}}/assets/speaker_part5.stl)
[6]({{site.url}}/assets/speaker_part6.stl)
[7]({{site.url}}/assets/speaker_part7.stl)
[8]({{site.url}}/assets/speaker_part8.stl)
[9]({{site.url}}/assets/speaker_part9.stl)
[10]({{site.url}}/assets/speaker_part10.stl)*
