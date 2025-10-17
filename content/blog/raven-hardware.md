+++
title = "Raven Smartwatch - Hardware"
date = "2024-12-27T18:04:00-07:00"

description = "Personal project - Raven"

tags = ["project", "hardware", "raven"]
+++

- [GitHub profile](https://github.com/merrittlj)
- [raven-hardware repository](https://github.com/merrittlj/raven-hardware)
- [schematic pdf](https://github.com/merrittlj/raven-hardware/blob/master/kicad.pdf)
- [parts spreadsheet](https://docs.google.com/spreadsheets/d/1oSL-olhkF5xc7F7o5_qvC-SHkViK23rOYAubs1avKWQ/edit?usp=sharing)

# Hardware
This is the third post regarding my current project, the Raven smartwatch. In this post, I will be discussing the general hardware design and choices, as well as the lower-level aspects of the hardware.

# Initial choices
Overall, my main driving aspects of the hardware were to only use necessary components, removing unused or unnecessary components for a project like this(ex: some sort of speaker/audio output). While I initially attempted to focus greatly on power consumption, this is more of a side-goal rather than the main focus, as by nature with what this watch does and uses, it will be inherently low-power, so optimization should only come later when necessary(pre-optimization is the root of all evil!). Many choices came from my own personal preferences and ideas on what a smartwatch should have, so these are not all-encompassing.

## STM32WB55xx & BLE
I chose this microcontroller as I have existing experience within the STM ecosystem, it is commonly used within the embedded industry, and as the radios of these wireless chips have ridiculously low power consumption(only a few mAs for TX/RX). Similarily, I am utilizing BLE over other potentially better protocols due to its ubiquity throughout the wireless gadget space, and its pre-existing support in many applications.

## E-ink display
In addition to being an interesting technology that I wish was utilized more, an E-ink display provides very little power usage(by itself, and as it does not require a backlight), large viewing angles, and readability. As this watch mainly acts as a "mirror", displaying notifications or the time updating rarely, rather than functioning as an entire phone(see Apple Watchs), it can afford the low refresh-rate of the display. I am using a 1.54 inch 200x200 black and white display from Waveshare(though I am 99% sure it is a generic Chinese model), as this was a nice size and acceptable resolution, and potentially introduces compatability with Watchy's watch faces if I port them over.

## Physical buttons
I have always found touchscreens on watches particularily obnoxious due to the limited screen space and the difficult controls. The watch uses 4 physical buttons which provide convienent shortcuts and intuitive navigation. Despite my preferences for physical buttons, a touchscreen on an E-ink display is generally not a good idea anyways.

## USB-C & Battery
The watch uses USB-C due provide charging power and flashing through DFU. While a Micro-USB port may be easier to work with due to its simpler specification and widespread adoption, besides those fixable reasons it does not make sense to rely on old technology for products designed for the modern times(unfortunately many products do not follow this). Ignoring everything else, USB-C is just a cool standard overall with its design and capabilities. The obvious choice battery-wise is LiPO, I hope to fit at least ~150mAH(Watchy has a 200mAH battery), which I manage charging with the BQ25185 charger IC, and some other components to manage power-path support. I don't have a battery fuel gauge IC as relying on voltage is enough for my usecase and excluding it helps with reducing complexity.

## Storage
During my initial plans(and subsequent PCB development) I did not consider the effectiveness of GadgetBridge for managing settings, data, etc. While I did select nice and simple low-power EEPROM components for memory, given how I can immediately sync time, preferences, and other data on watch startup with the phone, there is no need to store any data, really. While I will abstract watch face implementation to make it easy to modify quickly, I personally do not need to dynamically download or store watch faces, so leaving this hardcoded within the flash's program code works fine for me. This may not be necessary, but any reduced complexity(especially in hardware) is greatly beneficial.

## Vibration
As mentioned previously, the watch will not include audio output, as I find that generally redundant and annoying compared to just using vibrations. I chose to use LRA motors, as I desired the very precise and crisp vibrations found in modern products(phones) compared to other options such as ERM motors, which despite being cheaper and smaller in size(though my selected LRA motor is very similar), can have higher power consumption. Anyways, apparently following a theme, I like using modern/higher-end technology.

# Hardware design process
I designed the first development board during my initial planning of the project. This first revision was meant to be a relatively easy soldering and debugging experience, with lots of test points and unoptimized design. As I have been working through the software, the board mostly holds up, though the storage section and some LEDs are unnecessary. I fully utilized KiCAD to design the schematic(see links at the top of the page) and layout the PCB. Though I am much more a software guy than hardware, fully creating my own hardware design from scratch gave valuable insights into the software, my vision for the watch, etc., and I imagine the experience would be useful for general embedded development(most embedded programmers come from EE).

The hardware development process required me to become much better at utilizing datasheets and EE concepts, which I am overall glad for choosing this route. I ordered the PCBs from JLCPCB, and am working through building them as I develop and nuance the software. Currently, in fact, I have a majority of the software developed and functional, so the main focus is on building, fixing, and shrinking the hardware. While initially skeptical of my ability to design and (more importantly) solder a smartwatch-sized PCB, as I develop my knowledge of the project, the hardware, my soldering skills, and remove complexity, I am confident that I am able to get the hardware wrapped up relatively quickly. Additionally, as most of the effort of figuring out designs for individual sections(USB-C, battery, vibration, etc.) has already been done with the first development board, there should be little fundamental changes hardware-wise.

# Conclusion
The next post will regard my general design of the smartwatch's PCB.

If you are interested in learning more about this project or other projects, look at my GitHub and the links provided at the top of the post.
