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

``` objc
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

```objc
2016-06-16 10:17:54.528 QNChat[90242:8483681] Unable to simultaneously satisfy constraints.
	Probably at least one of the constraints in the following list is one you don't want. 
	Try this: 
		(1) look at each constraint and try to figure out which you don't expect; 
		(2) find the code that added the unwanted constraint or constraints and fix it. 
(
    "<NSLayoutConstraint:0x7fb2451f8d70 V:|-(33)-[cell_contentLabel]   (Names: cell_contentLabel:0x7fb24530e6b0, cell_box:0x7fb242d0ff10, '|':cell_box:0x7fb242d0ff10 )>",
    "<NSLayoutConstraint:0x7fb2453001a0 V:[cell_contentLabel]-(29)-|   (Names: cell_box:0x7fb242d0ff10, cell_contentLabel:0x7fb24530e6b0, '|':cell_box:0x7fb242d0ff10 )>",
    "<NSLayoutConstraint:0x7fb24507d2e0 UITableViewCellContentView:0x7fb242e604d0.topMargin == cell_box.top   (Names: cell_box:0x7fb242d0ff10 )>",
    "<NSLayoutConstraint:0x7fb245089e90 UITableViewCellContentView:0x7fb242e604d0.bottomMargin == cell_box.bottom   (Names: cell_box:0x7fb242d0ff10 )>",
    "<NSLayoutConstraint:0x7fb2450251e0 V:[cell_contentLabel(>=86.3333)]   (Names: cell_contentLabel:0x7fb24530e6b0 )>",
    "<NSLayoutConstraint:0x7fb2453537d0 'UIView-Encapsulated-Layout-Height' V:[UITableViewCellContentView:0x7fb242e604d0(164.333)]>"
)

Will attempt to recover by breaking constraint 
<NSLayoutConstraint:0x7fb2450251e0 V:[cell_contentLabel(>=86.3333)]   (Names: cell_contentLabel:0x7fb24530e6b0 )>
```

Now, lets use the constraint `identifier` property:

```objc
2016-06-16 10:28:10.826 QNChat[90865:8520702] Unable to simultaneously satisfy constraints.
	Probably at least one of the constraints in the following list is one you don't want. 
	Try this: 
		(1) look at each constraint and try to figure out which you don't expect; 
		(2) find the code that added the unwanted constraint or constraints and fix it. 
(
    "<NSLayoutConstraint:0x7fbe004f5a30 'id10' UITableViewCellContentView:0x7fbe004bc6e0.bottomMargin == cell_box.bottom   (Names: cell_box:0x7fbe004c0000 )>",
    "<NSLayoutConstraint:0x7fbe028161e0 'id15' V:[cell_box(>=183)]   (Names: cell_box:0x7fbe004c0000 )>",
    "<NSLayoutConstraint:0x7fbe004ec860 'id6' UITableViewCellContentView:0x7fbe004bc6e0.topMargin == cell_box.top   (Names: cell_box:0x7fbe004c0000 )>",
    "<NSLayoutConstraint:0x7fbe028476b0 'UIView-Encapsulated-Layout-Height' V:[UITableViewCellContentView:0x7fbe004bc6e0(199)]>"
)

Will attempt to recover by breaking constraint 
<NSLayoutConstraint:0x7fbe028161e0 'id15' V:[cell_box(>=183)]   (Names: cell_box:0x7fbe004c0000 )>
```

Really nice, right? 

Another nice thing we get using these properties, is that we can search in XCode for those identifiers :3

But lets go deeper.

What about the [Visual Format Language](https://developer.apple.com/library/prerelease/content/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW3) ?

```objc
2016-06-17 23:05:13.817 QNChat[16948:11377878] Unable to simultaneously satisfy constraints.
	Probably at least one of the constraints in the following list is one you don't want. 
	Try this: 
		(1) look at each constraint and try to figure out which you don't expect; 
		(2) find the code that added the unwanted constraint or constraints and fix it. 
(
    "<NSLayoutConstraint:0x7fc045aa5c50 cell_avatar.top == cell_box.top   (Names: cell_avatar:0x7fc0436f76e0, cell_box:0x7fc0436676c0 )>",
    "<NSLayoutConstraint:0x7fc045ab8fb0 'id1' UITableViewCellContentView:0x7fc043693f90.topMargin == cell_avatar.top   (Names: cell_avatar:0x7fc0436f76e0 )>",
    "<NSLayoutConstraint:0x7fc0436eee00 'id10' UITableViewCellContentView:0x7fc043693f90.bottomMargin == cell_box.bottom   (Names: cell_box:0x7fc0436676c0 )>",
    "<NSLayoutConstraint:0x7fc0459d9d60 'id15' V:[cell_box(<=83)]   (Names: cell_box:0x7fc0436676c0 )>",
    "<NSLayoutConstraint:0x7fc0436e4300 'UIView-Encapsulated-Layout-Height' V:[UITableViewCellContentView:0x7fc043693f90(285.667)]>"
)

Will attempt to recover by breaking constraint 
<NSLayoutConstraint:0x7fc0459d9d60 'id15' V:[cell_box(<=83)]   (Names: cell_box:0x7fc0436676c0 )>
```

Having something like :

```objc
<NSLayoutConstraint:0x7fc0459d9d60 'id15' V:[cell_box(<=83)]   (Names: cell_box:0x7fc0436676c0 )>
```

Is something i think that can be improved and this is what i see now:

```objc
2016-06-17 23:10:43.964 QNChat[17383:11384401] Unable to simultaneously satisfy constraints.
	Probably at least one of the constraints in the following list is one you don't want. 
	Try this: 
		(1) look at each constraint and try to figure out which you don't expect; 
		(2) find the code that added the unwanted constraint or constraints and fix it. 
(
    "<NSLayoutConstraint:0x7fdb9da6c5c0 UIImageView:cell_avatar[0x7fdb9dba8700].top == UIView:cell_box[0x7fdb9b543e60].top>",
    "<NSLayoutConstraint:id1[0x7fdb9b5f5580] UITableViewCellContentView:0x7fdb9b5001c0.topMargin == UIImageView:cell_avatar[0x7fdb9dba8700].top>",
    "<NSLayoutConstraint:id10[0x7fdb9b5f7b20] UITableViewCellContentView:0x7fdb9b5001c0.bottomMargin == UIView:cell_box[0x7fdb9b543e60].bottom>",
    "<NSLayoutConstraint:id15[0x7fdb9b5cd8d0] UIView:cell_box[0x7fdb9b543e60].height <= 83>",
    "<NSLayoutConstraint:UIView-Encapsulated-Layout-Height[0x7fdb9da7da40] UITableViewCellContentView:0x7fdb9b5001c0.height == 285.667>"
)

Will attempt to recover by breaking constraint 
<NSLayoutConstraint:id15[0x7fdb9b5cd8d0] UIView:cell_box[0x7fdb9b543e60].height <= 83>
```

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
