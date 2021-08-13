---
layout: post
title:  "Interfacing an LNB with an SDR"
description:
date:   2021-08-13
categories: 
---

Previous article: [**How an LNB works**](https://sgcderek.github.io/posts/how-an-lnb-works/)  
Next article: **Receiving the Ku-Band and Es'Hail-2 using an SDR** *(coming soon)*

## Introduction

In the previous post in this "series" I have explained what an LNB is and what it does in order to deliver high frequency microwave broadcasts to receivers over ordinary UHF/L-Band equipment. In this post I will present you an example of utilizing an LNB's functionality in the amateur radio world and actually using it with your SDR.

I will eventually describe how to receive voice, SSTV, and other digital and image amateur broadcasts from Es'Hail-2, as well as showing how other geostationary satellites' beacons can be received and demodulated. While the latter may not be as exciting it will at least allow you to get familiar with how to use an LNB even if you live outside of Es'Hail-2's range, which may come in handy in the future when/if other satellites with payloads like QO-100 are launched.

## LNB selection

Depending on where you live there may be different LNBs available to you. I will predominantly focus on "universal/Astra" Ku-Band LNBs that tend to have a single waveguide and a 9.75/10.6 GHz local oscillator frequency. There are different Ku-Band LNBs that behave the same, for example a "monoblock" LNB that has multiple waveguides and often many outputs can essentially be treated as a universal LNB by just utilizing one waveguide and one output.

While these LNBs are common here in Europe, in the US - for example - you may come across DirecTV or other Ku-Band LNBs that can use circular polarization and/or different (usually higher) local oscillator frequencies. It is possible that even the signalling and supply voltage may be different so always make sure you know these specifications about your LNB before proceeding. I am going to be describing how to use the aforementioned universal "Astra" LNB, which you can easily identify by a 9.75 GHz LO value (if your LNB label or documentation states that frequency, you most likely have a universal LNB).

These universal LNBs can nowadays be bought brand new for less than 10 Euro. If you do not plan on modifying your LNB and don't care about the internal circuitry, you will most likely be fine just buying the cheapest converter available to you. I own many old used LNBs at this point but have recently bought a brand new one for about 8€ including shipping, and it performs just as well and often better than any of the older ones I've used before. The mass adoption and manufacture of these LNBs gives us a very unique opportunity to experiment with something new without having to make a huge investment beforehand, which is very rare in the amateur radio world.

I will also only be focusing on the Ku-Band. While I would love to explore the C and Ka-Band satellites, I currently have no access to those LNBs. Hopefully I will be able to write about them in the future though!

For reference, the LNB I bought and that I will be using as an example in this and future posts is a *Zircon Single L-101 ECO*. While most satellite TV enthusiasts would probably advise against this, it has performed perfectly fine for my use so far.

![Zircon L-101 ECO LNB](https://raw.githubusercontent.com/sgcderek/sgcderek.github.io/main/images/4/zircon-l101.jpeg)

## Bias-T selection

Normally, an LNB is connected directly to the television receiver by coaxial cable. This works because the receiver contains all the necessary circuitry to produce the control voltage and signalling that the LNB needs to operate. An SDR, however, lacks this. While your radio may be equipped with a Bias-T for powering active elements such as LNAs, as was described in the previous post the LNB requires at least 12 and up to 18 volts to operate, with its current draw also not being very small. As far as I am aware, there is no SDR with Bias-T specifications sufficient to power an LNB, meaning that you will need to obtain an external one.

A Bias-T usually has three connection points; one for the DC supply, one for RF+DC, and one for RF only. When voltage is applied on the DC port it gets passed into the coax and continues through the RF+DC port where the LNB or other active device is connected. The receiver is connected to the RF only port, so it needs to be protected to make sure no DC leaks into it. A similar protection is in place on the DC port which blocks the RF from getting into the DC lines.

While this may seem complicated, a simple Bias-T circuit can be made using just two passive parts; a capacitor and an inductor. This is because these parts have the exact properties we're after (assuming that correct values are chosen) - the capacitor lets RF pass through while also providing no DC path, meanwhile the inductor lets only DC pass through it, blocking the RF.

![Basic Bias-T circuit](https://raw.githubusercontent.com/sgcderek/sgcderek.github.io/main/images/4/bias-t.png)

While this circuit looks simple to build on your own (because it is), there is a potentially even easier way of obtaining a good Bias-T that will also solve a lot of other issues that aren't really considered in the schematic above (such as structural integrity of the circuit or proper shielding). Terrestrial (free-to-air) TV antennas often also require power over the coax. In some cases this is done by the TV's or the receiver's internal Bias-T similarly to how LNBs are powered, however some hardware can not do that, which necessitates the introduction of antenna power supplies on the off-the-shelf market.

These units - also sometimes called power injectors or power junctions - are essentially nothing but a Bias-T circuit with an integrated voltage source, usually in the form of a power brick that plugs into a wall outlet. These can often be bought at very low prices from local electronic stores or ordered online, and even though they technically aren't rated up to the 2.2 GHz LNB IF frequency, in my own tests they have performed just fine.

When shopping for an antenna power injector, there are a few things to consider. The first and probably the most important one are the connectors used on it. While it is not too hard to modify DC circuitry to inject your own voltage, swapping a connector may get annoying. If you can get a power injector with F connectors you will have absolutely no issues connecting it to your LNB. Some are also made with TV IEC connectors that are more common in terrestrial TV reception. To connect those to an LNB you will either need an adapter or simply put an F connector on one end of your coax and an IEC connector on the other.

The other connector on the power injector also matters, as that is the one that you will eventually have to adapt to your SDR (usually to an SMA connector). I personally suggest getting a power injector with F connectors as those are generally better than IEC, even for normal TV applications that don't involve SDR or amateur radio. Even sourcing adapters for F connectors was much easier in my experience.

Another thing to consider when selecting a power injector is how it generates its supply voltage. As mentioned earlier, an LNB operates at 13 and 18 volts, while terrestrial TV antennas usually require 12 volts. To the Bias-T itself, the voltage doesn't matter, however some may come with integrated 12 volt wall power supplies. This will probably work just fine with your LNB, with the exact same effects as if you were running it on 13 V. You will however lose the ability to switch the LNB's polarization by altering the voltage, and - more importantly - you will not be able to go portable if you are using a laptop with your SDR and there isn't an outlet nearby.

If you do plan on using the integrated supply of the power injector, make sure to check that it is 12 V or close enough. The output voltage will usually be specified on the power brick that contains the AC-DC circuitry.

In my case, I have selected a Bias-T without an integrated supply that instead comes with two exposed power leads that can be connected to whatever voltage source I want. This Bias-T also came with F connectors and a neat shielded metal case, making it pretty much perfect for my application.

![Examples of antenna power injectos](https://raw.githubusercontent.com/sgcderek/sgcderek.github.io/main/images/4/injectors.jpeg)

The power injectors come in all different shapes and sizes but they all contain the same Bias-T circuit. On the image above you can see some examples of antenna power injectors, all of which could be used to power the LNB after varying amounts of modification effort. If you had a selection like the examples above, you would most likely want to get either the top-center or the top-right one, depending on whether you prefer the DC leads soldered to your supply directly or whether you'd rather use a barrel jack connector. I ended up getting the top-center one as that was readily available locally and simply soldered a barrel jack connector to the leads.

The three ones in the bottom row can all be used with a different supply by just cutting the power lead, while the one in the top-left has the AC-DC circuit integrated and thus would require opening up and connecting to the Bias-T directly if you wanted to use your own voltage source (which isn't very practical and also adds unnecessary cost and bulkiness for a feature that you won't use).

Also keep an eye out on the material the injector is made out of. If it is plastic it will most likely be poorly shielded and it might as well be easier and cheaper to just build the Bias-T circuit on your own at that point. A proper rigid and metal case with sturdy connectors (at least in my opinion) is worth paying for a little more than you would if you went the complete DIY route.

Depending on what injector you got, the port labeling may be a bit ambiguous or outright lacking. It is very important to know which port is RF and which is RF+DC, since if you happen to connect the injector backwards you may end up sending DC into your SDR and potentially destroying it. To distinguish the two, you can apply voltage to the DC input and then use a voltmeter to probe the two coax connectors. One should give you a clear DC voltage reading (that's your RF+DC port) while the other should read a voltage close to 0 (that's RF only).

## Powering the LNB

Once you have your power injector and a way to connect it to the your LNB (if your LNB has multiple connectors, it doesn't matter which one you choose), you will need a way of getting the correct voltage onto the DC port. If the power injector comes with a 12 V supply and you have means of powering it then you should be able to just use that.

If however, like me, you can only use your SDR outside with a laptop and you don't have access to the power grid, you will have to get a little more creative. In my case I did what seemed the most obvious and I simply use a USB port of the laptop to power the LNB. An LNB should require below about 2 watts to operate while a USB 2.0 port is usually rated for at least 2.5 W. This means that a cheap low-current DC-DC step-up converter can be used to turn the 5 volts coming from the USB port to 13 or 18 volts for LNB power.

If you don't like that margin and want to be safer about it, you can use a USB device with a higher current rating such as USB 3.0 or a power bank. Alternatively, something like a car or motorcycle battery can supply the correct voltage directly without the need for a step-up converter.

## Connecting to the SDR

For a basic setup all that's needed is the LNB and SDR (obviously), the Bias-T, and a 12-13 volt DC supply that can provide 2 W (about 170 mA at that voltage). For Es'Hail-2 and other satellite reception (which is what the next post is going to focus on) you do not need polarization or oscillator switching, so you don't have to worry about the 18 V supply or the 22 kHz control signal at all.

The physical form of such a setup will depend entirely on what hardware you have locally available so there isn't much I can describe besides showing my example later, however one important guideline that you absolutely have to follow is that under no circumstances can any direct current leak from your supply into the SDR antenna port. The Bias-T circuit takes care of this by placing a capacitor between the supply and the SDR, however that only works when connected properly, so ALWAYS make sure your SDR is connected to the RF only and not the RF+DC port of the Bias-T before powering it up.

The same also applies to alternating current, which isn't filtered by the capacitor. While there is no reasonable scenario in which AC should get into your Bias-T (besides the radio signal itself of course), if you are using a power injector with an integrated AC-DC converter then a fault may cause this. In conclusion, if you can you should always measure the DC and AC voltage on the RF port of your Bias-T while it's powered before connecting it to your receiver, unless you have absolute confidence in your setup.

Before connecting to the SDR you may also want to consider using an attenuator. LNBs are often used in cases where they have to push the IF signal through a lot of coaxial cable often terminated with cheap an insensitive receivers, so the signal (and noise floor) gets amplified a lot. An attenuator essentially does to RF what a resistor would do to DC (it can in fact be built using just a few resistors) and brings the signal back down to a more reasonable level for the SDR. I personally haven't used an attenuator with the RTL-SDR V3, the Nooelec NESDR SMArt V4, the Airspy Mini, nor the HackRF and I didn't have any issues, but it is a measure that can be taken to be more safe. If getting or building an attenuator isn't something you want or can do, you can simply use a few extra meters of coaxial cable.

An example image of a cheap 20dB attenuator can be found below. These can be bought with male and female F connectors already in place so they can just be inserted between the Bias-T RF port and the F-SMA adapter before the SDR (the attenuator should be in the RF only path, you shouldn't pass your supply direct current through it).

![20dB F connector attenuator](https://raw.githubusercontent.com/sgcderek/sgcderek.github.io/main/images/4/attenuator.png)

Speaking of which, you will need a way to finally connect the setup to your SDR's antenna port. Most SDRs have a female SMA port, unless you are using a USB TV tuner stick which may come with a different one (in some cases easier to adapt). In any case, you will need an adapter. That may arguably be one of the most difficult parts to obtain in the setup, however it is neccessary for a proper connection. A female F to a male SMA adapter can come very handy in many other situations as well, so I consider it a useful investment.

![Male SMA to female F connector adapter](https://raw.githubusercontent.com/sgcderek/sgcderek.github.io/main/images/4/f-sma-adapter.jpeg)

In many cases, cheap 75 ohm TV coax may be much more useful than expensive 50 ohm SMA-tipped coax. F connectors are cheap and very easy to connect and replace, and in combination with thick solid-core TV coax they are also significantly more durable. While not really rated for higher frequencies, when a strong enough amplifier is used (such as the one in an LNB or in Sawbird the filters from Nooelec), most if not all of the drawbacks of this cheap hardware are nullified (including the 75/50 ohm mismatch loss). I use 75 ohm TV coax with F connectors for satellite recepetion all the way between 137 up to 2400 MHz, mainly because sourcing good 50 ohm coax and SMA connectors is very hard for me (and expensive).

As long as the amplification is high and the coax runs reasonably short, you may find little to no difference between cheap 75 ohm widely available TV hardware and properly rated 50 ohm hardware.

With all that said though, keep in mind this is only really applicable when you're on the receiving end of a radio link. I probably would be much less comfortable with using cheap hardware that's not rated for my application if I wanted to transmit several Watts of RF power through it, but that is hardly relevant to this topic.

## Final setup

Below you can see a block diagram of what the setup to interface an SDR with a universal Ku-Band LNB can look like. It also represent the setup that I have successfully used in the past.

![Block diagram of a basic SDR/LNB interface setup](https://raw.githubusercontent.com/sgcderek/sgcderek.github.io/main/images/4/setup.jpeg)

One thing that I haven't yet mentioned, which you probably realized, is the satellite dish antenna. That is because, unlike LNBs and most other hardware, pretty much any dish that can be used for television reception can be used in this case as well. Unless it is physically bent or otherwise damaged, there isn't really anything that can go wrong with it, so you can often get used ones pretty much for free from people who have moved from satellite television to internet or cable.

There is one caveat though, and that is that in order for an LNB to fully utilize its dish, its illumination (beam) angle must match the focal length to diameter (f/D) ratio of the dish. This again sounds more complicated than it really is. For example, a C-Band LNB that's meant to be used with a large prime-focus dish will have a wider beam angle than a unversal Ku-Band LNB from a much smaller offset dish. I doubt that this will even be a factor for you though, as in case of Ku-Band reception only offset dishes are really used, and they all have very similar f/D ratios.

The diameter of the dish dictates the signal-to-noise ratio that you can expect. A bigger dish will receive stronger signals but will also be more challenging to aim as its own beam will become narrower. In case of Ku-Band satellite television, about 80 cm is the usual diameter used for Astra and similar satellites, and it is also about the diameter that you should aim for as it gives good performance while still being small enough to be handled easily. If you do however have access to a bigger dish and you like the challenge of getting it working and the potential of a stronger signal, then by all means go for it. That is the basis of satellite TV [DXing](https://en.wikipedia.org/wiki/TV_and_FM_DX) which is a whole another hobby that can be explored.

## Powering up

You can do a basic check of your LNB by powering it up without being aimed at a satellite (or even attached to a dish) by simply powering it up while connected to your SDR. Make sure to decrease your gain settings before doing that though as you do not want to over-saturate your receiver. If the LNB works, then you should see a notable noise floor level increase in the IF range of the LNB, so about 950 - 2200 MHz. You should then see furhter noise level increase when the LNB is pointed at the Sun (once again even without a dish) as that is a powerful source of Ku-Band radiation (sometimes even resulting in satellite TV [Sun outage](https://en.wikipedia.org/wiki/Sun_outage)).

![LNB noise floor increase example](https://raw.githubusercontent.com/sgcderek/sgcderek.github.io/main/images/4/LNB-noise-floor.jpeg)

In the next post I will finally describe how to use the setup to receive your first Ku-Band satellite signals and the promised Es'Hail-2 QO-100 amateur radio transponder.

---
Previous article: [**How an LNB works**](https://sgcderek.github.io/posts/how-an-lnb-works/)  
Next article: **Receiving the Ku-Band and Es'Hail-2 using an SDR** *(coming soon)*

