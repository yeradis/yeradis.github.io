---
layout: post
title: Create a Calendar event from web page info - Bookmarklet
date: 2012-11-6
tags: 
comments: true
permalink:
share: true
---


Someone asked me if I knew how to create an event in Google Calendar using the address of a contact.

To be honest, I did not know if this was possible.
But after a little searching, I found it was something many were asking.

So i set this challenge: Make a Bookmarklet to create events using information from a web page.

Mainly contact information, but , I gave up this idea after trying to understand the page code of Google Contacts xDDDD

So, for the event i will use two sources of information:



> 1) The web page title
2) The text you **select/mark** on the web


This **Bookmarklet** will create a Google **Calendar event** with this info:



> **title**            :  the title from the current web page.
**where**        : here will appear the selected text    
**description** : here will appear the selected text 



Here is the code, feel free to use it or improve it




> javascript:
var title=document.title;
var url='';
var space=' ';
var body, popw;
if(document.selection) {
    body=space document.selection.createRange().txt;
} else if (window.getSelection) {
    body=space window.getSelection();
} else if (document.getSelection) {
    body=space document.getSelection();
}
function buildCalendarURL(eventDetails) {
    return "http://www.google.com/calendar/event?action=TEMPLATE&trp=false"
    "&text=" eventDetails.title
    "&location=" eventDetails.location
    "&details=" eventDetails.details
    "&sprop=" eventDetails.url;
}
var eventDetails = new Object();
eventDetails.title = escape(title);
eventDetails.location = escape(body);
eventDetails.details = escape(body);
eventDetails.url = escape(location.href);
url = buildCalendarURL(eventDetails);
popw = window.open(url,'gcal','scrollbars=yes,width=1000,height=600,top=175,left=75,status=no,resizable=yes');
if (!document.all) T = (setTimeout('popw.focus()',50));
void(0);




"));