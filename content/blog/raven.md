+++
title = "Raven Smartwatch"
date = "2025-12-18T12:54:31-07:00"

description = "Personal project - Raven"

tags = ["hardware","open source","project","software"]

draft = true
+++

- [GitHub profile](https://github.com/merrittlj)
- [Firmware repository](https://github.com/merrittlj/raven)
- [GadgetBridge(phone app) repository](https://github.com/merrittlj/raven-gb)
- [Hardware repository](https://github.com/merrittlj/raven-hardware)
- [Case repository](https://github.com/merrittlj/raven-hardware/blob/master/kicad.pdf)
- [Parts spreadsheet](https://docs.google.com/spreadsheets/d/1oSL-olhkF5xc7F7o5_qvC-SHkViK23rOYAubs1avKWQ/edit?usp=sharing)

- [Previous posts]

## Intro - The philosophy
Massive software platforms, extreme complexity, trainings, data collection, bloated code, as well as sheer mental overhead all are commonplace in modern technology and consumer products. While there is certainly many aspects in which these are necessary byproducts of ecosystems designed to support so many contemporary features, I believe that this has come to the point of significantly neglecting user experience for the sake of profits, laziness, ignorance, or whatever other factors may be present.

What if we took the time and effort to prioritize how someone interacts with and enjoys something? Would the technological landscape be equivalent to what it is today?

The modern world of consumer products especially reflects this. A very large majority of products focus strictly on profitability and ease of development to the point of creating identical, mediocre products. The lack of an ability to tailor products specifically to one's own needs through customization afforded by the product necessitates a completely different approach. For products that I will use daily for multiple years or decades, such issues compound greatly. A key motivator in tailoring my Linux setup to my needs is that a computer is such a foundational and essential tool to so much of my hobbies, work, and life, that restricting myself to what others have decided compared to what works for myself is not reasonable.

The ability to create personalized configurations, products, or projects is more accessible than ever, especially in the technology space. The FOSS community powers essentially all modern software, and given a problem, it's likely a FOSs implementation already exists. This allows for designing and creating the project at hand rather than worrying about implementation details. PCB manufacturing is very cheap in recent years, allowing anyone to design and order a batch of custom PCBs for $5, and have them delivered in under two weeks. PCB components and other parts can be found on many websites. 3D printing allows for custom cases, parts, or anything else that would previously need a manufacturing process for.

Although my own DIY projects are mostly motivated by personal enjoyment, or a curiousity to learn, this philosophy lies beneath. Such projects are liberating in the sense that I can place the control back within my own hands. Freedom to change any aspect and choose how I want to live my life instead of allowing it to be dictated by the impersonal decisions of others.

Such has inspired my keyboard project(), which tailors it to what I need and desire from such an everyday device.

## Intro - The actual project
A little over a year and a few months ago, I began planning a smartwatch project. My ultimate goal was to make a everyday minimalistic and functional smartwatch that satisfied a few requirements and issues I have with other options.
- Minimalistic does not mean lack of functionality, but rather lack of distractions
> E.g. no WiFi connection necessary, no login/account, no unnecessary UI, reduce hardware complexity as much as possible
- Connect to my phone and display relevant or useful things(act as a "mirror" of the phone)
> Music, navigation, notifications, alarms, timers
- D.R.Y. principle - do not duplicate what the phone already does
> No need to send messages, track steps, interact with services directly, set timers or alarms(the phone can do this!)
- Efficiency
> Navigate between screens easily with little interaction or effort, display what's useful, reduce complexity

More concretely, these principles solidified into the following specifications:
- 200x200 black and white E-ink screen
> This provides very little distraction compared to traditional screens, black and white + low resolution encourages intentional design, very low power usage, image permanance(flash once, it stays on the screen) which is especially useful for clocks, notifications, music, etc. Personally E-ink feels the most natural option, the closest option to traditional wristwatches that still allows for smart functionality.
- Phone connection through BLE and GadgetBridge
> Recent BLE technologies are very low-power, WiFi itself is unnecessary as it does not need to reach out to any external services or such. GadgetBridge is a phone companion app that is discussed more in the software post.
- Provide necessary phone services
> Notifications, navigation, music, notifications, calendar events, timers, alarms, phone calls are the main features implemented by the watch through the phone. The phone app can change various preferences on the watch(watch face, dark mode, hide certain alerts, etc.).
- 4 physical buttons
> Touchscreens are annoying to use, especially with such a small form factor. Buttons are easy to use by touch alone, and can provide the same functionality that a touchscreen would.
- STM32WB55xx microcontroller
> STM32 is an industry standard, their WB series is modern and built for this sort of application
- USB-C
> Micro-usb is ancient and should be promptly removed from this world. USB-C also allows for easy firmware flashing.
- No storage on the watch itself
> This greatly reduces complexity, watch settings can be stored on the phone itself through the app, there is no practical need for on-board storage.
- Long battery life
> The E-ink screen, microcontroller, and multiple software optimizations allow for battery life for around 1 to 2 weeks on a single change from a 200mAh battery.

I've written multiple blog posts documenting my progress while working on this project(linked at the top). I would highly recommend reading the design post() for more details on this specific part of the project.

## Software
- C++
> Generally the standard for the embedded systems industry, many libraries that I used(button debouncing, E-ink screen) also were written in either C or C++. However, I wrote the project mostly within C-style C++(rather than modern C++), and in hindsight I would prefer to use a different language given that it's reasonable for this sort of application(such as MicroPython), as C++ itself is a horrid mess.
- GadgetBridge
> My phone companion app is based off of the FOSS GadgetBridge(only on Android!), which provides an interface for smartwatches and other gadgets, this is mostly used to make a FOSS implementation for other existing smartwatches or accessories, although I chose to base my watch's protocol from scratch on this. GadgetBridge provides time updates, music updates, notification updates, etc., and my code in GadgetBridge forwards this through BLE to the watch which parses these details and displays it accordingly.
- FreeRTOS
> This is a common and widely used RTOS which provides tasks, semaphores, mutexes, timing, and similar functionality necessary for this scale of embedded project. In hindsight it may have been easier to use the Zephyr RTOS instead, considering that FreeRTOS is relatively old and has some strange quirks and syntax.
- LVGL
> This is an advanced embedded UI library where I would define text, shapes, images, etc., and it would be parsed into pixels to be passed to the E-ink code to display. This helped greatly with UI development and maintainance, and is one of the only viable options for my sort of application.

The software of the watch took a majority of the total time to develop. In hindsight, one of the biggest issues was the very strange complexity and abstraction/lack of within the STM32 ecosystem, although it is very much a standard within the embedded space, the tools provided by STM32 proved to be relatively complicated, buggy, or unnecessary. I had A LOT of issues with STM32's HAL which heavily complicated my code, as the HAL implementation was essentially C utilized to the most hacky extent.

I spent a significant amount of time working on the phone app as well, as this is where many of my requirements lied. Most of the time of this was due to my relative inexperience with Android apps and GadgetBridge itself, although GadgetBridge's code and implementation is very clean and easy to handle or modify.

Additionally, the scale of this project allowed me to see what large-scale projects would look like in terms of code organization, build systems, workflows, etc.

More details can be found in the software post().

## Hardware
- Used KiCad to design the PCB
> KiCad is a free and open source software competitive with many well-known EDAs. The masses of documentation allowed for quick learning and easy usage as well.
- JLCPCB to manufacture the PCB
> JLCPCB is a Chinese company that provides PCB manufacturing services for very cheap(~$5 for my case), I've used them for past projects
- Digikey for parts
> Where the parts are obtained is less important, although I had a good experience here.
- Hand soldered and assembled the PCB
> Using a soldering and hot air station as well as a microscope

The main difficulty of the hardware aspect of this project was actually assembling the PCBs. Personally, designing the schematic was relatively intuitive given that I have previous experience with electrical engineering, and laying out the PCB was not too difficult. The entire project was composed of ~$30 of parts and ~300 separate components to solder. While an assembly of the PCB may only take 6-10 hours(given a slow pace), organizing parts beforehand and debugging hardware issues were both major blockers for me during this stage. Although the project was based on existing and proven designs, it was impossible to determine if something was unfunctional due to a minor soldering error or a fundamental hardware issue. Soldering the 0.5mm FPC connector for the E-ink screen proved to be the most laborious part of this project.

More details can be found in the hardware/PCB posts().

## Summary
In an attempt to list out all of the major features of the project(alothough not all are included):
- Custom Android companion phone app
- Time automatically syncs with phone's time, respects phone's AM/PM or military time setting, automatically update on time zone change
- Full notification support, will display app, subject, and details, and store notifications for later reading
- Display music artist and title when playing a song, and set the background to the song's album art
- Watch buttons can be used to pause, skip, or rewind a song
- Various user settings available on the phone app, including multiple watch faces and dark mode
- Abstractions to easily add additional watch faces or settings
- Display upcoming/future calendar events or alarms on the watch from the phone
- Display incoming phone calls
- Display weather information from a chosen phone app
- Provide direction symbols, distance, and streets while using a navigation app
- Receive and display any image from the phone on demand
- Haptics(vibration motor) for notifications and button presses
- Easy firmware flashing and battery charging through USB-C
- 200mAh battery gives 4-5 day battery life due to E-ink screen and low-power microcontroller actions
- User-friendly UI is efficient and requires little interaction
- Power switch
- Physical buttons and button combos allows for efficient UI navigation
- BLE disconnection and dead battery handling
- Customizable status LEDs
- Smart UI screen management to minimize refreshes and display important information
- Low hardware component price and complexity, ~$30 total, PCBs and components are easily manufacturable or available
- Support for fonts, images, animations, and other necessary UI elements
- Extensibility and customizability through both the watch's firmware and the companion phone app
- Accessible and easily 3D-printable case that is compatibile with essentially any 3D printer, and can be easily modified
- Extensive hardware and software documentation
- BLE connection between watch and phone is optimized to reduce transmissions, data size, and power usage

Personally, the main things I have noticed as I have extensively used this for the past few weeks are:
- Not distracting, very minimalistic in aesthetic and usage
- No major bugs or issues that will break functionality, crash anything, or cause the watch to need to be power-cycled
- Reliably displaying notifications, music, navigation, etc., little to no updates from the phone or vice versa are missed
- Stable for very long periods of time
- Case is tough but not too bulky, watch strap holders work well, the case's design curves to fit my arm
- Great battery
- Easy and speedy iteration, case can be changed quickly and has interchangable 3D-printable parts, software issues can be quickly corrected through flashing via a USB-C cable

## Gallery
{{< multi-figures
    images="/images/aurora40/40-1.jpg, /images/aurora40/40-2.jpg, /images/aurora40/40-3.jpg, /images/aurora40/40-4.jpg, /images/aurora40/40-5.jpg, /images/aurora40/40-6.jpg, /images/aurora40/40-7.jpg, "
    lightbox="gallery"
    row="4"
>}}

#### If you enjoy my writing style or want to see more projects and thoughts from me, consider [reading my other posts](https://merrittlj.github.io/blog/) or [checking out my Github](https://github.com/merrittlj/).
