+++
title = "Raven Smartwatch - PCB"
date = "2025-04-25T09:22:47-06:00"

description = "Personal project - Raven"

tags = ["project", "hardware", "pcb", "raven"]

draft = true
+++

[GitHub profile](https://github.com/merrittlj)
[raven-hardware repository](https://github.com/merrittlj/raven-hardware)
[schematic pdf](https://github.com/merrittlj/raven-hardware/releases/download/v1.0.0/kicad_v100.pdf)
[parts spreadsheet](https://docs.google.com/spreadsheets/d/1oSL-olhkF5xc7F7o5_qvC-SHkViK23rOYAubs1avKWQ/edit?usp=sharing)

# PCB
This is the fourth post regarding my current project, the Raven smartwatch. In this post, I will be discussing the PCB design and other changes as I finish the project.

So far I have mostly tested the first revision of the final(small, not development board) PCB design. I've sinced fixed minor silkscreen and footprint issues, but all of these were able to be worked around during assembly of the first board. This revision utilized 0402 and smaller components, which while initially difficult, I was able to solder relatively easily as I developed my hot air and soldering iron skills.

## Design decisions
Rather than use an SWD connector, USB DFU is much more practical and extremely convienent. SWD not only requires a few pads to be exposed, but also makes it nearly impossible to easily update the firmware at any time without wires soldered on. USB-C is already used for power and at least for most STM32 microcontrollers, just connecting DM and DP between the port and the microcontroller adds DFU functionality.

I decided to implement a small power switch that turns on and off 3.3v, this is used to easily get the device into DFU and bootloader mode without needing an NRST switch. Holding the first watch button down as you turn on power holds BOOT0 high for entering DFU.

Although I could have opted for a microcontroller module which incorporates both the microcontroller and the antenna, this was unreasonable due to space(and refactor) costs, and the antenna was easily implemented with the MLPF-WB55-01E3, which is a small filter optimized for STM32 RF performance.

The PCB size is as the exact size of the display, and due to reducing hardware complexity via other methods, I was able to fit everything on in such a small space without the additional costs of extra small vias, etc. I opted to just avoid putting any components on the back side of the PCB so I would be able to lay the display flat and simply.

# Final touches
