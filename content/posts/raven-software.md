---
title: "Raven Smartwatch - Software"
date: 2024-12-24T20:21:48-07:00
draft: true
---

# Software
This is the second blog post out of multiple that I am posting regarding my project, the Raven smartwatch. See the first article for a broader introduction on this project, to fully understand the usage of the technologies introduced here. By the way, all of the software and hardware of this project are open-source.

# Technologies
## The smartwatch side
raven repository

This project is built using a collection of technologies which I have selected through research and previous usages within other projects. Firstly, I will be using CPP with CMake. CPP provides many benefits, providing necessary language additions and also allowing me to gain experience with embedded development within CPP. At this point, while still a contested topic, for this project CPP is the obvious choice due to the decent specs of the microcontroller and due to the fact that we are not in 1980 anymore and are not limited to C. CMake provides valuable experience and is easier to work with for larger projects.

see libs

## The phone side
raven-gb repository

On the other hand, the phone will pretty much entirely utilize GadgetBridge. GadgetBridge was initially designed to provide an open-source solution for replacing vendor-specific apps through providing a generic interface to the different gadgets and reimplementing vendor protocols. See their website for more information. However, in this case, I will design the smartwatch from the ground up with this compatability in mind, avoiding the need for a nuanced app exclusively for the smartwatch and greatly reducing effort required on the phone side to communicate with the smartwatch and provide information. Ultimately, this provides very solid connectivity with the phone, providing notifications, device settings, navigation, music, etc., etc. to the smartwatch, completely removing the need to implement many time-consuming aspects of the smartwatch, such as even on-board storage. Overall, GadgetBridge is a great project, and one that needs more recognition for its extreme usefulness.

The following are currently implemented with support of this app:
* Time: The watch syncs time completely with the phone whenever the phone sets its time(not an event that happens every second, things like timezone changes)
* Notifications: The watch displays and manages notifications, and GadgetBridge provides filtering or other nice features(such as DnD management)
* Events(Alarms/Calendar): The watch displays and manages alarms and calendar(supporting multiple apps) events
* Music: The watch syncs with multiple music players, providing current track, artist, album, and album art
* Navigation: The watch provides updating navigation info and directions when directions are set on one of multiple phone navigation apps
