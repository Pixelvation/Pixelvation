Pixelvation enables people to create high-quality LED art using open source tools

This GitHub repository is the current home of Pixelvation and will be updated whenever there's some significant new development.  To get notified on significant new developments, set up an alert on new releases here, or follow [@PixelvationRGB on Twitter](https://twitter.com/PixelvationRgb).

I'll set up a forum or Discord server at some point, but for now, feel free to open a GitHub Issue, or post to [SmartMatrix Community](https://community.pixelmatix.com) if you have any questions or want to discuss anything.

## Open Source Early

I've released several open source products, where the open source files were published after the product was designed, manufactured, and at a distributor ready to purchase.  With my [latest crowdfunded product](https://www.crowdsupply.com/pixelmatix/smartled-shield-for-teensy-4) I realized how much time I had spent on marketing and logistics while prepping for and running the campaign - work I really dislike - relative to the creative and technical work that I really enjoy.  I also really enjoy sharing my projects and collaborating with other people, but releasing the product through a crowdfunding campaign delayed the start of fully sharing my project details and collaborating with other people by months.

With Pixelvation, I'm trying a new approach to sharing my projects: releasing projects as open source early on - well before I even have plans to release a project as a product - and making it easy for people to source the hardware themselves.  I'm designing my projects around components that are available from [JLCPCB's SMT Library](https://jlcpcb.com/parts) whenever possible, so if you want to try one of the hardware designs you just place an order with JLCPCB for 5+ pieces of assembled PCBs, and they'll be manufactured and shipped for a very reasonable price in under a week.

I'm releasing work-in-progress designs under a CC BY-NC-SA license.  Usually I don't release my designs with a "-NC" Non-Commercial license, and after the design is further along and ready for sale I'll change the license to CC BY-SA.  I'm conflicted about this and may change my mind later as designs released "-NC" aren't truly open source, but for now I'm being conservative and keeping the "-NC".

## Pixelvation Engine

The first tool in development now is the Pixelvation Engine, a small board dedicated to receiving a single stream of LED data via serial, and driving a lot of LEDs with that data.  The Engine will support driving HUB75 panels, or addressable LEDs.

![Pixelvation Engine Overhead View](https://raw.githubusercontent.com/wiki/Pixelvation/Pixelvation/photos/PixelvationEngineV01Top.jpg)

As input, Pixelvation Engine accepts APA102 data at up to 5MHz, with 1-bit serial (like an APA102 LED), or up to 8-bit parallel.  The 1-bit serial input and CLK are buffered to accept up to 5V signals, but the remaining 7-bits parallel data must be driven at 3.3V only.

Pixelvation Engine has 16 outputs buffered to 5V levels, and an extra 3.3V output.  The outputs are arranged around the pinout of a HUB75 panel, so the board can be plugged into a HUB75 panel directly.  To instead drive up to 16 channels of addressable LEDs, Pixelvation Engine needs to be plugged into a simple breakout board.  Generic breakout boards will be designed and released in the future, but you can also design your own project-specific breakout board with space for the Pixelvation Engine, and the form factor and LED connectors your project actually needs.

Pixelvation Engine is based around the ESP32, which has enough RAM, CPU power, and DMA functionality to deal with a lot of LED data, and some unique I2S peripherals that allow for receiving a fast stream of serial or parallel LED data as input, and setting up another stream of parallel LED data as output.

To program Pixelvation Engine you need a USB-Serial adapter.  This functionality was purposely left off the board to save space and cost, and ideally these features are only needed when setting up a project.  The programming pins on the input side were designed around the ESP-01 pinout, so cheap ESP-01 programmers can be used.  Specifically [this ESP-01 programmer](https://www.amazon.com/ESP-01S-Programmer-Adapter-Wireless-4-5-5-5V/dp/B07V556Q82) has the most functionality and is the easiest to use on its own, is widely available, and with a couple jumper wires added becomes even more functional.

![Pixelvation Engine Programmer](https://raw.githubusercontent.com/wiki/Pixelvation/Pixelvation/photos/PixelvationEngineProgrammer.jpg)

### Status

Currently Pixelvation Engine has been tested driving HUB75 panels up to 64x64 pixels, with APA102 input sources from Pixelblaze (V1-V3), WLED's soundreactive fork compiled with APA102 output and running on an ESP32 Dev board, and a Fadecandy-inspired USB to APA102 output board currently in development.

Pixelvation Engine uses SmartMatrix Library 4.0's ESP32 support for driving HUB75 panels, and is able to drive a 64x64 HUB75 panel with 36-bit color, at 240Hz.  It's possible to drive a 128x64 HUB75 panel with the same color settings, but it requires additional hardware outside the Pixelvation Engine (this is being prototyped now).

The addressable LED output functionality hasn't been tested yet.  

The I2S Input code has some room for improvement, as it's very inefficient with memory right now, and can't keep up with a 4MHz 1-bit serial input when driving a 64x64 pixel output, but this is fixable.

![Pixelvation Engine Demo](https://raw.githubusercontent.com/wiki/Pixelvation/Pixelvation/photos/PixelblazeV3PVEDemo2020-12.jpg)

[Click to watch demo video on Vimeo](https://vimeo.com/491391639/f8f3a9616e)

I ordered hardware version V0.1 assembled from JLCPCB, and out of eight boards that I functionally tested, seven of them were working, and one has an undiagnosed problem.  The solder pad under the ESP32 should be using a [smaller squares for solder paste](https://twitter.com/GregDavill/status/1326309124565467136) instead of the one large square it's currently using, as the large square can lead to problems.  I don't plan on making any major changes to the pinout or functionality of the board at this point, but I may move to a 4-layer board, and shrink the board width by 0.1"/0.2" if possible.

### More Details

The hardware is posted in [Hardware/Pixelvation Engine], HUB75 firmware is [in a separate repo](https://github.com/Pixelvation/PixelvationEngine_HUB75) and more details are in  [the Wiki](wiki).



## About Pixelvation

I (Louis Beaudoin) created the open source [SmartMatrix Library](https://github.com/pixelmatix/SmartMatrix) and [SmartMatrix/SmartLED Shields](http://docs.pixelmatix.com/SmartMatrix/shieldref.html) for Teensy in 2014 under the name Pixelmatix and have been improving those projects ever since.  I thought a new brandname was appropriate for this series of new projects with a focus on both addressable LEDs and HUB75 LED matrix panels, and a focus on creating innovative tools for LED art.
