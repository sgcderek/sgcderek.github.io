---
layout: post
title:  "V-Dipole Introduction"
description: 
date:   2021-03-18
categories: 
---

## How a satellite V-Dipole antenna works and how to build one

| Table of contents |
| - |
|[Introduction](#introduction)|
|[How a dipole radiates](#how-a-dipole-radiates)|
|[Satellite elevation](#satellite-elevation)|
|[V-Dipole (finally)](#v-dipole-finally)|
|[Building a V-Dipole](#building-a-v-dipole)|
|[Testing the antenna](#testing-the-antenna)|
|[Conclusion](#conclusion)|

### Introduction
When it comes to amateur satellite reception, you would most likely not be able to find a more used and a better documented combination than the 9A4QV V-Dipole and 137 MHz weather satellites.
[Adam describes his design on his own blog pretty well](https://lna4all.blogspot.com/2017/02/diy-137-mhz-wx-sat-v-dipole-antenna.html), and shows how simple it can be to build and mount, all while delivering very good results.

The reason I have decided to make my own writeup about this antenna is so that I can explore some aspects of the antenna in more detail, such as some construction and placement tips, as well as address common mistakes and misconceptions.

A dipole is the perfect starting point for not only satellite reception, but the entirety of amateur radio in general. It is easy to build and mount, it is very forgiving in design imperfections, it performs well and most importantly it is dirt cheap so even if it doesn't end up working as expected it won't be a big loss.

But for satellites especially, the dipole can be manipulated in a way that ends up providing a very well-suited radiation pattern.

### How a dipole radiates
If you look up "dipole antenna" online, you will most likely come across images of "I" or "T" shaped antennas. A *dipole* is composed of *two poles*, or "legs" as we are going to be calling them. Under normal circumstances these legs are spread out under a 180° angle. This makes sure that most of the energy is radiated in an imaginary plane perpendicular to the dipole.

*(Note that when talking about antennas, the way they radiate is generally identical to the way they receive, so when we talk about an antenna radiating energy in a certain direction, the same antenna will also have the best receiving performance in the same direction, hence why a "radiation pattern" of an antenna is also indicative of its receiving performance)*

Having your energy focused in a single plane around the antenna is desirable when you are using it for most omni-directional earthly tasks. Broadcast FM radio is a great example of this. You want your car radio to receive as many stations as it can, with signals coming in from all directions. Your car will also often be changing its own position relative to the radio towers, but unless something went horribly wrong you will generally stay on the same plane as those towers (that being the ground).

The wires sticking up from cars are usually monopole antennas (or "ground plane" antennas but for the sake of simplicity we'll just call them monopoles). The radiation pattern on these is pretty much identical to a vertically mounted dipole, in fact we could even imagine the car itself acting as the second "leg" of the dipole, although that is a big oversimplification. What's important to understand though is that all "stick" antennas like this, be it monopoles, dipoles, or collinear coax antennas (e.g. what's put inside WiFi router antennas) radiate (and therefore receive) best in a plane perpendicular to their "long" axis.

The image below shows the three mentioned antenna types, and highlights in red the actual radiating "antenna" part. Even though these are all different types of antennas with different principles of operation, when talking about their polarization and angles we can treat them all the same.

![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/vertical-antennas.png?raw=true)

On the following illustration you can see how differently positioned receivers using these antennas would be affected by having their antennas at apparent angles to the transmitter. To better understand what 2D plane we are working with, you can imagine the radios are laid on a table that you are looking at from above.

![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/monopole-polarization.png?raw=true)

In this example, optimal power transfer is achieved between the transmitter and receiver #1. This is because both antennas can "see" each other's full length.
Because of the position receiver #2 is at, an apparent angle is introduced, and thus the power transfer is less efficient. The exact same effect would be achieved if we put receiver #2 where #1 is and instead angled it. The apparent angle is highlighted as the orange line.
The antenna of receiver #3 is in line with the antenna of the transmitter, which would theoretically result in absolutely no power transfer due to the radiation pattern of this antenna. To stick with the previous analogy, the two antennas would "see" each other only as very small points, significantly lowering the area usable for signal transfer.
Keep in mind that you could substitute the monopole antennas from this example with 180° dipoles and the exact same effects would take place.

We laid the radios with their monopoles on the table horizontally. Therefore we can say that the antennas are all horizontally polarized. If we used the same example but rather stood up the radios vertically, now they would all be in the same plane and all of them would be able to receive the signals from the transmitter optimally.

Well now we've explained why radio antennas are vertical and why you don't need a big directional antenna for radio like you do for TV. But why is this important?

### Satellite elevation

We are interested in receiving satellites. Be that NOAA/METEOR weather satellites, the ISS, or any of the AMSAT fleet members. And, *surprisingly enough*, there is a big difference between a satellite and a terrestrial radio station. For one, a satellite can not stop. It is constantly in motion, orbiting planet Earth. The only exception to this rule would be satellites in geostationary orbit that you are already familiar with if you have satellite TV. These can get away with static, high-gain antennas, because the rate at which they move matches the rotation speed of the Earth almost perfectly.

But we won't be receiving satellites in GEO. Well, we can, and I hope to write about those at some point, but for the most part all the satellites of interest are instead in LEO, or low-Earth orbit. This results in the satellite only flying by your antenna for about 10-15 minutes, rising at one side of your horizon and setting on the other, just like the Sun and Moon do it except much faster.

The direction and position of the satellite relative to your antenna will also be changing with each pass. If it is a satellite in a polar orbit it's possible that it will rise directly South, fly perfectly overhead (a 90° elevation angle) and then set North. However, the same satellite during the next pass can only reach a low peak elevation, and only peek above the horizon in the West or East. This means that we now have to give a much better thought to the radiation pattern of our antenna than we could get away with for our car radio example.

But before we do that, let's see what happens when we use a vertically mounted dipole antenna to receive a satellite. Most satellites use circularly-polarized antennas themselves, that essentially act as both vertical and horizontal antennas with a 90° phase shift. This is very good for us because it means that we do not have to worry about the orientation of the satellite and only focus on our own antenna.

![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/vertical-dipole-sat.jpeg?raw=true)

In this example we are looking at the antenna from the side, where the bottom of the image is the ground and the top is space. As we've learned before, the signal transfer from the satellite to an antenna mounted like this will only be efficient if it is within the plane perpendicular to the dipole. But because the satellite is orbiting the planet, it will slowly rise up and fly over the antenna. Just like with our "radios on a table" example before, this will result in the signal getting worse and worse as the satellite gets above the dipole, in line with the legs.

But of course, the satellite will not always pass directly overhead. In fact, the satellites will be passing at lower elevations more often, meaning that a setup like this would most likely still be capable of delivering satisfactory results, and if you didn't care about signal consistency during high-elevation passes it could be perfectly acceptable.

To better understand the terms used to describe satellite passes, following is a composition of screenshots from the program "gpredict". These show what is known as a polar plot, which you will come across very often no matter what program you use to predict and track satellites.

![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/noaa-2-polar.jpeg?raw=true)

In this case the polar plots show three consecutive passes of a specific satellite above my location. The blue line shows the path the satellite is taking including timestamps of where the satellite will be at what time. Also please note where the West and East markers are in this example, as some programs invert those and thus display the polar plots mirrored.

Polar plots like this allow you to read the azimuth and elevation of any point along the satellite's track. I've highlighted a point on each in red that we are going to be using as an example. This is also the half-way point of each pass where the satellite is at its highest elevation, which is what's commonly used to evaluate the whole pass. The peak elevation directly correlates to how long the satellite will stay in line of sight, how strong the signal will be, and if the satellite will be able to clear any possible line-of-sight obstructions. In general, the higher the peak elevation is, the better the pass is (assuming a suitable antenna is used).

Each circle symbolizes a 30° step in elevation, starting at 0° with the outer one. Passes #1 and #3 have their peak elevation point slightly under the second mark, which means their peak elevation is slightly under 30° above the horizon. Pass #2 is pretty much directly overhead, meaning its peak elevation is close to 90°.

Reading the azimuth is done by drawing an imaginary line from the center of the cross through your point of interest and then looking at the angle between that line and the 0° point, which is the line connecting the center and the North. Passes #1 and #2 both have their azimuth during peak elevation around 45°, although with pass #2 it is harder to see. Pass #3 has its peak point right on the West line, meaning that its pretty much perfectly on 270°.

Knowing both the azimuth and elevation of the satellite is more important when tracking with a directional antenna, however we can return to our previous example to illustrate how a vertically mounted dipole would respond to passes like this. Passes #1 and #3 are relatively low so the vertical dipole could most likely receive them well. However pass #2 takes the satellite directly above the dipole, which would certainly result in a long signal blackout. So how can we fix this?

The most simple solution would be placing the dipole horizontally. Because our satellite of interest is on a polar orbit, it means that it will always be tracking either from the South to the North or the other way around. If we put our dipole horizontally with its legs pointing East and West, we essentially align its imaginary receiving plane with the high-elevation passes of our satellite.

In a horizontal East-West configuration like that, pass #2 would now become the most favorable one, with the satellite aligned with the dipole pretty much perfectly for the full duration of the pass. But by gaining this extra performance on pass #2, we have now sacrificed passes #1 and #3 since the "blind" spots of the dipole are now at these lower elevations. In case of pass #1 the satellite would be passing the blind spot around the 12:49 mark, and in case of pass #3 it would be the 16:39 peak mark. So how can we fix **this**?

### V-Dipole (finally)

One solution to this problem would be manually adjusting the angle of the dipole for each pass. For example, if your pass has a peak elevation of 45° at a 30° azimuth, you would rotate the dipole with one of the legs facing roughly North-North-East, as well as matching the peak elevation with the angle of the dipole. This however is impractical, and if you are already going to be doing this you might as well switch to a completely directional antenna and enjoy its full benefits.

We can instead use Adam 9A4QV's findings to modify our dipole into a V-Dipole configuration. This is done by bending the legs of the dipole in such a way that they form a 120° angle between them. That way we can reduce and even eliminate the blind spots of the dipole that are in line with the legs, allowing it to pick up satellites that are passing even on lower elevations and through a much wider azimuth range.

To reuse our previous analogy once again; by shaping the dipole into a "V" instead of an "I" there no longer is any angle under which the legs would line up, therefore no matter from which direction the signal arrives it will always encounter at least some usable receiving area of the antenna.

In the article linked at the beginning of this post, Adam includes a pdf he's written explaining the design process he used to land on the 120° angle specifically. He also describes his own way of constructing such an antenna, using very cheap and simple materials, as well as sharing the results he got from this antenna.

### Building a V-Dipole

Before you start building an antenna, even before you start getting the materials for it, you have to decide what frequency you will want to use it at. Most commonly a V-Dipole will be used for weather satellites centered at 137.5 MHz. This is also around the lowest frequency you will encounter any satellites on. A V-Dipole however is a perfectly valid antenna even at higher frequencies, up to lower microwaves (depending on the signal strength of the received satellite). The only difference in the construction for a different frequency will be a different leg length, everything else stays the same.

To come up with the leg length, we can simply divide the number 147 with the desired frequency in MHz. This will give us the combined length of the dipole in metres. Further dividing this by 2 will leave us with the target length for each leg. So, in our case:

147 / 137.5 ≃ 1.07
1.07 / 2 = 0.535 → **53.5 cm**

This will be the leg measurement I will be following from now on, however if you got a different result for your desired frequency you can still follow along, just using a different leg length.

To keep the cost of the antenna as low as possible, I've only used materials that I was able to source cheap and quick from local hardware and carpentry stores, even in the midst of the current pandemic.

![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/dipole-build-1.jpg?raw=true)

If you do decide to follow this tutorial to build your antenna, you will need the following;
* 2 metal welding wires for the dipole legs (in my case iron cut from 100 to 53.5 cm)
* 3 pairs of terminal blocks
* 4 screws
* A plank or a piece of plastic to act as the main antenna body
* A wooden/plastic/metal rod to act as the mounting point for the antenna
* 100k resistor (optional)
* A piece of 50 or 75 ohm coax cable and a matching connector (make sure you own the appropriate adapters/cables to be able to connect the final assembly to your SDR)

I start by bending the ends of the iron welding wires roughly so that when put next to each other they form the 120° angle. I also file the ends down a little to make it easier for the screw of the terminal block to make good contact. I then mark the position of the first terminal block as well as the angle I want the dipole to have on the piece of plank I am using as the antenna body.

![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/dipole-build-2.jpg?raw=true)

Before mounting the first terminal block, I also file down one side of the plastic overhang. This way it becomes much easier to connect the dipole legs to it, and it also allows them to bend much closer to the block itself, staying more true to the 120° V shape. On the image below you can see the difference between the normal and the shortened length.

![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/dipole-build-3.jpg?raw=true)

This modification isn't necessary for the remaining two blocks as those are only used to hold the shape of the dipole. First I mount the center block and connect the legs of the dipole, after which I can slide the two remaining blocks on each leg. This is where the angle of the dipole starts to matter as screwing on the two other blocks will effectively fix the shape in place.

![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/dipole-build-4.jpg?raw=true)

As you can see the center block deformed a little since I didn't get the bend in the legs correct, however this doesn't really matter as long as the piece of plastic in the middle isn't cracked.

Once all three terminal blocks are secured, the entire assembly can be turned upside down and mounted on top of the support pole. In my case its just a wooden stick I got at the carpentry store. By mounting this assembly upside down we give it some very rudimentary rain/snow/bird protection, and we also decrease the chance of water collecting inside the terminals.

![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/dipole-build-5.jpg?raw=true)

Now it's time to prepare the cable that we will use to actually interface with the antenna. I just grabbed a coil of 75 ohm TV coax at the store and then cut a smaller piece out of it. You could use a long run of coax connected directly to the antenna, however I plan on using the antenna with a filtered pre-amplifier. These only work properly when put as close to the antenna as possible. But even when not using such a device, I find it more practical to only leave small portions of cable at the antenna and just using a separate extension cable. This way you can share it between multiple antennas as well, and it makes the antenna less awkward to handle and transport when not receiving.

![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/dipole-build-6.jpg?raw=true)

On one end of the cable I mounted a standard F connector. I happen to have a F to SMA adapter which I can use to connect the antenna to my amplifier as well as a 3 meter SMA-tipped extension. On the other of the cable I leave both the center conductor and shield exposed, and this is also when I connect the optional 100K resistor between the two.

I've learned this from the dipole kit shipped with the RTL-SDR V3, as it is a very simple way to provide basic protection against static electricity building up on the dipole. I have used a dipole without this successfully before, and I'm sure many others have as well, so I'm not entirely sure how big of a benefit this provides, but it costs pretty much nothing so I might as well include it.

![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/dipole-build-7.jpg?raw=true)

It doesn't matter to which side you connect the center conductor and the shield, however if you were ever building a vertical dipole antenna it is common practice to have the center conductor connected to the leg facing up.

Once the coaxial cable is connected, the antenna is pretty much finished and ready for its first test. Of course there are still many improvements to be done, for example I highly recommend using zipties or tape to fix the cable to the mounting rod so that no tension is applied onto the connection in the terminal.

Another thing I can recommend doing would be encasing at least the central terminal block in epoxy or even just hot glue. This in combination with a more suitable leg material such as brass or aluminium is usually enough to make the antenna very weather-proof. Make sure the antenna works well before doing this though, as you won't be able to simply disassemble it afterwards. Wood also isn't a very ideal material so a good and cheap upgrade would be simply using plastic in its place.

![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/dipole-build-9b.jpg?raw=true)

### Testing the antenna
Because the frequency I chose for this V-Dipole was 137.5 MHz, I temporarily mounted it above one of my dishes and waited for a 50° pass of the NOAA-19 weather satellite. Because these satellites are on the already mentioned polar orbits, to achieve an optimal radiation pattern I aimed the antenna in such a way that its legs were pointing West and East. If you were using this antenna to receive a satellite in a less inclined orbit, such as the International Space Station, you would rotate it 90° as to make the legs face South and North instead.

The direction the "closed" and "open" ends doesn't really matter, so if you have your dipole in a configuration for polar orbit satellites with the legs facing towards the West and the East, you can have the "open" end of the V-Dipole facing either North or South.

As NOAA-19 started to rise from beyond the horizon, I started to see traces of the APT signal coming in surprisingly soon, pretty much on par with my Winkler QFH antenna that you can see in the background of the picture above. However, as is expected from a poorly mounted V-Dipole like this, the good performance sadly wasn't constant, and the resulting image had two very big chunks of it completely overwhelmed by noise.

![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/dipole-build-10.jpg?raw=true)
*(Note that the sharp contrast change visible on the right channel was a result of the satellite itself switching to a longer-wavelength infrared sensor, a perfectly normal behavior of APT)*

Still, the result wasn't *that* bad, and it definitely is miles better than what my first ever antennas were able to deliver, so I am going to call this a successful design. Besides, I am confident that these noise issues can be completely ironed out by simply putting the antenna into a better spot (there was a big metal dish below this one), or possibly by attaching an additional V-shaped reflector as I have already done in the past, but I will leave this for a future blog post instead.

### Conclusion

It is very simple to build a starter antenna like this and make it work well enough. You will most likely come across people who will tell you that something more advanced like a *turnstile*, *DCA*, or a *QFH* is required to get good results, and while we've seen here that that's not the case, there is some truth to that point.

Most importantly, as evidenced above, the radiation pattern of the V-Dipole is very hard to get consistent. In a perfect simulated world there should be absolutely no dips in the gain of the antenna as the satellite's elevation changes, however, once real-world aspects such as the ground are introduced, the pattern can get completely misshapen. As I said I will attempt to fix this using some simple and proven methods, that (if successful) will get their own dedicated blog post.

But there is another aspect of the V-Dipole that inherently makes it less suited for this task than other antennas, that being its polarization. Not all satellites use circular polarization, but pretty much all of the interesting ones do. And because the V-Dipole is linearly polarized instead, it will essentially only use 50% of the signal.

This sounds way worse than it actually is though, as a 50% decrease in signal strength amounts to "only" about 3dB, which may not even play a role since APT and LRPT signals are usually already a few dozen dB above the noise floor. But, if you wanted to be a perfectionist and get the best possible results, you would most likely want to switch to something like the QFH.

With all that said though, I highly recommend starting with the V-Dipole. Try it out for yourself, experiment with different placements, and see if the effort required to build a more advanced antenna is even worth it in your case. You will most likely come to find that the antenna placement plays almost a bigger role in its performance than the actual antenna type.

I hope you enjoyed reading this post, I know that I've probably went into much more detail than was necessary, but I believe that it is very good to understand the basic principle of operation of the V-Dipole before building your very own.

If you have any comments, questions, or suggestions, you can check out my [subreddit](https://www.reddit.com/r/amateursatellites/) and its [discord server](https://discord.gg/kuUGGJ5) focused primarily on the amateur satellite receiving hobby, or if it is something more personal or specific to this blog, you can reach out to me via [Twitter](https://twitter.com/dereksgc).

Thanks for reading and stay tuned for more updates!
