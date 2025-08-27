+++
title = "My NixOS experience"
date = "2025-08-27T14:04:43-06:00"

description = "My experiences with NixOS"

tags = ["open source", "software", "opinion"]
+++

## Intro
Over the past ~6 years I've used various Linux distributions as my preferences change over time and as my knowledge expands. Roughly, my journey has gone about:
``Mint(1 year) -> Void(2 months) -> Gentoo(3 years) -> NixOS(1.5 years) -> Alpine(1 month) -> Chimera(1 week) -> Void(2 weeks) -> NixOS(present)``
Some of this may be due to an unnecessary tendency to distro-hop, but I'm glad that I took this path over sticking to one or two distros as this allowed me to experience many different types and aspects of Linux that I would not have seen if I simply stuck to a few the entire time. Mint was my only DE experience, for the rest I used tiling or tiling+floating window managers.

The exact history of the distros is not important however, this is mostly just background context for why I will stay on NixOS. 

## Hacks and Mental maps

Part of the setup process(and general maitenance process) of nearly all of these Linux distros is complicated and time-consuming. Often there are issues with setting up the bootloader correctly given that I run a dual-boot setup on both my laptop and desktop(laptop is Linux/MacOS OpenCore, desktop is Linux/Windows), given the old and niche hardware of the 2012 Macbook I am running for the laptop there are often hardware issues with graphics, touchpad, etc. etc. Often it feels like putting in effort to optimize the system through configuring for power-saving, or cleaning up the system, etc. is not worth the effort as it would be ultimately impermanent and lost if I ever decided to change distros. Additionally, after some time I would forget most of the random hacks or configurations on these systems, and especially what has been implemented on one system and not the other, leading to massive headaches regarding things working out on one system and not the other, reproducibility, etc.

What about NixOS? Find an issue, fix it, forgotten forever, no longer part of your life. The permanence of all these hacks and quirks of your system within configuration files really removes such a large mental burden, outside of just installing, knowing that your system is running perfectly. Need to sync systems? Just git pull and then rebuild the system. Installing a new system? git clone and then build, done.

NixOS's syntax(the Nix DSL) did take some time to get used to, but many people often view massive Nix configuration files and are daunted by the apparent amount of knowledge needed. The reality is, with all programming and technical things, nearly everything can be accomplished from simply modifying examples and only learning a working amount of base knowledge. I don't fully understand what a monad is yet(and I don't care, it's not important to my setup), yet I can have a system massively more functional then anything else.

At the end of the day, even if I blacked out and didn't remember anything I did, I can see my entire system neatly defined in a few configuration files. If I needed to update some bootloader entries, I don't need to grapple with the EFI partition and random configuration files, making sure everything updated correctly, I just add three lines defining the entry within one configuration file and rebuild.

Mentally, I have a picture/map of my system, its components, etc. This is more subconcious than anything but it is very noticable in some scenarios. When hacking through a DIY minimal distro like Void, eventually I forget everything I have done, all the little hacks and details, leading to my mental map feeling like a black box, blurry and unsure of what I have done and what I need to do. This actually leads to some mental stress in using the system as I feel no longer in control of the system. With NixOS, my mental map is forever clear and using the system feels much better mentally. Maybe I want to add some more shortcut functionality to the touchpad. In other systems, I wonder: have I done this before? Where are the files at? Are there driver specific files or is this an Xorg thing? Actually, where are my Xorg configuration files anyways? In NixOS, it is simply: Oh, here is the touchpad.nix configuration I wrote earlier. It uses libinput and natural scrolling. I'll add some shortcuts by just defining them in this array here..

The peace of NixOS is not necessarily easy reproducibility, rollbacks, the declarative package manager, or anything else specifically. Rather, the peace in NixOS is being able to understand your system and modify it at a glance. The peace is in always having a very clear mental map of your system and feeling good using it.

## Multiple hosts

For awhile I have been using Linux on both my main desktop system as well as my laptop, trying to keep them in sync in regard to configuration, distro, etc. rather than having them diverging. Dotfiles are relatively easy to manage with stow, but what about if I want different font sizes between the systems? Different themes? Of course I could create a git branch, write some scripts, maybe stow has some functionality to manage multiple hosts somewhere.. but should all this effort be necessary, just to have different font sizes? The problem isn't managing multiple hosts directly necessarily, rather it is doing so simply and without adding a bunch of extra software crap to my otherwise simple dotfiles setup.

For NixOS however, I simply have two different outputs for my system, as simple as that. Adding another computer would simply require adding another output and then adding the hardware specific details. This expands beyond dotfiles as well. If I want to change all my systems from Xorg to Wayland, that would simply be changing a few lines in the common.nix file that both systems include. If I want to setup a bunch of power-saving settings and daemons, I can simply include that as a laptop module and forget about it. Not only does this include the previously mentioned benefits of permanence and mental clarity, managing both systems is essentially as easy as it can be. I've used home-manager recently within NixOS to setup the Qtile window manager for two different hosts, having different bar configurations for each host for a variety of reasons, and I could'nt imagine a clean and viable method for managing this outside of the clean management of NixOS.

## Conclusion

The main, killer points of NixOS to me personally are found within the sheer mental clarity as well as the ease of implementing multiple hosts. A few other significant points include:
* Easy dev-shell integration: many projects are just pulling and running nix-shell rather than messing with dependencies and build issues.
* Reproducibility: This is a major point of NixOS, but I didn't discuss it much as lots of discussion already exists, but this is a life-saver when installing new systems as well as doing roll backs.
* Philosophy: Many aspects of Linux are arguably artifacts of the past, and ideally moving away from these things into a from-scratch contemporary system should be how many distrobutions are designed. Specifically philosophy-wise, mathematical functions, purity, and other things are important.
* Ease of use: Of course there is a (very) significant setup cost and learning curve, but once you have studied it a bit and understand the basics, NixOS is extremely easy to use a developer, where everything works as it seems it should work, rather than random breakages or quirks.

Despite all these things, however, I want to drive home that NixOS is simply a niche solution for a niche field(experienced developer wanting to make things I work with more comfortable and efficient). I don't think the average Linux Mint or Arch user should just switch over and expect to reap these benefits, the things described are simply my personal issues and how NixOS helps me solve them. 
