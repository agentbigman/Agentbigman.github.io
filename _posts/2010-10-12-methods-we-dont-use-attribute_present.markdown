---
author: jagira
date: '2010-10-12 19:50:00'
layout: post
slug: methods-we-dont-use-attribute_present
status: publish
title: Methods We Don't Use - 'attribute_present?'
wordpress_id: '87'
tags:
- programming
- ruby
redirect_from:
    - /methods-we-dont-use-attribute_present/
    - /blog/2010/10/12/methods-we-dont-use-attribute_present/
---

We usually check the value of an attribute by using
**object.attribute.blank?**. This method works fine when the
attribute has been defined by the user or it has been loaded by the
database. But it will raise a **NoMethodError** if the attribute is
not defined by the user and is not loaded by the database.

We can use **attribute\_present?** method to check both the
existence of an attribute and its value. The method takes
**attribute name** as message and returns boolean value.

{% gist 622789 %}

