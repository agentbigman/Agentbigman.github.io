---
author: Jigar Patel
date: '2010-06-26 11:55:00'
layout: post
slug: reddit-api-with-ruby-on-rails
status: publish
title: Reddit API with Ruby on Rails
tags:
- programming
- ruby
redirect_from:
    - /reddit-api-with-ruby-on-rails/
    - /blog/2010/06/26/reddit-api-with-ruby-on-rails/
---

**This post is old!!! Checkout [Reddit's dev site](http://www.reddit.com/dev/api) for detailed API docs.**

Reddit is social news site which is quite popular among geeks.
Reddit provides an API which allows third party apps to access
stories, votes, comments etc. Reddit team is also developing API
that allows application to post data to Reddit. Using Reddit's API
in RoR is a two step process.

**Step 1: Install JSON gem**

{% gist 454006 %}

Reddit provides response in JSON and XML/RSS formats. I personally
prefer JSON over XML because it is easier to read, lighter and
faster to transfer than XML. To deserialize JSON data you will need
the JSON gem.

 

**Step 2: Begin Hacking**

{% gist 453997 %}

The code sends a request to the API url, deserializes the response
using JSON gem and the returns a hash.

Following is a part of such hash. You can easily access the values
like a regular hash.

Make sure that:

-   You do not make more than two request per second.
-   Pages are cached for 30 seconds. So do not put unnecessary load
    on servers by hitting the same page frequently.
-   Reddit provides 25 stories per page and it offers approx. 40
    pages. So do not make more than 40 requests in a go.

Visit [Reddit Code](http://code.reddit.com) for more details.

 



