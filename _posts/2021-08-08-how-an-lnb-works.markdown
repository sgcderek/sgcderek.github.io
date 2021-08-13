---
layout: post
title:  "How an LNB works"
description:
date:   2021-08-08
categories: 
---

Next article: [**Interfacing an LNB with an SDR**](https://sgcderek.github.io/posts/interfacing-an-lnb-with-an-sdr/)  

___

## Introduction

About two years ago I first started playing with the idea of receiving geostationary TV satellites using an SDR after reading an [RTL-SDR blog post](https://www.rtl-sdr.com/receiving-satellite-tv-beacons-rtl-sdr-lnb/) about it.  

In the series of the following posts I will focus on describing how a satellite converter works and how you can interface one with an SDR receiver, how to use this setup to receive and decode transmissions from the amateur radio transponder on the Es'Hail-2 satellite, and how to demodulate beacon broadcasts from other satellites as well.

As is mentioned in the RTL-SDR blog post, these particular signals are being broadcast from the satellites in the Ku-Band, specifically between 10.70 and 12.75 GHz in the case of my local coverage. As you're probably aware, the common SDRs can only go up to about 1.7 GHz, while even the consumer DVB-S tuners only operate with an intermediate frequency range of up to about 2.2 GHz.

## The purpose of an LNB

This is what the LNBs are for. A common LNB consists of three main parts; the actual antenna, a local oscillator, and a mixer. The LNB is usually mounted into the focal point of a dish reflector. The reflector collects the incoming signal and concentrates it onto the waveguide of the LNB, inside of which we can find a tiny antenna (in the case of the Ku-Band, the wavelength is about 3 cm, so a half-wave antenna element is only 1.5 cm long).

High microwave frequencies suffer from extreme attenuation when traveling through a conductor, which is why the signal received by the LNB's antenna element is usually amplified as soon as possible by an LNA directly next to the antenna's PCB connection point. Still, even after this amplification, we only want to deal with such a high frequency for as little time as possible, so the amplified signal is then passed into the mixer (usually through a bandpass filter).

The mixer has three connections. One for the input high-frequency signal (RF), one for the local oscillator (LO), and one for the output intermediate frequency (IF). When the RF and LO signals are mixed together, they create multiple products that are the results of subtraction and addition of the two. The one we are interested in and that's the most useful is the product of the LO signal subtracted from the RF one and vice-versa.

For example; a Ku-Band LNB receiving a 12 GHz RF signal with a 10.6 GHz LO will result in the signal converted down to 1.4 GHz. C-Band LNBs use the second case where the LO frequency is often higher than the RF input, so if one were to receive an RF signal at 3.9 GHz using an LO frequency of 5.15 GHz, the IF product would appear at 1.25 GHz.

*(Note that upconverters - that you may be familiar with if you've ever tried receiving shortwave signals - work the exact same way, except the IF product is the LO and RF signals added together. This RF+LO product is also present in LNBs but can largely be ignored due to its extremely high frequency that's well outside of the operational range of the LNB's components and that usually gets filtered out anyway.)*

Once converted down to the lower frequency, the signal is amplified (and filtered) again, but this time it can be sent over much longer and cheaper coaxial cable runs, which is how it gets from the LNB to the receiver, as well as processed by hardware similar to what terrestrial TV is using (similar LNAs, filters, ...).

## Example block diagrams

![Block diagram of a basic converter](https://raw.githubusercontent.com/sgcderek/sgcderek.github.io/main/images/3/lnb_simple.png)
The above image represents a very simplified block diagram of an LNB, highlighting only the three previously mentioned fundamental components. The antenna collects the RF and feeds it into the mixer where it is mixed with the LO signal, resulting in the desired intermediate frequency output.

But to bring in a specific example, following is a more detailed block diagram showing the components of a possible single-output Ku-Band LNB configuration.

![Example block diagram of a Ku-Band LNB](https://raw.githubusercontent.com/sgcderek/sgcderek.github.io/main/images/3/lnb_ku.png)
The three previous components still define main sections of the LNB, but as you can see they have been expanded upon to work in a real-world scenario.

Instead of a single antenna, it is common for Ku-Band LNBs to have two of them at a 90° angle, one for receiving horizontally and the other for vertically polarized signals. This way, a satellite can transmit two completely separate signals on the exact same frequency, and by switching the polarization of our receiver we can select which one we want to receive (the same principle applies to circularly polarized broadcasts, except they use a depolarizer to turn the antenna elements into right-hand and left-hand circularly polarized ones).

Right after each antenna is an RF amplifier which increases the signal level so that it can be distributed around the LNB's circuit without a loss in the signal-to-noise ratio. The amplified signal then goes through an RF switch, which as I've mentioned can be used to select the desired polarization. With Ku-Band LNBs, this is usually done by altering the supply voltage. There is a chip inside the LNB that recognizes the voltage level signalling and controls the RF switch accordingly.

The next and last component in the RF input chain is a bandpass filter. This is important because an unwanted signal at a completely different frequency than the one we're trying to receive could be mixed into the IF output. For example, a desired signal at 11.4 GHz will result in a 1650 MHz IF output when a 9.75 GHz LO is used (11400 - 9750 = 1650). However, in the same configuration, a strong enough unwanted signal at 8.1 GHz would result in the exact same IF frequency (9750 - 8100 = 1650). This could perhaps be a harmonic of strong cellular networking signals or a weather/traffic radar, which is why it is important to filter the RF before mixing.

The same applies for the LO signal, which sometimes has its own tuned bandpass filter simply to provide a cleaner output. Universal Ku-Band LNBs come with two local oscillators, one usually tuned to 9.75 and the other to 10.6 GHz. These can be switched between in a similar manner to the antenna polarization. This has an advantage of effectively widening your receiver's operational range and also making sure that the IF doesn't climb too high in frequency (and thus preventing high cable loss).

Modern PLL LNBs generate the LO signal in a chip using a crystal oscillator as a reference, followed by frequency multipliers. Older LNBs had dielectric resonator oscillators (DROs) on the PCB specifically tuned to the desired LO frequency. In both cases though the overall principle of operation is the same, with the PLL LNBs offering a more stable LO. While DRO LNBs are more modifiable, they are also much more susceptible to environmental conditions such as pressure and temperature. This normally isn't a big issue for very wideband DVB-S signals, but for our SDR purposes a new PLL LNB may be more desirable. Some ham radio operators even undergo the extra effort of modifying their LNB to accept an external, even more stable reference signal so that they are better suited for receiving very narrow signals, such as the ones being relayed by Es'Hail-2.

## LNB signalling

While the antenna RF switch is controlled by the input DC voltage, the LO switch is usually engaged by injecting a 22 kHz control signal into the LNB's output. By default the 9.75 GHz LO is selected, and when the control tone is received the LNB switches to the 10.6 GHz oscillator. The incoming 22 kHz control tone is low enough in frequency that the LNB can easily separate it from the outgoing IF signals, which is why it isn't really worth mentioning in the above block diagram.

Once the RF and LO signals have been mixed together, the resulting IF signal is further filtered and amplified before being delivered to the LNB's output which is usually in the form of an F connector. From there the signal can be handled by standard hardware without the need for specialized microwave parts.

As mentioned before, when a universal (Astra) Ku-Band LNB is powered it defaults to vertical polarization and the 9.75 GHz local oscillator frequency. The nominal supply voltage of this LNB is 13 V for vertical and 18 V for horizontal polarization, and to switch to the 10.6 GHz oscillator the 22 kHz control signal must be injected.

Luckily for us though, we won't really have to worry about active control of the LNB as its default configuration is good enough for some SDR experiments, including receiving voice and SSTV transmissions from Es'Hail-2.

___

Next article: [**Interfacing an LNB with an SDR**](https://sgcderek.github.io/posts/interfacing-an-lnb-with-an-sdr/)  

