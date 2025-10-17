+++
title = "Raven Smartwatch - PCB"
date = "2025-10-10T09:18:25-06:00"

description = "Personal project - Raven"

tags = ["project", "hardware", "pcb", "raven"]

draft = false
+++

- [GitHub profile](https://github.com/merrittlj)
- [raven-hardware repository](https://github.com/merrittlj/raven-hardware)
- [schematic pdf](https://github.com/merrittlj/raven-hardware/blob/master/kicad.pdf)
- [parts spreadsheet](https://docs.google.com/spreadsheets/d/1oSL-olhkF5xc7F7o5_qvC-SHkViK23rOYAubs1avKWQ/edit?usp=sharing)

## Intro
This is the fourth post regarding my current project, the Raven smartwatch. In this post, I will be discussing the PCB design and other changes as I finish the project.

The first 1.0.0 version of the PCB was simply a large debuggable test PCB, with lots of test points and modularized sections(eg. battery, display, haptics, etc.). The most recent revision utilized 0402 sized smaller components, which while initially difficult, I was able to solder relatively easily as I developed my hot air and soldering iron skills, and actually shrinks the PCB down to the size of the screen while removing all the debug components. I think that this method of first producing a large debugging PCB, then moving onto smaller sizes is the best approach towards most DIY hardware projects of this complexity, as they would quickly become unfeasible or impossible on breadboards, and immediately jumping into small-sized PCBs would be a nightmare to easily debug.

## Design decisions
Rather than use an SWD connector, USB DFU is much more practical and extremely convienent. SWD not only requires a few pads to be exposed, but also makes it nearly impossible to easily update the firmware at any time without wires soldered on. USB-C is already used for power and at least for most STM32 microcontrollers, just connecting DM and DP between the USB-C port and the microcontroller adds DFU functionality.

I decided to implement a small power switch that turns on and off 3.3v, this is used to easily get the device into DFU and bootloader mode without needing an NRST switch. Holding the first watch button down as you turn on power holds BOOT0 high, entering DFU for easy flashing.

Although I could have opted for a microcontroller module which incorporates both the microcontroller and the antenna, this was unreasonable due to space(and refactor) costs, and the antenna was easily implemented with the MLPF-WB55-01E3, which is a small filter optimized for STM32 RF performance. Using a PCB antenna was both space-saving and suprisingly simple with little issues in execution or performance.

The PCB size is as the exact size of the display, and due to reducing hardware complexity via other methods, I was able to fit everything on in such a small space without the additional costs of extra small vias, etc. I opted to just avoid putting any components on the back side of the PCB so I would be able to lay the display flat and simply.

## Assembly
Parts were ordered from Digikey, with the PCB from JLCPCB. The main difficulty of the project's hardware was not within the PCB and hardware design itself, but rather managing parts and assembling boards. Even for such a simple design with maybe ~30 total distinct components, correctly sourcing everything, organizing, etc. is very tedious at some points. On the assembly side of things, shorts between GND and power would happen relatively frequently, and other assembly mistakes made it difficult to verify if the microcontroller hardware design itself was flawed or if something was assembled poorly. However, over time and practice assembling many many boards and components, 0402 components are easy to solder(given the right tools, eg. a digital microscope), with the main difficulty being in the decent amount of hours needed to assemble a board. The FPC connector was the most bothersome to solder, but eventually I was able to succeed with that and produce a successful board.

I eventually got pretty good at using hot air to solder the microcontroller package, although difficult at first, a lot of flux certainly helped. The main reason I had to solder it so many times was because when I first started setting up the firmware on the shrunken boards, I bricked 4/5 microcontrollers by updating to the newest FUS(firmware upgrade service) version on them, then realizing that ST(or likely I) screwed something up with this new firmware version, bricking the wireless functionality on new updated versions.

I found during assembly that, suprisingly, I did not have any fundamental hardware flaws. Some progress was slow due to shorts or badly soldered microcontrollers, but overall, the assembly was smooth.

## Final thoughts
Currently, I have a v2.0.2(shrunked down, revised version) PCB fully assembled with all components fully operational. The project is nearly complete, with only the case design left. The case is currently a work-in-progress in SolidPython, and will be discussed in further detail in the next project post.
