---
layout: post
title: JAVA_HOME and Alternatives on Linux
date: 2012-10-27
tags: 
comments: true
permalink:
share: true
---

Usually when we install Java on Linux, we are "forced" to setup the JAVA_HOME variable.

And we usually perfom this task in this way:


> **
** **sudo nano /etc/profile**

adding this line:



> **export JAVA_HOME=/usr/lib/jvm/java-6-openjdk-i386**


And thats all!

But what about if we have more than one JVM installed on the system and depending on the project we switch the active JVM with "update-alternatives" or "galternatives" ?

So, if we have OpenJDK at JAVA_HOME variable, but our active JVM is another and we want all programs when using JAVA_HOME now use the active JVM, we will need to perform an update on "profile" specifying the new JVM path... again!

Doing that every time we switch between our installed JVM implementation.

I know, this is a pain in the ass.

But dont worry, we are on Linux and getting a solution for this kind or problem is very easy.

Just change that line with this one:


> **
** **export JAVA_HOME=$(readlink -f /usr/bin/javac | sed "s:bin/javac::") **


This will assign JAVA_HOME the current path configured in the /user/bin/javac symbolic link.

So, we can now switch from JVM Alternatives without worry about JAVA_HOME value. "));