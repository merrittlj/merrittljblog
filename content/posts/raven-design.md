---
title: "Raven Smartwatch - Design"
date: 2024-12-24T20:21:48-07:00
draft: false
---

[GitHub profile](https://github.com/merrittlj)

# Intro
This is the first of multiple blog posts regarding a project I have been working on for the past few months. With this project (unlike previous projects) I have seriously attempted to plan, design, and document all aspects of the project during the phases of the project. In this post, I will discuss my thoughts on the design and planning of the project in its current stance. Posts regarding hardware, software, and UI/UX will follow shortly after this post.

At its core, this project is a learning experience and challenge for myself, yet I aimed to orient this around real-world practicality and application, to the point where a company would be able to sell this. These fundamentals played into the early planning and design, as despite its nature as simply a personal project, practical application would guide it towards a reasonable outcome and prepare for similar scenarios I might find myself within jobs. The hardware, software, UI/UX, and effort towards the project reflects this goal to create a complete product. While this is an attempt to create a real-world product, things such as unoptimized hardware(I am not an expert) and personalized UX ultimately tailor this more towards my usecase, so some usual features(such as health features, which are obviously unused) found in smartwatches are excluded to reduce the scope.

# Design philosophy
Most of these points are self-explanatory, but to get a decent idea:

To list general differentiators and requirements, this smartwatch will
* Use an E-ink display: due to low power usage and the general cool factor
* Use 4 physical buttons(no touchscreen): I find touchscreens hard to use and unnecessary in a smartwatch form-factor, and physical buttons have proven their effective usage within smartwatches like the Pebble, useful for shortcuts
* Use a phone connection through BLE: Wireless compatability is necessary, BLE is the most realistic option given its prominence as a standard and due to the low-power radios that support it
* Display notifications, alarms, calendar events, navigation, etc.: The main usage of a smartwatch regards its ability to conviently provide information from the smartwatch, so ideally the watch should act more as an extension rather than as a standalone item(no need to independently send messages or anything like that)
* Be efficient: The buttons provide great shortcuts, no need for animations(not like we can display animations), random icons or text, etc. found on nearly all modern smartwatches, and the vibration motor can provide distinct sight-free patterns for different functions

As mentioned, the smartwatch is not designed to be standalone, and good usability shouldn't require this. I have thought of the smartwatch as a "mirror" of the smartphone, providing convientent access to information rather than acting as a second phone with all of the same functionality.

# Existing products
## Watchy
For the uninformed, Watchy is another E-ink smartwatch. While partly an inspiration for this project, it has many issues that prevent it from being beyond a tinker-toy. First, the company's main purpose of this was to be an interesting open source project over a widespread and practical product(though I'm sure they did still focus on this), and this is reflected in its general design and support. Watchy lacks meaningful phone connection due to the power inefficiency of its main microcontroller, the ESP32, and due to other design choices. From my view, it mainly just serves the time with some additional stats, which is not very close to my desires to create a multi-use and interconnected watch. While I applaud the effort SQFMI put towards the development of this, due to a lack of notifications and other connectivity, I believe the gap still remains for users desiring minimalistic smartwatch functionality overall.

## Bangle.js, PineTime, & the others
The Bangle.js, PineTime, and other open-source watches are efforts to create a modern and open-source smartwatch. They are certainly better options for users desiring all the functionality modern smartwatches can provide, but as I am not attempting to entirely redefine smartwatches, I believe my niche within the minimalistic and optimized department is still viable(its also a learning project after all).

# Conclusion
Its difficult to fully view this project as a whole as most of the nuance and interesting aspects of the project is within its utilized technologies, hardware, UX, etc(though some difficulty may be due to my obscure writing). Yet, I hope these posts documenting the project provide insight or help regarding the process of creating and managing a full-fledged side project. The next post will be on the software of the watch, such as the libraries or applications it uses, and will provide a better view of the project on a lower level.

See my GitHub and project pages if you are interested.
