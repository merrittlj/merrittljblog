---
title: "Brawlhalla Network Protocol Analysis - Automation for fun and profit"
date: 2025-08-08T19:47:07-06:00
draft: true
---

## Intro
Recently I have looked into the network protocol of the fighting game Brawlhalla. My intial goal was to explore and understand the basic protocol, but I was able to automate the game quite simply leveraging the protocol. Little has been done on the network side of this game before, so my exploration was completely blind(though I may be missing something pre-existing).

## Setup
My first idea was to use TCPView to view the connections of the game. TCP would certainly be what was used for most of the game(excluding real-time matches, which would obviously use UDP), due to reliability and commonplace compared to other protocols(UDP, HTTPS, etc.).

Using TCPView on brawlhalla.exe led to a few TCP connections on startup, but I chose to focus on one connection which sent much more data then the rest and appeared to send data when I did interactions within the game menus. From my observations, this connection would usually have a destination port of 23001 or 23002, later decompilation of the game confirmed this.

[Python proxy source](https://github.com/merrittlj/brawlhalla-proxy)
There are many methods to intercept/proxy traffic, but for simplicity and maintainability I chose redirect the Brawlhalla program's 23001/2 TCP connection using Proxifier to my local python program which would act as a SOCKS proxy. As for the python program itself, there isn't much to it, mostly just basic proxy logic as well as a filtering setup to easily intercept/modify TCP traffic matching specific patterns.

Wireshark is useful for analysis as well, mostly to initially understand the protocol, where later the proxy can be used itself for modification of the traffic rather than observation.

To best understand this network protocol(and for analysis in general), you should decompile as well. Brawlhalla is an ActionScript(eg. Adobe Flash!) game, so the ffdec/JPEXS flash decompiler can be used. The main game is stored within BrawlhallaAir.swf within the game's local files, this should be the file decompiled. Direct modification of the file doesn't work(due to some integrity checks locally and on the network, I can't bypass this *yet*), though it has shown some promise.

## Initial analysis
Following the TCP stream in Wireshark, one of the first TCP segments(eg. packet) actually starts with the ASCII string "Brawlhalla client to server protocol 1.0". It looks like the connection is encoded but not encrypted, leading to reverse engineering potential. After the game starts up, when doing actions the segments seem to mostly start with the hex 80, with the occasional heartbeat packet looking something like "2f440000". For example, setting the user profile picture to the default causes a segment with the data "80400000000000010c" to be sent from the client to the server.

There aren't many leads currently on what we are looking at, although there are some patterns, the protocol will be much easier to understand after analysis of the decompilation.
