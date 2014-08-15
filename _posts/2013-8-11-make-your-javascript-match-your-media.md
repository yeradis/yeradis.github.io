---
layout: post
title: Make your JavaScript match your Media Queries - Tip!
date: 2013-8-11
tags: 
comments: true
permalink:
share: true
---

It's easy to adapt the design or resize elements in CSS using Media Queries.

But what if we need to change the content or functionality?

For example, on smaller screens we might want to use fewer or different JavaScript libraries, or modify the actions.

I know its possible to analyze the viewport size in JavaScript but its a little messy, because of all existing ways to do it depending on the browser, also their way to work with existing methods varies from browser to browser.

Even if we successfully detect viewport dimension changes, we  must calculate factors such as orientation and aspect ratios.

Thats why there is no guarantee our calculation will match our browser assumptions when it applies media query rules.

Thats why i prefer to use **window.matchMedia**, because i will be under same browser assumption for media query rules.


So,  here is our Media Query :


```javascript
@media (min-width: 480px) {

 #columns {

  -webkit-column-count: 1;

  -moz-column-count: 1;

  column-count: 1;

 }

}
```

Now, let our JavaScript match same rules:

```javascript
var jmediaquery = window.matchMedia( "(min-width: 480px)" )

if (jmediaquery.matches) {

// window width is at least 480px

}
else {

    // window width is less than 480px

}
```

You can even receive query notification using a listener

```javascript


 var jmediaquery = window.matchMedia("(orientation: portrait)");

 jmediaquery.addListener(handleOrientationChange);

 handleOrientationChange(jmediaquery);


 function handleOrientationChange(jmediaquery) {

        if (jmediaquery.matches) {

            // orientation changed

        }

  }



```

If you no longer need to receive notifications about changes simply call removeListener()

```javascript
jmediaquery.removeListener(handleOrientationChange);
```


Thats all.