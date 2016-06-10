---
layout: post
title: 'Skip the regular iOS launch sequence when testing using an alternative application delegate'
tags: ios testing
comments: true
permalink: alternative-delegate-testing
share: true
---

<a href="http://fewlaps.com/wp-content/uploads/2016/06/Screen-Shot-2016-06-08-at-11.04.57.png"><img src="http://fewlaps.com/wp-content/uploads/2016/06/Screen-Shot-2016-06-08-at-11.04.57-300x74.png" alt="Screen Shot 2016-06-08 at 11.04.57" width="300" height="74" class="aligncenter size-medium wp-image-608" /></a>

Why skipping the regular launch sequence when testing using an alternative application delegate can be useful ?

You can find your own answer, in my case: speed! and because i want to be able to test host application APIs (to run tests in iPhone environment and test Xib loading and other things) but without all the usual launch sequence.
<!--more-->

But what launch sequence i want to avoid?

-> The normal app delegate (and eventually application:didFinishLaunchingWithOptions)

It depends on your app, but it’s not unusual for it to doing things like:

<ul>
<li>Set up all your default data if missing (with Core Data/NSUserDefaults/NSUbiquitousKeyValueStore/Realm/[write yours here])</li>
<li>If there is some stored data reconfigure necessary things, like scheduled local notifications</li>
<li>Check network reachability</li>
<li>Configure the root view controller (in our case if it's the first time there appears a wizard)</li>
<li>Retrieve from a server some configuration</li>
<li> ... N tasks</li>
</ul>

Can be a lot of things to do before we even start testing. 

So, if all we want is to run our tests, why not avoid all that things?

Lets skip it and use an alternative application delegate to speed up the testing process.

The trick here is to create a new class for example: "QNTestingAppDelegate" and put it in the testing target, but only in the testing target.

<a href="http://fewlaps.com/wp-content/uploads/2016/06/Screen-Shot-2016-06-08-at-10.39.27.png"><img src="http://fewlaps.com/wp-content/uploads/2016/06/Screen-Shot-2016-06-08-at-10.39.27.png" alt="Screen Shot 2016-06-08 at 10.39.27" width="172" height="131" class="aligncenter size-full wp-image-602" /></a>

An then in our "main", if the class is available, lets use it :

<script src="https://gist.github.com/yeradis/0c508a130b916f891dae3c6198d5632b.js"></script>

Our "QNTestingAppDelegate" Looks like:

<strong>QNTestingAppDelegate.h</strong>
<script src="https://gist.github.com/yeradis/4cea612937378914588bdff3832b3d32.js"></script>

<strong>QNTestingAppDelegate.m</strong>
<script src="https://gist.github.com/yeradis/9cd373d6b8e8e691cfbd616957dcc2aa.js"></script>

And that's all folks.
