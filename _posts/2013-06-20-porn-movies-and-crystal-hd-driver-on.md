---
layout: post
title: Porn movies and the Crystal HD driver on Ubuntu 13.04 Linux kernel 3.8.0-25
date: 2013-6-20
tags: 
comments: true
permalink:
share: true
---

**_Broadcom Hardware Decoder BCM70012 & BCM70015_**

**WHY ARE YOU DOING THIS TO MEEEEE!!!! **
**Or how i hated the Crystal HD driver on Ubuntu 13.04 Linux kernel 3.8.0-25**

After a lot of retries to get the right experience with Crystal HD at Ubuntu, i thought that building from sources  was what i was needing.....

WHAT A TERRIBLE IDEA!

Trying to build from the official sources is like a nightmare, because the source is not updated to support the new linux kernels, in my case version 3.8

WTF!!!!!

Ubuntu sources for Crystal HD driver are not updated.
Broadcom  sources for Crystal HD driver are not updated.

They are for Linux kernels up to 3.4 or thats what i  read in a forum related to this error while building:

**fatal error: asm/system.h: No such file or directory**

Saying that that file is not longer available on newer kernels.

WTF!!!!

At the end with a lot of browsing, and with a solution to my problem.

I have just created a repo at GitHub with all the code patched and the steps to build and install the Crystal HD driver in Ubuntu.

https://github.com/yeradis/crystalhd

Now porn movies run as it should!
