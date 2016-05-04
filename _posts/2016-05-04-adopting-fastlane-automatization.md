---
layout: post
title: 'Adopting Fastlane for automating our daily iOS developements tasks'
tags: ios fastlane automatization deployment
comments: true
permalink: adopting-fastlane-automatization
share: true
---

<img class="aligncenter wp-image-569 size-medium" src="http://fewlaps.com/wp-content/uploads/2016/04/fastlane_logo-300x83.png" alt="fastlane_logo" width="300" height="83" />

&nbsp;

For those who don't know:  <a href="https://fastlane.tools/">Fastlane</a> is the Holy Grail (the one that Indiana Jones was really looking for, if only he had been a programmer)

<a href="https://fastlane.tools/">Fastlane</a> helps you automate every aspect of your release to the App Store.

It allows coders to deploy app updates from any computer, set up and submit apps more easily, and automate the use of the standard commands required for app revision and testing.

We can even add components on top of Fastlane itself — the tools support customized, user-built actions to extend their functionality.

But what exactly are the usual steps around iOS development and releases to the App Store ?

<strong>Current steps (of course can vary):</strong>
<ul>
    <li>Pull changes from remote repository</li>
    <li>Run tests (including UI testing) (if got lucky there is nothing to fix)</li>
    <li>Increment build number (and version number if required)</li>
    <li>Check provisioning profiles and certificates for code signing (Freddy Krueger was the creator of all this process for sure!)</li>
    <li>Build new binary version</li>
    <li>Generate release notes for every supported localisation</li>
    <li>Take screenshots for every device and every supported localisation</li>
    <li>Prepare new version on iTunes Connect (and TestFlight)</li>
    <li>Upload binary to iTunes Connect</li>
    <li>Share with testers</li>
    <li>Upload binary to Crashlytics Beta</li>
    <li>Share with testers</li>
    <li>Push changes to remote repository</li>
</ul>
😩

That is a lot to handle in one day!

Because just taking into account the binary build time plus iTunes related stuff time.... got and get your bag to sleep at the office.

Of course, the duration can be different depending on some miracles (cof cof <a href="https://twitter.com/hashtag/iosprocessingtime">#iosprocessingtime</a>).

&nbsp;

So publishing a new version to iTunes App Store is very awkward and time consuming.

But, no more!

Now with a single command:

<script src="https://gist.github.com/yeradis/0327d9a9f26e86e8d32a07df9e325f5b.js"></script>I do all these steps:

<ul>
    <li>Increment project build number</li>
</ul>
<ul>
    <li>Download up-to date provisioning profiles for code signing</li>
</ul>
<ul>
    <li>Build/Archive the app using the correct scheme (QuitNow or QuitNow Pro)</li>
</ul>
<ul>
    <li>Notify to channels (cli, slack, mac os x notification center)</li>
</ul>
<ul>
    <li>Upload a perfectly signed binary to TestFlight/Crashlytics for Beta testers to try</li>
</ul>

 And i just need to use my time in another task while Fastlane save me 30 minutes.

<img class="aligncenter wp-image-567 size-full" src="http://fewlaps.com/wp-content/uploads/2016/04/Screen-Shot-2016-04-27-at-14.57.21.png" alt="Screen Shot 2016-04-27 at 14.57.21" width="639" height="711" />

The last step: "<strong>notification</strong>" , puts a message at the console and also at the notification center.... a reminder like:

<a href="http://fewlaps.com/wp-content/uploads/2016/04/Screen-Shot-2016-04-28-at-10.27.45.png"><img class="aligncenter wp-image-581 size-full" src="http://fewlaps.com/wp-content/uploads/2016/04/Screen-Shot-2016-04-28-at-10.27.45.png" alt="Screen Shot 2016-04-28 at 10.27.45" width="312" height="197" /></a>

Fastlane<strong> pilot_free</strong> task looks like this:<script src="https://gist.github.com/yeradis/aec5aa8b0c75e4fa45de2eb016a1d6c3.js"></script>

You can improve the process to fit your needs, including running test, syncing changes from remote repositories and much more.

You can even make it to work only on specific git branches and only if they are not dirty. And you can commit changes at the end, like committing the new build number bump and push it to the remote repository.

Lets see our lane for releasing a new version to iTunes:

<script src="https://gist.github.com/yeradis/6db8730d615c0a45aa623794a5cd4ca0.js"></script>

Another automation example: Generate APNS Certificates containers for your server (PUSH notifications related)

<script src="https://gist.github.com/yeradis/a5191ffdcf0a19deea913c5b117b9565.js"></script>


