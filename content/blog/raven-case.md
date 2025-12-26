+++
title = "Raven Smartwatch - Case"
date = "2025-12-18T12:54:31-07:00"

description = "Personal project - Raven"

tags = ["project", "hardware", "case", "raven"]

draft = true
+++

- [GitHub profile](https://github.com/merrittlj)
- [raven-hardware repository](https://github.com/merrittlj/raven-hardware)

## Intro
This is the fifth post regarding my current project, the Raven smartwatch. In this post, I will be discussing the designing and building the case as I finished the project.

I required the case to simply hold the entire PCB/screen in place as well as hold button caps to press the main 4 buttons, as the button plungers on the PCB did not extend far enough. Beyond this, I would use a simple back piece to snap fit into the bezel and walls, and some holes for the watch strap, but I would not need any necessarily complex designs. Due to the relatively simple nature of the case, the main factors in consideration for tools was ease-of-use and accessibility.

## Basic details
- Rounded bezel with free space around the screen for visibility
- Button caps for the 4 main buttons
- USB access through a hole
- A holder on the top and bottom for a watch strap to run through
- Snap-fit back plate
- 3D-printed in PLA on a Sovol SV07 Plus, 0.2mm nozzle

## Tools
The case itself is not extremely intriguing, but I believe the philosophy behind the tools I chose to use is. I chose a more OpenSCAD-esque/programmer-oriented approach to designing the case. Rather than learning CAD software from scratch which would also require licenses, trials, and other proprietary crap, and would introduce unnecessary complexity to my project, I chose to use the build123d Python library.

This library offers a approach to building CAD models through Python code. 3D objects can be specified, modified, parametrized, and utilized all through code. I've had previous experience with some OpenSCAD code, so I am familiar with this approach in general, but build123d provided a much more modern and cleaner interface to build models with. The benefit of this approach is that it allowed me to specify certain functions, such as adding a certain style of rounding to an object or putting a hole in the wall aligned with a face, abstracting it for usage across multiple different cases. The parameterization of the models also allowed me to specify certain parameters(such as battery thickness, free space between the bezel and the screen) and quickly change these over iterations, and any related variables or geometry would also change. This allowed for iterations and prints that would simply be changing a line or two of variables, but it would change multiple different geometries to fit these variables, saving me considerable time as compared to maintaining a mental context of what influences what when changing a specification.

As noted in my NixOS post, there is additional benefit to having a declarative-style file which details everything simply and clearly, instead of relying on imperative actions and needing to remember what specific chain of events occurred. Another benefit that I did not use to the fullest extent, but I imagine being extremely significant, is the ability to test the specifications of the result model. For example, I can ensure that my combination of variables for the model does not lead to the case being > 10mm in thickness, or I can ensure that my PCB will fit with at least 1mm of space on all sides.

## Conclusion
Most of the difficulty of the case was printing multiple iterations to find the correct tolerances and spacing for all the various components. Although the watch case is relatively small, it still took around 50m-1h 30m to print. It was also a learning experience for myself, and I hope to use build123d in the future if I need to design any more models.

My next post will be the final conclusion and a general summary of this project.
