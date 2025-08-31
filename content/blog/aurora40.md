+++
title = "The Aurora40 Keyboard"
date = "2025-08-30T14:48:57-06:00"

description = "The Aurora40 Keyboard"

tags = ["hardware","open source","project","software"]

draft = true
+++

[Full Github repo](https://github.com/merrittlj/aurora40)

Recently I have been wanting to tailor an ergonomic split keyboard exactly to my needs and move towards my vision of an ideal keyboard, as I have been using the [Keebio Iris Rev. 5](https://keeb.io/products/iris-rev-5-keyboard-pcbs-for-split-ergonomic-keyboard) for a few years now and was looking for some change. The Iris is truly an amazing keyboard that I would reccomend to many people, but some of my main issues with it include:
- Normal (not low profile) Cherry switches
> I have found over the years that I truly prefer low-profile feel and ergonomics to the standard profiles
- 56 keys, when I only found myself using 40 or less
> Many keys were pretty much unused, in inconvienent spots, and just added unnecessary bulk in general for me. I much prefer symbol and function layers rather than trying to fit everything on a main layer with lots of reaching.
- Thumb keys
> This is a very personal opinion on a polarizing topic, but I have found dedicated thumb keys(eg. not part of the main grid, angled, etc.) to actually be pretty uncomfortable. Unless the thumb keys are completely molded to the shape and preference of my hands after many revisions, I find them hard and painful to fully use them all of the time. A good thumb key position is nice, but any variation besides perfect too often is very uncomfortable.
- Completely wired
> I'm a big fan of wired tech in general, but I find that wireless on keyboards reduces complexity significantly, improves my experience, and subjectively comes with better software(wireless is usually ZMK rather than QMK firmware).
- Column stagger
> While the Iris does have some column stagger, it is not as extreme as some boards that I have seen before, but regardless I find that I prefer ortholinear without column stagger due to the issues I discussed regarding thumb keys, of how the design not being tailored to your hands(which requires a lot of effort, even if you are already designing a custom keyboard) can easily lead to many ergonomic issues personally.
- Just general complexity
> The Iris features tenting holes, complex RGB backlighting, an acrylic case, etc., which I found that I did not really use any of these at all(my RGB configuration is solid white, anything else is distracting).

{{< multi-figures
    images="/images/aurora40/iris-old.jpg, /images/aurora40/iris-new1.jpg, /images/aurora40/iris-new2.jpg"
    lightbox="iris"
    caption="Iris Rev. 5 - Click to open image gallery"
>}}

I'm sure most users would greatly prefer more features, more keys, and are fine with minor annoyances, but I found that If I were to be using one keyboard constantly for multiple years or longer, I would want it to be as tailored to my preferences as possible.

And anyways, designing and building a custom keyboard is a lot of fun.

## Design
I did not start the project by jumping straight into the PCB or details right away, rather for such a project I decided that the best way to go forward with it would be to write a design document first, outlining the must-haves as well as the features that I certainly did not want to include, here is a basic summary of it:
> - Macropad like feel and design - rectangular grid of keys
> - Low profile Choc switches with MBK keycaps
> - Ortholinear, no column-stagger, all keys should be 1u
> - 5 columns with 4 rows, 20 keys each side, total 40 keys
> - Wireless with nice nano v2
> - No rgb or backlight
> - Include power and reset buttons(instead of shorting pads for reset)
> - No hotswap - hotswap adds complexity and requires a plate, I have never used hotswap on other boards
> - Inspiration:
>   * [Keebio's Nyquist boards](https://keeb.io/products/nyquist-levinson-rev-5-pcb-kit-60-40-split-ortholinear-keyboard)
>   * [kunsteak's briq keyboard](https://github.com/kunsteak/briq)
>   * [peej's For Science keyboard](https://github.com/peej/for-science-keyboard)
{{< multi-figures
    images="/images/aurora40/nyquist.png, /images/aurora40/briq.jpeg, /images/aurora40/for-science.jpg"
    lightbox="inspiration"
>}}
> - No screens, rotary encoders, sliders, trackball, etc.

This gave me a clear mental picture of what I would have to do at each of the stages of building the keyboard, and also prevented me from having to do large redesigns if I decided halfway through a design to change a fundamental thing about the keyboard; I would highly reccomend to create a design document, no matter how simple or small the project is, as this initial investment sped up my progress significantly. 

## PCB
[Kicad repo](https://github.com/merrittlj/aurora40-kicad)
{{< multi-figures
    images="/images/aurora40/schematic.png, /images/aurora40/pcb.png"
    lightbox="pcb"
>}}

Initially I looked into ergogen and the wonderful support and community around it, I think that it is really a great project, but for my situation and knowledge(previous experience with designing complex PCBs and such), I found it much easier to simply use Kicad. Although I may be somewhat biased due to my experience within it, I truly think that designing a keyboard PCB from scratch in an EDA is much less complex and overwhelming than it may seem at first. The schematic itself is very rudimentary(and can be essentially entirely based on existing reference designs), and a keyboard PCB is dead simple; no need for much thought in placement or routing.

The design itself was not too interesting or different from essentially every other keyboard, although for a clean and finished look(as well as reducing space and complexity), I opted to put the microcontroller on the bottom side of the keyboard, allowing it to be hidden, as well as allowing me to use an identical PCB for both halves with no alterations between them(there is actually nothing differentiating them except for which side firmware I put on it). The footprint of the nice nano microcontroller itself had tight spacing with the surrounding keys, but ultimately it worked out better than expected. Unlike the briq keyboard, I did not design a plate or bottom standoff PCB as I could simply make a case to solve these issues, rather than relying on raw PCBs.

## Case
[Case repo](https://github.com/merrittlj/aurora40-case)
{{< multi-figures
    images="/images/aurora40/case.jpg, /images/aurora40/case-py.png, /images/aurora40/case-scad.png"
    lightbox="case"
>}}

## ZMK
[ZMK repo](https://github.com/merrittlj/zmk-config-aurora40)
{{< multi-figures
    images="/images/aurora40/schematic.png"
    lightbox="zmk"
>}}

## Gallery
{{< multi-figures
    images="/images/aurora40/40-1.jpg, /images/aurora40/40-2.jpg, /images/aurora40/40-3.jpg, /images/aurora40/40-4.jpg, /images/aurora40/40-5.jpg, /images/aurora40/40-6.jpg, /images/aurora40/40-7.jpg, "
    lightbox="gallery"
    row="4"
>}}
