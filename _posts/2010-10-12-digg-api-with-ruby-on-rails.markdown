---
author: Jigar Patel
date: '2010-10-12 20:01:00'
layout: post
slug: digg-api-with-ruby-on-rails
status: publish
title: Digg API with Ruby on Rails
wordpress_id: '79'
tags:
- programming
- ruby
redirect_from:
    - /digg-api-with-ruby-on-rails/
    - /blog/2010/10/12/digg-api-with-ruby-on-rails/
---

[Digg](http://digg.com) is a social news site which made the
concept of "digging" a story popular. This concept has been
extrapolated to Retweets, Likes etc. If you binge on news, Digg is
the site for you. They also have an extensive API which can be
easily integrated with Ruby on Rails app. As with other APIs Digg
offers data in json format.

I am mainly interested in fetching the data (for my *pet project*)
from Digg instead of posting to it. (posting data to Digg is easy
as well). Below is the code I use to fetch the top 20 hottest
stories from Digg.

{% gist 622817 %}

When you deserialize the json response, you get an array of
stories. You can also specify offset (in form of timestamp) and
limit to fetch more stories.

Note:-

-   I have used this code in Rails 3 and it is working fine.
-   I have not used the new API of Digg v4. They are still
    supporting the old API and I hope they will revert back to older
    version in some time.
-   Do not forget to mention user agent.
