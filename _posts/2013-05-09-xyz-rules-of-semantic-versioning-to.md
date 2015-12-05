---
layout: post
title: "[X].[Y].[Z] The rules of Semantic Versioning to fight the dependency hell"
date: 2013-5-9
tags: 
comments: true
permalink:
share: true
---

As you probably know, in systems with many dependencies, releasing new package versions can quickly become a nightmare.

As a solution to this problem, was born: **Semantic Versioning Specification (SemVer)**

**SemVer**, a set of rules to help us to fight the dependency hell.

According to [http://semver.org][1].

Those rules impose a versioning scheme **[X].[Y].[Z]** that can be summarised as follows:


* **[Z] - **If a patch release includes bugfixes, performance improvements and API-irrelevant new features, **[Z]** is incremented by one.
* **[Y] - **If a minor release includes backwards-compatible, API-relevant new features,**[Y]** is incremented by one and **[Z]** is reset to zero.
* **[X] - ** If a major release includes backwards-incompatible, API-relevant new features, **[X]** is incremented by one and **[Y]**, **[Z]** are reset to zero.

If all of this sounds desirable, all we need to do to start using SemVer is to declare that we are doing so and then link to this website:   from our README,CHANGELOG or whatever, so others know the rules and can benefit from them.

Thats all, happy fight!

[1]: http://semver.org/
