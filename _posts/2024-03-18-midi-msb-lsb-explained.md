---
layout: post
published: true
title: MIDI MSB/LSB Explained
date: '2024-03-18 00:34:00 -0500'
categories:
  - music
tags:
  - midi
---

I was trying to understand MIDI better, so that I know the difference between
note messages, controller change messages (CC), and System Exclusive (SysEx).
Ultimately my goal is to better understand and work with MIDI and MIDI devices
in [Cubase][].

I ended up coming across references to Most Significant Byte (MSB) and Least
Significant Byte (LSB), which seems related to [Bit Numbering][]. However
I'm seeing MSB referred to as Most Significant Byte (not Bit), and LSB referred
to as Least Significant Byte (not Bit).

I tried to get an explanation to contextualize what this means in the context
of MIDI controller change messages, but didn't find much that was really clear,
other than this article -
[Changing patches over MIDI using Bank Select Controller][].

## Explained

Here is how I would explain it to someone.

### MIDI Limitations

When the MIDI specification was first developed, it wasn't foreseen that anyone
would need MIDI control change messages to have a value in a range greater than
0 - 127.

"Who would need more than 128 different patches/programs to choose from?"
"Who would need a resolution of more than 128 for the instrument's volume?"

By the way, MIDI refers to the different patches, or instruments, supported by
a device as "programs".

Because of this, MIDIs design does not allow you to send a value higher than 128
in a single message. Remember that 8 bits of binary can represent values
0 - 255, so MIDI limited values to 7 bits (0 - 127).

### Overcoming the Limitations

When sound modules came out with more than 128 programs, manufacturers
tried to overcome this limitation by organizing the programs into "banks". By
using a single Controller Change message to specify the bank, you could
have 128 banks multiplied by 128 programs each, for a total of
16384 programs you can switch to.

"Who would need more than 16384 programs?". At this point I think they didn't
want to limit systems again, so the MIDI specifications were updated to
accomodate any future needs.

### 14 Bits of Resolution

This is where the MSB/LSB scheme comes in.

For situations where Control Change messages might need to specify a value with
much higher resolution (more than 0 - 127), they decided to create pairs of
messages that each would send a value between 0 - 127. Each value is 7 bits,
for a combination of 14 bits, and thus a value range of 0 - 16383.

The first value, is called the Most Significant Byte (MSB). The second value,
which is also 7 bits long, is called the Least Significant Byte (LSB). This
terminology simply communicates that the first value is more significant than
the second value in determining the ultimate value derrived from both combined.

If you actually look at the Control Change messages that are supported for
selecting the Program Bank, Control Change message number 0 ('CC#0'), is the
"Bank Select MSB" value. Control Change message Number 32 (CC#32) is designated
as the "Bank Select LSB" value.

This means that you can specify up to 16384 banks, each including 128 programs,
for a total of 2,097,152 programs that can be specified by sending 3 messages:

* Bank Select MSB
* Bank Select LSB
* Program Change

Who could possibly need more than over 2 million program changes, right?

### Other Control Changes

This story makes the most sense in terms of Banks and Program changes, but it
also applies to other Control Changes. All the original control change messages
designated for Modulation Wheel, Breath Controller, Foot Controller, Volume,
Balance, Pan, etc. have equivalent "LSB" messages designated to increase the
resolution of their values if needed.

You can see them all defined in [MIDI 1.0 Control Change Messages (Data Bytes)][]

[Cubase]: https://www.steinberg.net/cubase/
[Changing patches over MIDI using Bank Select Controller]: http://midi.teragonaudio.com/tutr/bank.htm
[Bit Numbering]: https://en.wikipedia.org/wiki/Bit_numbering#Most_significant_bit
[MIDI 1.0 Control Change Messages (Data Bytes)]: https://midi.org/midi-1-0-control-change-messages
