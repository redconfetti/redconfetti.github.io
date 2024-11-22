---
layout: post
published: false
title: Amateur Radio
date: '2024-11-22 09:30:00 -0400'
comments: true
categories:
  - Miscellaneous
tags:
  - amateur-radio
---

I recently jumped into getting an amateur radio license, successfully passing
the test and getting my Technicians license. Something I didn't gain from all
the studying I did however was an understanding of what a person does with
an amateur radio license, and what kind of wisdom is needed to purchase the
right equipment.

## The Frequency Divide

This quick reference guide to the
[Amateur Radio Technician Privileges][ARRL Tech], provided by [ARRL.org][],
indicates the frequency bands and radio modes you can use as an Amateur Radio
Technician licensee. The majority of your privileges lie within the UHF and VHF
bands.

You can also view charts that map out all the bands and privileges extended
to the General and Amateur Extra licensees. See
[Graphical Frequency Allocations][]

[Graphical Frequency Allocations]: https://www.arrl.org/graphical-frequency-allocations

### Band Designations

The [radio spectrum][] of frequencies is divided into bands with conventional
names designated by the International Telecommunication Union (ITU).

Within these bands, there are sub-bands that are often referred to by the
wavelength of the radio waves, ranging in size from kilometers to millimeters.

The larger the wavelength of a radio wave, the more likely it is to propagate
long distances, diffract around large obstacles, or bounce off the ionosphere.
The designations lower than [Low Frequency (LF)][LF] are not included in any
amateur radio privileges, likely due to uses reserved by the FCC (government,
military) or due to the impracticality of cost for most citizens.

As you can see, a technicians has access to the high frequency bands in many
modes, but is restricted to CW on the 15 - 80 meter bands, with a narrow
range of Phone and Data on the 10 meter bands.

* Technician
  * [Ultra High Frequency (UHF)][UHF]
    * Phone, Image, Data, RTTY, CW
  * [Very High Frequency (VHF)][VHF]
    * Phone, Image, Data, RTTY, CW
  * [High Frequency (HF)][HF]
    * CW (3.525 - 3.6 MHz) - 80 meters
    * CW (7.025 - 7.125 MHz) - 40 Meters
    * CW (21.025 - 21.2 MHz) - 15 Meters
    * RTTY, Data (28 - 28.3 MHz) - 10 Meters
    * SSB Phone (28.3 - 28.5 MHz) - 10 Meters

The General license opens up access to a much wider range of Low, Medium,
and High Frequency ranges and modes. The Amateur Extra license only opens up
frequency ranges that are exclusive to Amateur Extra licensees.

[LF]: https://en.wikipedia.org/wiki/Low_frequency
[HF]: https://en.wikipedia.org/wiki/High_frequency
[VHF]: https://en.wikipedia.org/wiki/Very_high_frequency
[UHF]: https://en.wikipedia.org/wiki/Ultra_high_frequency
[radio spectrum]: https://en.wikipedia.org/wiki/Radio_frequency#Frequency_bands

### Modes

* CW stands for [carrier wave][], a mode used to communicate via
  [Morse Code][] using a pulse of carrier waves on a single frequency occupying
  only 150 Hertz of bandwidth (frequency space)
* AM stands for [amplitude modulation][AM], a mode that modulates the amplitude
  of a radio signal to encode an audio signal. AM was the first method developed
  for making audio radio transmissions.
* SSB stands for [Single Side Band][SSB]. This mode is similar to AM, however
  it only uses half the bandwidth of an AM transmission, thus allowing
  more space in the radio spectrum for other communications.
* FM stands for [frequency modulation][FM], a mode that modulates radio
  frequencies to represent an audio signal. Less susceptible to interference
  than AM, and able to obtain more clarity and fidelity by using a wider range.
  of frequencies (bandwidth).
* "Phone" is the term used for voice communications of any type including AM,
  SSB, or FM.
* [RTTY][] is the abbreviation for Radioteletype, a form of FM communication
  that utilizes [Frequency-shift keying][] to encode digital information. In
  this case that information is ASCII text.

[ARRL.org]: https://www.arrl.org/
[ARRL Tech]: https://www.arrl.org/files/file/Tech%20Band%20Chart/US%20Amateur%20Radio%20Technician%20Privileges.pdf
[carrier wave]: https://en.wikipedia.org/wiki/Carrier_wave
[morse code]: https://en.wikipedia.org/wiki/Morse_code
[RTTY]: https://en.wikipedia.org/wiki/Radioteletype
[AM]: https://en.wikipedia.org/wiki/AM_broadcasting
[SSB]: https://en.wikipedia.org/wiki/Single-sideband_modulation
[FM]: https://en.wikipedia.org/wiki/Frequency_modulation
[Frequency-shift keying]: https://en.wikipedia.org/wiki/Frequency-shift_keying

### Practical Applications

With this understanding in mind, you have to ask yourself what you want to
do with amateur radio.

#### City / Regional

If you want to establish contact with people in your city or region, you may
want to focus on a radio system that operates in the VHF/UHF bands
(2 meter / 70 cm). Radio transceivers are commonly manufactured to cover either
of these bands, or both ("dual band"), and some even cover three bands
("tri band") with support for the 1.25 meter (219 - 225 MHz) band.

VHF/UHF signals are much less affected by atmospheric noise, solar activity, and
ionospheric conditions. This results in clearer, more reliable communications,
especially in urban or suburban environments. UHF signals penetrate obstacles
like buildings and vegetation more effectively, thus making VHF better suited
for rural or outdoor settings.

