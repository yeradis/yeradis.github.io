---
layout: post
title: 'ALL > TRACE > DEBUG > INFO > WARN > ERROR > FATAL > OFF or how works Log4j levels in our shitty application'
tags: log4j logging levels
comments: true
permalink: log4j-levels
share: true
---

Dealing with Log4j will push us inevitably (but in a good way) to work with Appenders and Levels.

When that moment arrives, and we configure our log4j.properties to just INFO messages.

We will discover there are other type of messages.
And thats because of Levels! There is a hierarchy!

Works in this way:

* `TRACE`:   Shows messages classified as DEBUG, INFO, WARNING,ERROR and FATAL
* `DEBUG`:   Shows messages classified as DEBUG, INFO, WARNING, ERROR, and FATAL
* `INFO`:    Shows messages classified as INFO, WARNING, ERROR, and FATAL
* `WARNING`: Shows messages classified as WARNING, ERROR, and FATAL
* `ERROR`:   Shows messages classified as ERROR and FATAL
* `FATAL`:   shows messages at a FATAL level only
* `ALL`:     You are a genius! Turn on all logging.
* `OFF`:     Yes you are! Turn off all logging.

Let's put this into a drawing:

<img src="http://1.bp.blogspot.com/-WzknlRnfffM/Ul0B9hjzeOI/AAAAAAAA614/NqNGixGPp2g/s1600/log4j+levels.jpg">

And that's all folks.