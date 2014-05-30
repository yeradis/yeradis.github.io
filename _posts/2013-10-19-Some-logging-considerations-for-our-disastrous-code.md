---
layout: post
title: 'Some logging considerations for our disastrous code'
tags: logging development
comments: true
permalink: logging-considerations
share: true
---

Experience indicates that logging is an important component of the development cycle. It provides precise context about a run of our shitty application.

But that precision with logging, only appears when used conscientiously xD

We usually use logging for:
* aid in problem diagnosis in development without requiring a debug session.
* aid in problem diagnosis in production where no debugging is possible.
You can even use logging to:
* help educate new/old developers in learning/remembering the application.

All that sounds really nice for sure! right ?

The truth is, that, the crude reality reveals something even worse than our disastrous code.

And if you use threads, oh my!!!

The problem appears when we abuse of logging and when we use it with no path.

That path must include the use of the correct type of logging with the correct info.

And we need a good path not only for us, but also for those who will help us.

Having a terrible path is as useful as no using any kind of logging.

Because, a terrible path does not help to follow the flow through the system.

 Now lets try to fix this mess xDDD.

Thats all folks.