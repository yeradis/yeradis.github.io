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

```objc
2016-06-16 10:22:24.414 QNChat[90514:8499728] Unable to simultaneously satisfy constraints.
	Probably at least one of the constraints in the following list is one you don't want. 
	Try this: 
		(1) look at each constraint and try to figure out which you don't expect; 
		(2) find the code that added the unwanted constraint or constraints and fix it. 
(
    "<NSLayoutConstraint:0x7f82038d3ca0 V:|-(33)-[UILabel:0x7f82039ca670'iPhone Simulator-1:\n2 - t...']   (Names: '|':UIView:0x7f8201617b80 )>",
    "<NSLayoutConstraint:0x7f8201669ee0 V:[UILabel:0x7f82039ca670'iPhone Simulator-1:\n2 - t...']-(29)-|   (Names: '|':UIView:0x7f8201617b80 )>",
    "<NSLayoutConstraint:0x7f8201690990 UITableViewCellContentView:0x7f8201454460.topMargin == UIView:0x7f8201617b80.top>",
    "<NSLayoutConstraint:0x7f8201694000 UITableViewCellContentView:0x7f8201454460.bottomMargin == UIView:0x7f8201617b80.bottom>",
    "<NSLayoutConstraint:0x7f8201661a00 V:[UILabel:0x7f82039ca670'iPhone Simulator-1:\n2 - t...'(>=121)]>",
    "<NSLayoutConstraint:0x7f8203985ab0 'UIView-Encapsulated-Layout-Height' V:[UITableViewCellContentView:0x7f8201454460(199)]>"
)

Will attempt to recover by breaking constraint 
<NSLayoutConstraint:0x7f8201661a00 V:[UILabel:0x7f82039ca670'iPhone Simulator-1:
2 - t...'(>=121)]>
```

After applying some `accesibilityIdentifier`:

<script src="https://gist.github.com/yeradis/b9f3faf340765339af0fb09a0ecd9dfc.js?file=autolayout_error_after_accesibilityIdentifier.log"></script>

Now, lets use the constraint `identifier` property:

<script src="https://gist.github.com/yeradis/b9f3faf340765339af0fb09a0ecd9dfc.js?file=autolayout_error_after_accesibilityIdentifierAndContraintIdentifier.log"></script>

Really nice, right? 

Another nice thing we get using these properties, is that we can search in XCode for those identifiers :3

But lets go deeper.

What about the [Visual Format Language](https://developer.apple.com/library/prerelease/content/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW3) ?

<script src="https://gist.github.com/yeradis/b9f3faf340765339af0fb09a0ecd9dfc.js?file=autolayout_error_before_category.log"></script>

Having something like :

```objc
<NSLayoutConstraint:0x7fc0459d9d60 'id15' V:[cell_box(<=83)]   (Names: cell_box:0x7fc0436676c0 )>
```

Is something i think that can be improved and this is what i see now:

<script src="https://gist.github.com/yeradis/b9f3faf340765339af0fb09a0ecd9dfc.js?file=autolayout_error_after_category.log"></script>

Don´t know about you, but for me having this:

```objc
<NSLayoutConstraint:id15[0x7fdb9b5cd8d0] UIView:cell_box[0x7fdb9b543e60].height <= 83>
```

Its something better. 

Also, just in case, the address reference is there, but is something that can be removed.

I was able to get that format thanks to a category available in [Mansonry](https://github.com/SnapKit/Masonry)

Specially the category that allows to override the `NSLayoutConstraint description`. 

Check this: [https://github.com/SnapKit/Masonry#when-the--hits-the-fan](https://github.com/SnapKit/Masonry#when-the--hits-the-fan)

But my interest was only related to override that console output to make it friendly.

So, in case you want to use it without depending on `Mansonry`, you can get a modified version available at: [https://github.com/yeradis/objetivec-steroids](https://github.com/yeradis/objetivec-steroids)

And thats all folks.
