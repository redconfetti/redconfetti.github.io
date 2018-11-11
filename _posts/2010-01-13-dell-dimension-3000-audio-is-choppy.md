---
layout: post
status: publish
published: true
title: Dell Dimension 3000 - Audio is Choppy
author: maxwell keyes
date: '2010-01-13 18:49:45 -0800'
date_gmt: '2010-01-13 22:49:45 -0800'
categories:
- computing tips
---

At work I have a Dell Dimension 3000 workstation, and for months I've put up
with the computer being kind of slow. I just figured it was due to the
computer being kind of old...but I brought in some headphones so I could do
some serious music listening while I work and just got fed up with the way the
sound was choppy. Anytime I'd do anything processor intensive the music I was
listening to in [Grooveshark][Grooveshark] (which I highly recommend you check
out) would sound like crap.

So I installed the latest sound driver from Dell.com for the integrated sound
card and still after rebooting it sucked. Also when I start the computer up in
the morning, I would have to wait like 3 minutes at least for all the startup
programs to load. Nothing else would open until this was done.

So anyway, I searched for a solution to this issue and found this post:

[choppy sound on DELL Dimension 3000]

It turns out that the Primary IDE controller in Windows XP was set to use some
sort of PIO mode to communicate with the hard drive on the computer, as
opposed to DMA mode. DMA stands for Direct Memory Access, and is totally more
efficient than the PIO mode, Programmed Input-Output, where the central
processor transfers data byte for byte or word for word through your system to
the other components (like your sound card).

My computer runs blazing fast now in comparison to how it was running before.
I'm so glad I checked into this. If you have a Dell Dimension 3000 and it's
running like crap, definitely try this.

[Grooveshark]: http://www.grooveshark.com/
[choppy sound on DELL Dimension 3000]: http://www.softwaretipsandtricks.com/forum/hardware-problems/29157-choppy-sound-dell-dimension-3000-a.html
