---
date: "2017-09-05T00:35:19+12:00"
categories: ["Arduino", "Wtf"]
title: "A happy ending story between two: Arduino and macOS"
author: "Sergio Moya"
images: ["img/twitter-card.jpg"]
aliases: ["/2017/a_happy_ending_story_between_two_arduino_and_macos/"]
---

If you are an Arduino developer using Mac you probably experienced, at some point, driver issues. 
I want to mention that FTDI USB chip driver are a kind of lottery, sometimes they work without having to install anything, others they just do not. [FTDI drivers](http://www.ftdichip.com/FTDrivers.htm) caused me problems in the past, not only with Arduino boards, forcing me to install, uninstall, clean, and install again several times until magically worked.

I got a new MacBook Pro almost one year ago. It came with macOS Sierra pre-installed (by the time of this post, Sierra is the latest macOS version). I decided to install one of my Arduino Nano boards in my new brand computer. After plug in it, two thoughts suddenly came up to me:

1. I remembered the pain I suffered the last time I updated my OS to Sierra. I spent hours reinstalling the FTDI Drivers.
2. I was close to face a new world of dangerous and unfamiliar [dongles](https://www.youtube.com/watch?v=-XSC_UG5_kU). My first "*Fuck, I knew it*" came out of my mouth at the speed of a [Eurofighter](https://www.raf.mod.uk/equipment/typhoon.cfm).

Yes, you are right. It did not work.

I reinstalled the **FTDI drivers** several times, restarted, and disabled the System Integrity Protection (**csrutil**) with no success. Therefore, I bought the original Apple USB converter thinking that my cheap dongle was not providing the needed output power. No success. My computer did not recognise Arduino. Not even as unknown device.

**I just gave up.**

Then, the idea of buying a PC just for having Arduino working came to my mind after some days, even I got close to buy one.

After leaving work last Monday, I wanted to give a last chance to my MacBook Arduino ecosystem. Trying to analyse the problem from scratch I just came up with one far, but possible issue culprit.

What if I told you that the problem was there since time ago? What if I told you that the culprit was just an stupid piece of brass called Micro USB? But How could I have been so stupid all this time?

The USB wire was just [broken](https://media1.giphy.com/media/hIfDZ869b7EHu/200w.gif). After replacing it, everything worked as expected. No extra driver installation needed.

What's the key message of all this story?

> Do not judge a piece of Software or Hardware by past issues. Instead, thing on what did you do to make it work as expected. - Me, today.
