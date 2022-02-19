---
layout: post
title:  "How to receive and decode KG-STV transmissions"
description:
date:   2022-02-19
categories: 
---

On February 15th, 2022, Oliver DG6BCE made an announcement through the ARISS blog that during a special event on February 20th the International Space Station's V/U FM repeater would be used for experimental KG-STV operation. As this is the first time such event is taking place, I thought it could be a good idea to publish this short write-up for the people who will be receiving KG-STV for their first time. 

## What is KG-STV
KG-STV is an image transmission mode that is used to send and receive images in a manner similar to SSTV. Unlike SSTV though, this mode is digital; it compresses the image and then uses FSK modulation to transmit it in smaller chunks, which are subsequently decoded and pieced back together by the receiver.

KG-STV is used by amateur radio operators on the same bands as SSTV, meaning that you can find KG-STV transmissions on various HF bands, or, and perhaps more notably for the SDR community, on the QO-100 geostationary satellite.

### Advantages

Being digital, KG-STV offers a more efficient use of the spectrum it takes up than SSTV. The software is also very adaptive when it comes to receiving the image packets; for example, a transmission can be paused and then resumed at a later time and the decoder will simply continue where it left off. A transmission can also be repeated multiple times, and in case the decoder missed some image chunks (for example due to poor signal) it will use the repeated transmission to fill in the blanks.

KG-STV is also very flexible - different quality, compression, or even modulation settings can be used by the transmitter without having to adjust the receiver as it will be able to automatically identify the properties of the incoming transmission. One can therefore fine tune the ratio between image quality and the time it takes to transmit based on how much time they have available for the transmission. KG-STV also offers error correction which improves its tolerance of poor signal conditions and/or low signal to noise ratios.

One of the most advanced features and probably the most useful for two-way amateur contacts is that receiving instances of KG-STV software are capable of determining exactly which parts of the image were not decoded correctly, and then asking the transmitter to re-send those specific chunks without having to repeat the entire transmission.

Besides image transmissions, KG-STV can also be used for short text messaging.

### Disadvantages

Some amateur radio operators have expressed their concern about the choice of KG-STV for the ISS due to the very limited software choice. As of now, there is only a single program that can be used to receive the transmissions, which is available only for Windows and is closed-source.

When the missed data requesting feature is taken away (which will always be the case for unlicensed SDR users that can only receive and not transmit), missed parts of the transmission can have overall worse effects on the image than with SSTV. While a few seconds of bad SSTV signal will result in a thin line of noise spanning the entire width of the image, a missed KG-STV packet will result in a black square wherever the decoder was just writing data. Impact of this might however be dependent on the context, such as what part of the image was missed and what kind of encoding was used (as well as the preference of either a missing block or missing lines being subjective).

In terms of RX-only, which is what I mostly focus on, I would classify KG-STV as an interesting *alternative* of SSTV, but definitely not its replacement. Without the data re-send feature, the advantages of KG-STV such as better spectrum efficiency and error correction are not that ground breaking when compared to a standard SSTV transmission.

I personally welcome the ISS KG-STV experiment and would like to eventually see it routinely conducted with the VHF transmitter (perhaps with some other digital image modes), but I wouldn't want it to replace the SSTV events for good.

## Receiving KG-STV

The process of receiving a KG-STV transmission is pretty much the same as SSTV. Here I will demonstrate how an FM transmission from the ISS would be received and decoded, but it would be the same in any other context with the only real difference being the audio modulation used by your SDR software (for example, QO-100 uses USB, some HF bands might use LSB).

The first step is tuning to the correct frequency with an appropriate bandwidth. The ISS downlink that will be used for the KG-STV experiment is 437.80 MHz, and the bandwidth of its FM repeater is around 10 kHz. The station's Doppler shift also has to be taken into account, this can be up to 20 kHz. Overall a 30 kHz FM bandwidth used by the SDR should suffice for the reception of the ISS over its entire pass. Alternatively, a narrower bandwidth around 10 kHz can be used together with manual or automatic Doppler compensation, but note that some SDR software may cause skips in the received audio when changing its frequency which can disrupt the decoding.

In case of HF or QO-100, a much narrower single-sideband bandwidth would be used.

For your first few KG-STV receptions I recommend first recording the audio (or baseband if you want to be extra safe) and decoding it at a later point, so that you don't lose your data if the decoder isn't configured correctly.

The image below shows examples of how a KG-STV transmission might look like. The left one is FM with downwards frequency shift (like you may see from the ISS) and the right one is single-sideband.

![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/kgstv/fft.png?raw=true)

## Decoding KG-STV

The Japanese homepage for the KG-STV decoder can be accessed [via this link](http://www2.plala.or.jp/hikokibiyori/soft/kgstv/). English speaking visitors can conveniently notice the "click here to download" text which is a direct download link of a zip archive containing the decoder.

As KG-STV can only work with real-time audio, a virtual audio cable driver is needed to pipe the audio from your SDR receiver (or audio player in case you record the audio beforehand) to the decoder. To set the virtual audio cable as your audio source for KG-STV, click the "Settings" button and find your audio source in the "Input device" selection box. Click "OK" to save your selection and your KG-STV instance should be ready to decode.

The screenshot below shows KG-STV configured to accept audio from (and send to) the virtual audio cable driver.

![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/kgstv/config.png?raw=true)

Once the correct audio source is selected, the decoder will begin waiting for the signal. When a transmission is detected you should be able to see the incoming signal in the center waterfall view and the image should start slowly appearing in the right window under the "RX" tab.

Below you can see an annotated image of the KG-STV GUI while decoding an incoming transmission.

![](https://github.com/sgcderek/sgcderek.github.io/blob/main/images/kgstv/gui.png?raw=true)

Note that the "Mode", "Code", and "Scale" settings are only relevant when transmitting. When receiving, these will be recognized automatically by the decoder.

When a transmission finishes or is disrupted for a long enough time, KG-STV will save the decoded image into the "AutoSave" folder, which it will create in the directory where the executable is located. You can also browse your received images by clicking the "LOG" tab and using the two arrow keys. Images can be deleted or copied to your clipboard inside the KG-STV GUI using the appropriate buttons.

When a text message is received, it will appear in the bottom left text box in a CALLSIGN>MESSAGE format.

If you are using single-sideband modulation it is important to fine tune your receiver so that the signal is at a correct frequency. The data stream should be centered between the two dashed lines in the KG-STV center waterfall window. This isn't necessary with FM as the demodulator already offsets the audio correctly for you (assuming the signal is strong enough and inside the tuned bandwidth).

## Final notes

The same precautions you might be familiar with from APT, LRPT, or SSTV apply when receiving KG-STV.

Always make sure your SDR gain is set properly. Too low gain will result in your signal being weak, too high gain might overload your SDR and degrade the signal quality.

If KG-STV isn't showing an incoming signal but you are clearly seeing it in your SDR software, make sure you have the correct audio output device set as well as that your volume isn't too low or muted. Too high volume can also cause issues for the decoder.

If you have any further questions or suggestion, feel free to contact me via [Twitter](https://twitter.com/dereksgc) or ask the [r/amateursatellites](https://www.reddit.com/r/amateursatellites) subreddit *(when it's satellite related)*.
