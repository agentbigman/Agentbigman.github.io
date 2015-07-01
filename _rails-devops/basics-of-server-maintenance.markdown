---
author: jagira
layout: devops
title: Basics of Server Maintenance
sitemap: true
lastmod: 2014-09-30 00:00:05
---

A server requires certain routine maintenance tasks to be performed
regularly in order to acheive maximum performance. In this section, we
will checkout some basic maintenance tasks and how to automate them.

### Monitoring and managing processes

Processes on a Linux server at times can go haywire. An Apache server
process can hog all CPU resources or a terminated redis server can halt
disrupt your caching layer. It helps to have a basic process monitoring
system in place.

Monit is a simple program that monitors and manages processes on server
and can be configured to make sure that the process stay online and run
within preset resource limit.

Monit is availble in repositories of most distributions and can be
installed via package manager.

`sudo apt-get install monit`

Monit configuration is stored in `/etc/monit/monitrc` file. Configuring
monit is pretty easy and is well documented on [monit's wiki
page](http://mmonit.com/monit/documentation/monit.html).

Example configuration for monitoring apache server process:

```
check process apache2 with pidfile /var/run/apache2.pid
    start program = "/etc/init.d/apache2 start"
    stop program = "/etc/init.d/apache2 stop"
```

Read more about it
[here](http://mmonit.com/monit/documentation/monit.html#SERVICE-METHODS).

[Monit can also alert you when a service stops or crosses a threshold
(load value, cpu usage, filesystem usage,
etc.)](http://mmonit.com/monit/documentation/monit.html#ALERT-MESSAGES)

### Managing server updates


### Managing databases


### Managing log files




