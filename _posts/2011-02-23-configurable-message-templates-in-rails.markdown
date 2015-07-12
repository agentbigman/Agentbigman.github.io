---
author: Jigar Patel
date: '2011-02-23 20:03:00'
layout: post
slug: configurable-message-templates-in-rails
status: publish
title: Configurable Message Templates in Rails
wordpress_id: '59'
tags:
- programming
- ruby
redirect_from:
    - /configurable-message-templates-in-rails/
    - /blog/2011/02/23/configurable-message-templates-in-rails/
---

Modifying erb templates for small messages is not maintainable when
the number of messages is large. At that time we need configurable
message templates, which can be called from anywhere in the code
(along with some dynamic data). Plus points for storing the
template in database, as we can easily provide a GUI for modifying
'em.

This is one of the thousands of ways of accomplishing it.

{% gist 841051 %}

Here is an example of the GUI -

![Template\_1](/img/posts/archives/configurable-message-templates-in-rails/template_1.png)


