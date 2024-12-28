---
title: "Raven Smartwatch - Software"
date: 2024-12-27T18:04:00-07:00
draft: false
---

[GitHub profile](https://github.com/merrittlj)
[raven(embedded software) repository](https://github.com/merrittlj/raven)
[raven-gb(GadgetBridge) repository](https://github.com/merrittlj/raven-gb)

# Software
This is the second blog post out of multiple that I am posting regarding my project, the Raven smartwatch. See the first article for a broader introduction on this project, to fully understand the usage of the technologies introduced here. By the way, all of the software and hardware of this project are open-source.

# Technologies
## The smartwatch side
This project is built using a collection of technologies which I have selected through research and previous usages within other projects. Firstly, I will be using CPP with CMake. CPP provides many benefits, providing necessary language additions and also allowing me to gain experience with embedded development within CPP. At this point, while still a contested topic, for this project CPP is the obvious choice due to the decent specs of the microcontroller and due to the fact that we are not in 1960 anymore and are not limited to C. I mainly only use CPP's features such as classes and similar to avoid unnecessary complexity yet still remain beneficial compared to C. CMake provides valuable experience and is easier to work with for larger projects.

I am using the STM32WB55xx microcontroller. The specific details on why I chose this over other microcontrollers will be discussed more in the hardware post, but the main reasons is due to my previous STM32 experience and the great low-power capability of its BLE radio. With this, I am using the HAL and CMSIS provided by STM for this family, but not the ARM RTOS provided, as I saw it as unnecessary compared to FreeRTOS.

While I was initially hesitant to include FreeRTOS within the project, previous experience with using a giant while loop within the project, and the more nuanced timing requirements of the smartwatch necessitated it despite its complexity, and thankfully I am able to use FreeRTOS relatively simply while powerfully in this aspect as I do not require all of the features. FreeRTOS's widespread usage and documentation made this a better option for this project and my experience compared to the (seemingly new) RTOS interface ARM provides.

Additionally, this project uses LVGL for everything displayed on the watch. This helps simplify the code and effort needed to effectively display nice GUIs on the watch, and is widely used throughout embedded systems, so the gained experience is an additional benefit. I am no expert in UI design, but this library is very convienent for creating whatever UI scheme I want.

There are some other libraries or technologies that are not as significant, such as one I use for software button debouncing.

## The phone side
On the other hand, the phone pretty much entirely utilizes GadgetBridge. GadgetBridge was initially designed to provide an open-source solution for replacing vendor-specific apps through providing a generic interface to the different gadgets and reimplementing vendor protocols. See their website for more information. However, in this case, I will design the smartwatch from the ground up with this compatability in mind, avoiding the need for a nuanced app exclusively for the smartwatch and greatly reducing effort required on the phone side to communicate with the smartwatch and provide information. This also helps with gaining experience within Android app development and open-source contribution as a whole(as I have added some features to GadgetBridge). Ultimately, this provides very solid connectivity with the phone, providing notifications, device settings, navigation, music, etc., etc. to the smartwatch, completely removing the need to implement many time-consuming aspects of the smartwatch, such as even on-board storage. Overall, GadgetBridge is a great project, and one that needs more recognition for its extreme usefulness.

The following are currently implemented with support of this app:
* Time: The watch syncs time completely with the phone whenever the phone sets its time(not an event that happens every second, things like timezone changes)
* Notifications: The watch displays and manages notifications, and GadgetBridge provides filtering or other nice features(such as DnD management)
* Events(Alarms/Calendar): The watch displays and manages alarms and calendar(supporting multiple apps) events
* Music: The watch syncs with multiple music players, providing current track, artist, album, and album art
* Navigation: The watch provides updating navigation info and directions when directions are set on one of multiple phone navigation apps

Overall, I would highly recommend GadgetBridge if you want to port an existing vendor-app reliant gadget or if you want to design a gadget around it.

# Conclusion
The next post will be on the specific low-level details of the hardware choices and my current PCB design. I am not limited to these few posts on the different subjects, as I continue development of both the hardware and software of the watch I plan to document various interesting aspects through these blog posts.

If you are interested in learning more about this project or other projects, look at my GitHub and the links provided at the top of the post.
