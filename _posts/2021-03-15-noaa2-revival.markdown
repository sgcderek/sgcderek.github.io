---
layout: post
title:  "NOAA-2 returning from the dead"
description: "The old and decommissioned NOAA-2 satellite has been observed transmitting a signal on its original frequency."
date:   2021-03-15
categories: 
---
A few days ago, a long-time amateur radio satellite watcher Scott Tilley  [announced on Twitter](https://twitter.com/coastal8049/status/1370568925386215428)  that his automated L-Band receiver detected signal emissions from the old NOAA-2 satellite.

This satellite, also known as ITOS-D prior to its NOAA classification, was launched in 1972. It had operated until early 1975, after which it was decommissioned by NOAA and effectively considered "dead".

[![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/ITOS-TIROS-comp.png?raw=true)](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/ITOS-TIROS-comp.png?raw=true)  
*NOAA-2 (left) and NOAA-19 (right), showing the significant differences between the satellite bus designs*

It is not too uncommon for old decommissioned satellites to start transmitting a signal again, you may be familiar with a similar phenomenon that's happened to Transit 5B-5 or NOAA-9, and plenty other satellites. Happysat has a [page on his blog](http://happysat.nl/Deadsat/Gallery.html) documenting emissions like this, but note that it's now out of date and the satellites shown there may no longer be active.

[2021-03-19] Happysat got in touch and said that he may soon be revisiting his lists of historical satellites still broadcasting, so make sure to go check out his blog after this!

However what differentiates NOAA-2's newfound signal from NOAA-9's is the fact that the hardware responsible for encoding and modulating it still seems to be operational. This is similar to the already mentioned Transit 5B-5, in fact you may remember a recent [blog post](https://xerbo.net/posts/investigating-transit-pt1/) from Xerbo about the analysis and subsequent decoding of the Transit signal, that has revealed unmistakable patterns in the binary data such as a frame counter.

After seeing Scott's tweet, I have decided to use my own setup to attempt recording the signal;

[![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/NOAA2-HRPT-waterfall.png?raw=true)](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/NOAA2-HRPT-waterfall.png?raw=true)

The signal is being broadcast from NOAA-2 at 1697.5 MHz, so far it seems like it is active for about 30 seconds which is then followed by about a minute long period of silence. I received the signal shown above during daylight, and it is safe to assume that it would not be broadcast at night when the satellite's solar panels aren't receiving power, however this still hasn't been confirmed.

Modern satellites still use the same frequency band, in fact NOAA-19 can be found transmitting its own digital HRPT signal at 1698 MHz.

However, NOAA-2 predates digital HRPT as that was first used on NOAA-5 during the bus transition from ITOS to TIROS (a similar bus to that on which the three current NOAA/POES satellites are based).

NOAA satellites before HRPT used what essentially is high-rate APT; an FM audio transmission containing the selected instrument data in an analog format. Unlike APT though, this signal wouldn't have an interesting sound to it, as the actual subcarrier frequency is above the normal audible range. This is similar to how broadcast FM radio stations encode additional data in their signal, such as stereo audio and the digital RDS stream.

In contrast, the subcarrier frequency of APT is 2.4 kHz, which is responsible for its recognizable sound.

Just like with Transit 5B-5 before, Xerbo took a closer look at the signal that NOAA-2 started broadcasting, and after a while [it became obvious that the satellite wasn't as dead as it seemed](https://twitter.com/Xerbo10/status/1370865781949485056). After we looked through some of the old documentation, it was also revealed that the constant pattern that NOAA-2 is transmitting now partially matches the voltage calibration and sync [as described](https://twitter.com/ok9sgc/status/1370885888725684226).

After experimenting with the signal, Xerbo was able to determine how it is modulated, and extracted what would have been an Earth image if the satellite was still fully operational.

[![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/NOAA2-HRPT-image.png?raw=true)](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/NOAA2-HRPT-image.png?raw=true)

On the image above you can see a crop of the sync (2 vertical lines) and voltage calibration ramp. The empty grey/black parts are where the image would have been. This is similar to the calibration stripes that you may be familiar with from APT images. Basically it could be said that NOAA-2 is broadcasting "blank APT".

As mentioned though this isn't actual APT, although this satellite did have an APT payload. Following is a table containing the original frequencies this satellite was able to broadcast on. Note that so far only the L-Band link was observed active (and is the source of the "image" decoded above). There have been a few attempts to identify other emissions around 136 - 138 MHz but so far nothing has been found.

| Link | Frequency (MHz)   | Modern counterpart*
|--|--|--|
| Beacon | 136.77 | DSB |
| VHF Real-time | 137.50 | APT |
| VHF Real-time (backup) | 137.62 | APT |
| L-Band Real-time/playback   | 1697.50 | HRPT/GAC |  

**not the original names used for the links by NOAA*

It is not known when exactly NOAA-2 started transmitting this signal, however given the strength of the carriers included in the signal and its proximity in frequency to other satellites such as NOAA-19, METEOR-M N2/N2-2 or ARKTIKA-M N1, we can conclude that it must have been a recent revival as it would not take long to notice it.

It is also not known how long the satellite will stay alive. It may become a new longtime member of the historic fleet alongside the likes of Transit 5B-5, NOAA-9, or FengYun-1D, or it may shut off any second now remaining silent forever...

The baseband containing my signal can be downloaded [here](https://cloud.xerbo.net/s/PnSWL9TGmMFZJtF). Thanks again to Xerbo for taking his time to decode this and hosting the baseband recording on his server.
