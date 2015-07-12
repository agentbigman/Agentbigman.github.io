---
author: Jigar Patel
date: '2011-01-26 17:09:00'
layout: post
slug: secure-browsing-using-ssh-tunnel
status: publish
title: Secure Browsing using SSH Tunnel
wordpress_id: '63'
tags:
- howto
redirect_from:
    - /secure-browsing-using-ssh-tunnel/
    - /blog/2011/01/26/secure-browsing-using-ssh-tunnel/
---

***SSH*** (Secure Shell) is a network protocol which allows secure
communication between two secure devices. It has replaced crappy
protocols like Telnet that use plaintext.

Apart from logging into a remote server, executing remote commands
and file transfer, SSH can be used to forward a port or create a
tunnel. This feature can be used to browse securely.

SSH tunnels can be exploited in several ways -

**Secure Browsing**- All traffic passing via SSH tunnel is
encrypted. This renders tools like Firesheep and coffee shop
sniffers useless.

**Bypassing Firewalls** - Most corporate or college firewalls
filter traffic on port 80 but leave traffic on ports like 22, 24,
465, 993 etc unfiltered. Blocked ports can be accessed vis SSH
tunnels through these ports.

**Access restricted sites** - Sites like Hulu.com are restricted to
USA only. Luckily most cloud providers have their servers in USA.
An SSH tunnel to a machine located in USA can be used to access
such sites. This can be used to access USA only features provided
by services like Google Voice.

**How to create an SSH tunnel** -



![SSH Tunnel](/img/posts/archives/secure-browsing-using-ssh-tunnel/sshtunnels-1.png)

**Requirements -**

-   SSH Server - Most developers have access to an SSH server. If
    not, you can get a micro instance of Amazon EC2 for free.
    [http://aws.amazon.com/free/](http://aws.amazon.com/free/). Every
    stock linux server distro comes with an SSH server along with a
    daemon running.
-   SSH client - Linux and Macs come with a client. Windows users
    get a life putty.

**Creating a Tunnel -**

There are two ways of creating a tunnel. Forward single port or use
dynamic port forwarding. I prefer dynamic port forwarding.

To set dynamic port forwarding use

***ssh -C -N -D PORT user@server***

-N  - Because we do not need to execute any remote commands

-D -  To specify dynamic port forwarding

-C -  To enable compression

When this command is executed, a socket is allocated to listen on
specified port on local machine. In short, ***ssh*** will act as a
proxy server (SOCKS proxy server to be precise). Web browser or any
application capable of using a SOCKS proxy can be configured to use
the tunnel.

Example of proxy configuration in Google Chrome



![Chrome Proxy
Configuration](/img/posts/archives/secure-browsing-using-ssh-tunnel/sshtunnels-2.png)

Extra stuff - Encryption protocol can be changed by ***-c*** flag.

 



