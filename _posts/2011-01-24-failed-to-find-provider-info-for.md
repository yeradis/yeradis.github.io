---
layout: post
title: Failed to find provider info for Calendar, Unknown URL content://calendar/events Android
date: 2011-1-24
tags: 
comments: true
permalink:
share: true
---




Failed to find provider info for Calendar,

Unknown URL content://calendar/events

Android


Hi, if you are reading this, probably you are getting an error on Android 2.0 and above OS when adding calendar event.

And probably that peace of code that was working fine for Android 1.6, not is throwing error for android 2.0 and above versions.

You should know, that, on new Android versions the URI for Calendar content provider has changed,now you should use content://com.android.calendar/

Yes it´s a crap :(

So, if you was using content://calendar/ , to get successful, you should use content://com.android.calendar/

If you want to maintain a compatibility across all Android versions of your apps,
you will need to handle the Old URI along with the New URI, you can do something like this:








    Uri calendarUri;
    Uri eventUri;
    if (android.os.Build.VERSION.SDK_INT 