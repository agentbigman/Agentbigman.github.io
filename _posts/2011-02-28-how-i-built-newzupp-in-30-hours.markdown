---
author: Jigar Patel
date: '2011-02-28 16:55:00'
layout: post
slug: how-i-built-newzupp-in-30-hour
status: publish
title: How I Built Newzupp in 30 Hours
wordpress_id: '56'
tags:
- programming
- ruby
- pet project
redirect_from:
    - /how-i-built-newzupp-in-30-hours/
    - /blog/2011/02/28/how-i-built-newzupp-in-30-hours/
---

Today, I deployed the tested and stable version of
[Newzupp](http://newzupp.com "Newzupp") on my Linode server.
[HN Post](http://news.ycombinator.com/item?id=2271119) It is an
online new aggregator, which aggregates and analyzes
news/link/stories from various sites. \[Digg, HackerNews, Reddit,
Tweetmeme and Facebook as of now\]. I built a similar site in last
April using PHP + CodeIgniter, but since then I was thinking of
migrating it to a site, where users with browsers/smartphones can
share links, img/posts/archives or videos in real time. I chose Ruby on Rails
and started coding, but dropped the idea because of time
constraints.

Sometime during middle of last week, I thought of starting this
project again, and so I did. I coded for an hour or two every day,
with few more hours on weekends, and finished it off last night.
This is the log of the entire process.

I started with a rough sketch of the main page \[other pages look
the same\]. I did not want to invest time in making PSDs or
wireframes, so I did a rough sketch with pen and paper. Here is how
it looked.



![Sketch](/img/posts/archives/how-i-built-newzupp-in-30-hours/1.png)
Then I started collecting the data I had. I already had API methods
for
[Digg](http://jigarpatel.in/digg-api-with-ruby-on-rails "Digg - Newzupp")
and
[Reddit](http://jigarpatel.in/reddit-api-with-ruby-on-rails "Reddit - Newzupp").
I did some research on Tweetmeme API and Facebook API. Both are
fairly simple and well documented. All of these took around 10
hours. I wanted to add HackerNews, but there was no official API.
Then I found
[iHackerNews API](http://api.ihackernews.com/ "iHackerNews") \[Awesome
work by [Ronnie Roller](http://ronnieroller.com/ "Ronnie Roller")\].
In an hour I had a ruby method for HackerNews integration.
Next, I spent some time thinking the architecture. I did not want
it to be too complex, but at the same time I wanted it to be
extensible. Again, after wasting few pages and some ink, I
finalized this data model.



![Architecture](/img/posts/archives/how-i-built-newzupp-in-30-hours/2.png)
I wasted a lot of time (around 4 hours) on this. Then I took a
break of couple of days as I got busy with my day job project.
Coding this entire stuff, partially documenting it and creating a
github repo took another 7-8 hours.
Deploying it took long, as I struggled with cron jobs + rvm. Found
the soultion today morning and deployed the code right away.

This entire process of building the site from scratch took 30 hours
in all (Including the time spent earlier on Digg and Reddit apis).
I could have saved some time by 

-   Faster data modelling
-   Deploying it from day one, as updating the home page/apis is
    much faster on server than on my machine.
-   Researching about the tools used before starting (RVM + cron
    issues)

Using vim for coding saved a lot of time. Apart from vim, I used
Chrome + Firefox for testing and designing, git for version control
and MySQL query browser for handling database.

Other details -

-   Built using Ruby on Rails v3.0.5
-   Site is hosted on Linode 512. I will have to use caching in
    case traffic increases.
-   Source code is shared on
    [Github](https://github.com/jagira/newzupp "Newzupp - Source").
-   The time 30 hours was distributed across several days.

Feel free to provide feedback/suggestions for improving code or
site experience.

Visitor map - (Powered by
[Statcounter](http://statcounter.com "Statcounter"))



![Newzupp\_visitors\_map](/img/posts/archives/how-i-built-newzupp-in-30-hours/3.png)


