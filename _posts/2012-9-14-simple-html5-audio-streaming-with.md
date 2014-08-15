---
layout: post
title: Simple HTML5 Audio Streaming with Playlist for iOS
date: 2012-9-14
tags: 
comments: true
permalink:
share: true
---


In you want to make  a very simple iOS HTML5 compatible Audio player with Playlist support, then, this huge post is for you.

But do not bee fooled, this is a super complicated engineering technique.....

**< audio src="http://youserverurl/yourplaylist.m3u" type="audio/mpeg" > < / audio > **

WTF? THATS ALL ?

YES!!! That's all you need to have an HTML5 Audio player with Playlist support on your iOS device.

Sure you already knew that the AUDIO parameter SRC accepts an url to a mp3 file, or a live streaming broadcast like those provided by [Icecast][1] servers, so, here is another way using a M3U playlist format.

**yourplaylist.m3u **is just a simple text with this content:


#EXTM3U
#EXT-X-TARGETDURATION:10
#EXT-X-MEDIA-SEQUENCE:1

http://yourserverurl/mymusicforlder/track1.mp3
http://yourserverurl/mymusicforlder/track2.mp3
http://yourserverurl/mymusicforlder/track3.mp3

... (this means more lines, a music by line)

http://yourserverurl/mymusicforlder/track10000000.mp3


#EXT-X-ENDLIST
"));

[1]: http://www.icecast.org/