VHF/UHF both rely on line-of-sight propagation, so large obstacles like hills
or mountains can get in the way. This is overcome through the use of
[repeaters][] that can extend the range of communications significantly, usually
extending coverage from a city in a valley out to the surrounding region.

You'll want to do some research into the repeaters in your area, and which types
of modes they offer, before you buy a UHF/VHF radio transceiver. If there aren't
many repeaters in your area, you might not want to invest in a VHF/UHF
transceiver.

You can lookup the various repeaters and the modes they support in your area
via [RepeaterBook.com][]. You can also check to see which radio frequencies
are used for various local services (fire, police, EMS, etc) on
[RadioReference.com][].

##### Digital Voice

Additionally, some repeaters may support digital voice or data packet
communications, often acting as a gateway through the internet to other amateur
radio users, usually addressed by their FCC issued call-signs. Digital voice
systems commonly support virtual "groups" or "rooms" where voice communication between
parties is isolated. This opens up the capability of a VHF/UHF radio to
communicating with people anywhere in the world.

* [D-Star][] - commonly supported by ICOM and Kenwood radios
* [Yaesu System Fusion][] - Abbreviated as "YSF", proprietary digital mode only
  available with Yaesu radios
  * [WIRES][] - Yaesu standard that complements YSF, enabling amateur radio
    operators to connect their radios and repeaters over the internet,
    facilitating worldwide communication
* [Digital Mobile Radio (DMR)][]
* [AllStar][]
* [EchoLink][]
* [Internet Radio Linking Project (IRLP)][]

All these standards involve a mix of open and proprietary standards.
The software, protocols, hardware, or network access, may involve open
standards, while at least one or more components is proprietary to a company
supporting the standard.

For instance, DMR uses the AMBE codec (Advanced Multi-Band Excitation) for voice
encoding and decoding, which is proprietary to Digital Voice Systems, Inc.
(DVSI). This codec must be licensed, meaning that manufacturers of DMR radios
must pay for the rights to use it, which limits the ability for open-source
implementations of DMR.

The only open standard is [FreeDV (Free Digital Voice)][freedv]. Currently,
there are no commercially manufactured radios that natively support FreeDV as a
built-in mode, as most radios are designed with proprietary digital modes.
However, FreeDV can still be used with many commercially available radios, but
it typically requires additional hardware and software for operation.

[FreeDV]: https://freedv.org/
[repeaters]: https://en.wikipedia.org/wiki/Radio_repeater
[RadioReference.com]: https://www.radioreference.com/
[RepeaterBook.com]: https://www.repeaterbook.com/
[D-Star]: https://en.wikipedia.org/wiki/D-STAR
[AllStar]: https://www.allstarlink.org/
[EchoLink]: https://en.wikipedia.org/wiki/EchoLink
[Internet Radio Linking Project (IRLP)]: https://en.wikipedia.org/wiki/Internet_Radio_Linking_Project
[WIRES]: https://en.wikipedia.org/wiki/Wide-coverage_Internet_Repeater_Enhancement_System
[Yaesu System Fusion]: https://en.wikipedia.org/wiki/Yaesu_(brand)#Digimode_%22Fusion%22
[Digital Mobile Radio (DMR)]: https://en.wikipedia.org/wiki/Digital_mobile_radio

#### States / Nations

If you want to engage in the hobby of making contacts with people in other parts
of your country or even other parts of the world, then you'll want to continue
through at least the General license.

Keep in mind that usually the contacts you make are random, and that making
contact with the same people can be difficult at times due to weather or solar
conditions.

HF signals depend on the ionosphere's state, which varies with the time of day,
season, solar activity, and geomagnetic disturbances (e.g., solar flares or storms).
Daytime favors higher HF frequencies (15–30 MHz), while nighttime is better for
lower frequencies (3–10 MHz).

Although it is possible to get a "shack in a box" unit that covers a wide number
of radio bands, there can be features or functionality missing when you try to
get everything in one radio.

### Digital Modes

I covered digital voice modes a bit in the City / Regional section above.
There are other digital modes that don't involve voice communication.

#### APRS

A common digital mode known as the
[Automatic Packet Reporting System (APRS)][APRS] supports the transmission of
GPS locations, user-to-user text messages, weather station telemetry,
bulletins, announcements, and even email.

APRS data can be displayed on a map, which can
show stations, objects, tracks of moving objects, weather stations, search and
rescue data, and direction finding data.

Packet repeaters, called digipeaters, form the backbone of the APRS system,
and use store and forward technology to retransmit packets. All stations operate
on the same radio channel, and packets move through the network from digipeater
to digipeater, propagating outward from their point of origin.

An APRS Internet Gateway, called an IGate, routes packets to the
[APRS Internet Service][].

As an example, the International Space Station uses APRS to broadcast it's
location, and can be tracked on [APRS.fi][] (call sign `NA1SS`).

[APRS]: https://en.wikipedia.org/wiki/Automatic_Packet_Reporting_System
[APRS.fi]: https://aprs.fi/#!z=11&call=a%2FNA1SS&timerange=3600&tail=3600
[APRS Internet Service]: https://www.aprs2.net/

## Equipment

As a new amateur radio hobbyist, it can be overwhelming to see all the various
types of equipment with various features, specifications, and roles, especially
when you don't understand what kind of application they are needed for.

After realizing that 

### Radio Transceiver


