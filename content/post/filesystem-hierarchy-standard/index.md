---
title: "Filesystem Hierarchy Standard"
date: 2024-11-01
url: /filesystem-hierarchy-standard
image: "cover.jpg"
draft: true
categories: 
  - "nerdy stuff"
  - "cyber security"
keywords: 
  - "linux"
tags: 
  - "linux"
---
## A New World
Recently, I had to install a piece of software manually on a Linux system because it was not present in any package manager. While installing, the installer asked me for a target directory for the software. That was the moment I realized I had never really thought about the directory structure of Linux systems. On Windows, I would typically trash everything in `C:\Program Files\`, but for Linux, I had to find a different way. What belongs where?

## FHS
To my surprise, the directory structure is standardized. There is the **Filesystem Hierarchy Standard (FHS)**, which contains a set of conventions on where to put what file on Unix-like systems. That's great! This means, once understood, you can navigate nearly every Linux distribution out there.

Here is my Linux file system for Dummies:

## `/`
This is the root directory of a Linux system. It is the root of every other directory. Everything on the system is contained in `/`.

## `/bin`
Contains basic programs like `cd`, `ls`, and `cat`. These programs can be executed by all users of the system.

## `/sbin`
Contains system programs that, unlike the programs from `/bin`, can only be executed as a sudo user.

## `/lib`
Contains shared libraries. These are basically code snippets/modules used by the programs from `/bin` and `/sbin`.

## `/opt`
Another directory for programs. This time, it's for third-party programs that are self-contained.

## `/etc`
Contains system-wide configuration files.

## `/media`
This is where mount points for removable devices are located. Things like USB drives and DVD-ROMs appear here.

## `/mnt` or `/mount`
Another directory for mounting. The purpose of this directory is to temporarily mount other file systems.

## `/dev`
This directory contains *device files*. These files are needed to work with actual physical hardware, i.e., DVD-ROM drives. I think of them as hardware drivers, to use Windows jargon.

## `/boot`
Contains the Linux kernel and everything that is necessary to boot the system up.

## `/home`
Contains a private space for each user of this system. This is where files like images, documents, or downloads should be stored.

## `/root`
This is the private space of the root user. The root user feels too special to put their stuff into `/home`. That has historical reasons.

## `/run`
This folder contains runtime data. This is volatile data that is used and modified by running programs/processes of the system. This data is also **not** persisted between reboots.

## `/srv`
Servers are little divas, just like the root user. So instead of working with the runtime files in `/run`, they have their own directory, `/srv`.

## `/var`
This directory functions similarly to `/run`. The big difference here is that it does get persisted between reboots. That's why it mostly contains things like log files and caches.

## `/tmp`
This is the temporary directory. All files here get cleared on reboot. This can be useful to keep the system clean. Also, some programs might put files here instead of or in addition to `/run`.

## `/usr`
Until now, everything was kind of straightforward. But now, confusion is introduced by `/usr`. This historically meant *user*, but is not used in that manner nowadays. So it was decided to use the abbreviation for *Unix System Resources* instead.

`/usr` opens up a new directory structure with some duplications from `/`. You will find `/usr/bin`, `/usr/sbin`, `/usr/lib`, `/usr/tmp`, etc. Why is that?

The base idea was to have a directory that can be shared across multiple systems. While most of the programs from `/bin` are dependent on the local directory structure since they use `/lib` and/or `/run`, programs from `/usr/bin` do not use such variable local data at all, or it is stored in a shareable manner as well.

## Conclusion
This is not a complete list of all the directories of the `FHS`. For me, these are the important ones since I now know where to put my applications and files and which directories I better keep untouched.