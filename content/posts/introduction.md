+++ 
date = 2023-07-16T14:36:50+01:00
title = "Introduction"
description = ""
slug = "introduction"
authors = []
tags = ['non-tech', 'steam-deck']
categories = []
externalLink = ""
series = []
+++

## A Dream
I'm a software engineer, a casual gamer, and a film enjoyer. This makes for a lot of screentime. At 1700, I would log off my work laptop and deftly change the monitor/mouse/keyboard to my PC. When I know what hobby I want to pursue, I can get straight to it, but in those phases where I "have all these games but don't know what to play" or I can't stop scrolling through my [Letterboxd](https://letterboxd.com/iamjosh94/) watchlist feeling uninspired, the default state is to be browsing. Browsing is intensely unsatisfying.

In 2022, I tried to imagine what my life would look like without the Desk of All Activities. I could separate films from the Desk with a sofa and projector set up but work and PC gaming would be harder. I need the Desk for work and if I have the Desk, how can the PC be separate?


## The New Normal
Enter Valve's [Steam Deck](https://www.steamdeck.com). A portable PC gaming device with small-but-mighty specs, the Steam Deck caught my eye not with its portability but with its ability to dock. Undocked, it's a great gaming device but has an uncomfortable desktop experience. Furthermore, The Deck boots to gaming mode by default. I need to switch to the Deck's desktop mode _and_ dock myself in before I can open up a browser with Reddit and mindlessly scroll. Doomscrolling is now an active choice and it becomes a lot easier to say no.

A secondary reason to go for the Steam Deck is that the desktop is Linux-based. Linux-based means Unix-based and that's a platform I'm more familiar with and happier developing on thanks to all the work MacBooks I've been through. Perhaps now I'd feel more motivated (or at least less obstacled) to do some hobby coding.

So I buy the Q3 2022 Steam Deck. It arrives and I love it. My love of gaming is reignited when I can sit on the sofa or in bed and play and the Deck completely replaces my PC overnight. I soon sell the PC for parts and now I'm all in on Deck. When I get that urge to start coding, however, it reveals the Steam Deck's read-only inclinations. 

## Steam Deck Specifics
When first reading about Steam Deck's desktop environment, you'll quickly find out that most of the OS is read-only and only the `/home` and `/etc` directories are writable. This is because the Steam Deck uses A/B booting, where there are two versions of the underlying SteamOS (based on Arch). At any OS upgrade, one is updated before the other and if the update is successful, the Deck loads that newer partition on next reboot. The now old partition is (mostly) wiped clean. This makes the SteamOS very stable on the Steam Deck but unfortunately means that most of the filesystem is kept as read-only and unlocking it all to be writable doesn't change the fact that everything outside `/home` and `/etc` is lost between OS updates.

So the reality is that if I want to continue using SteamOS, I can't install anything that wants to use `/bin` or `/lib`, making installs via `brew` or other normal package managers inconvenient. This is just the beginning of the set up...

## Onwards
Fortunately there are many clever Linux users and tinkerers out there that have paved the way to making the most of the Steam Deck's locked-down system. I have different requirements to most of them, wanting to set up a development environment for JS and general web-dev activities, and I found I have just enough experience in tuning their methods to my vision that I feel I can pass on this knowledge to others in similar situations.

I'll also write about other bits and pieces as I feel like it but this is the basis of this blog.