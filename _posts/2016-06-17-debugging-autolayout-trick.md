---
layout: post
title: 'iOS Auto Layout Debugging Trick'
tags: ios autolayout debug log format
comments: true
permalink: debugging-autolayout-trick
share: true
---

Debugging Auto Layout issues in most cases can be easy, but there are some times that this is not the case.

Specially when there is a "complicated" layout and your friend "the unsatisfiable layout error" is making your day a pleasure xD

Mainly because you (and me) don't understand that log printed in the console. 

Because, lets be honest, having a memory address reference plus the Auto Layout Visual Format Language is not something "human readable" (Not if you came from another languages, where the developer experience is better)

So, before you close this page, let me tell you the trick: `accesibilityIdentifier` and contraint `identifier` property.

As simple as that, use those properties and your printed log will turn into something almost friendly xD

Let me show you some examples.

Before using the `accesibiltyIdentifier` property and without constraint `identifier`:

<script src="https://gist.github.com/yeradis/b9f3faf340765339af0fb09a0ecd9dfc.js?file=autolayout_error_before_accesibilityIdentifier.log"></script>

After applying some `accesibilityIdentifier`:

<script src="https://gist.github.com/yeradis/b9f3faf340765339af0fb09a0ecd9dfc.js?file=autolayout_error_after_accesibilityIdentifier.log"></script>

Now, lets use the constraint `identifier` property:

<script src="https://gist.github.com/yeradis/b9f3faf340765339af0fb09a0ecd9dfc.js?file=autolayout_error_after_accesibilityIdentifierAndContraintIdentifier.log"></script>

Really nice, right?

But lets go deeper.

What about the [Visual Format Language](https://developer.apple.com/library/prerelease/content/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW3) ?

<script src="https://gist.github.com/yeradis/b9f3faf340765339af0fb09a0ecd9dfc.js?file=autolayout_error_before_category.log"></script>

Having something like :

```
<NSLayoutConstraint:0x7fc0459d9d60 'id15' V:[cell_box(<=83)]   (Names: cell_box:0x7fc0436676c0 )>
```

Is something i think that can be improved and this is what i see now:

<script src="https://gist.github.com/yeradis/b9f3faf340765339af0fb09a0ecd9dfc.js?file=autolayout_error_after_category.log"></script>

Don´t know about you, but for me having this:

```
<NSLayoutConstraint:id15[0x7fdb9b5cd8d0] UIView:cell_box[0x7fdb9b543e60].height <= 83>
```

Its something better. 

Also, just in case, the address reference is there, but is something that can be removed.

I was able to get that format thanks to a category available in [Mansonry](https://github.com/SnapKit/Masonry)
Specially the category that allows to override the `NSLayoutConstraint description`. 

Check this: [](https://github.com/SnapKit/Masonry#when-the--hits-the-fan)

But my interest was only related to override that console output to make it friendly.

So, in case you want to use it without depending on `Mansonry`, you can get a modified version available at: [](https://github.com/yeradis/objetivec-steroids)

And thats all folks.
