---
author: Jigar
date: '2010-10-11 19:28:00'
layout: post
slug: methods-we-dont-use-dup
status: publish
title: Methods We Don't Use - 'dup'
tags:
- programming
- ruby
redirect_from:
    - /methods-we-dont-use-dup/
    - /blog/2010/10/11/methods-we-dont-use-dup/
---

There are plenty of methods defined in Rails framework, which we do
not use. (at least I do not use 'em). I have been exploring rails
source code since some time and have come across some cool methods.
This is my personal documentation of such methods.

**dup** - This method is defined in ActiveRecord::Base. It
overwrites Ruby's *dup* method. *dup* method returns a copy of
record with unfreezed attributes.

{% gist 620877 %}

**Use case** - Can be used if you want to work on an object without
modifying its attributes. Like reverse merging a hash.



