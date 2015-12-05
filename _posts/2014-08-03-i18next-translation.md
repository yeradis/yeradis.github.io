---
layout: post
title: 'i18next or how to easily build multi-language web applications in javascript'
tags: i18n translation web aplications multi-language internationalization
comments: true
permalink:  
share: true
---

i18next is a full-featured i18n javascript library for translating your webapplication . Runs in browser, under node.js, rhino and other javascript runtimes.

I have discovered this just recently, but i think i18next will be perfect for mobile webapps, those i release with Phonegap/Cordova.

So lets watch a simple sample.

page source:

```html
<!DOCTYPE html>
<html>
  <head>
  	<!-- optional -->
    <script type="text/javascript" src="[PATH]/jquery.js" />
    <script type="text/javascript" src="[PATH]/i18next.js" />
  </head>
  <body>
    <ul class="nav">
      <li><a href="#" data-i18n="nav.home"></a></li>
      <li><a href="#" data-i18n="nav.page1"></a></li>
      <li><a href="#" data-i18n="nav.page2"></a></li>
    </ul>
  </body>
</html>
```
loaded resource file (locales/en/translation.json):

```json
{
  "app": {
    "name": "i18next sample"
  },
  "nav": {
    "home": "Home",
    "page1": "Page One",
    "page2": "Page Two"
  }
}
```

javascript code:

```js
i18n.init({
    debug: true,
    fallbackLng: 'es'
}, function(t) {     
    
    //translate just nav
    $(".nav").i18n();
    
    //or you can translate everything
    $("body").i18n();
	
    // programatical accessing resources
    var appName = t("app.name");
    
    
});
```

You can run the translation by querystring. Just add '?setLng=en-US' to the location href.

For more info on i18next and better examples and uses case please go to http://i18next.com