---
author: jagira
layout: post
title: Deployment Strategies
sitemap: true
lastmod: 2014-09-30 00:00:09
---

Deploying apps is not that tough!

Over years, developers have standardized tool stacks, conventions and practices. 
Using standard stack and following conventions makes DevOps a lot easier.

### Standard Stack

* Linux (Ubuntu) as server OS
* Apache or Nginx as web server
* Passenger / Unicorn / Puma / Thin as application servers
* PostgreSQL or MySQL as database servers

### Use system ruby or install ruby via rbenv / rvm

Ubuntu (and most Linux distros) come with ruby preinstalled. Or they
have ruby in their package repositories. You may use it if you want, but
it is usually a few versions behind. For example, Ubuntu 14.04 comes
with 1.9.3 which latest versions of Rails do not support.

You may also grab source code of ruby and compile manually or using
tools like ruby-build.

Most developers prefer using rbenv or rvm to install and manage multiple
versions of rubies. [Both have their pros and cons]
(http://jonathan-jackson.net/rvm-and-rbenv). I personally prefer rbenv.

In this document, I will use rbenv.

### Apache or Nginx

Use whatever you are comfortable with and suits your use case.

Apache is more popular and has more features. Nginx is lighter and
slightly faster.

In this document, I will use Apache.

### Passenger or Puma or Unicorn

Setting up Passenger is pretty straight forward and [it is the preferred
application server of Rails core team](http://rubyonrails.org/deploy/).

Unicorn and Puma are other popular application servers. [EngineYard has
put up a nice article comparing Passenger, Unicorn and Puma. Do check it
out.](https://www.engineyard.com/articles/rails-server)

In this document, I will use Passenger along with Apache.

### Database

MySQL and PostgreSQL are two of the most popular database servers among
Rails developers.

I will document instructions for both MySQL and PostgreSQL.

### Manual deploys using git or use a deploy script?

Although you can manually deploy application using git to update code,
making necessary changes to server manually and restarting the server,
it is preferred to use a deploy script that automates all these steps
thereby reducing chances of error and making deploys faster.

Capistrano and Mina are two popular deploy automation tools. Up until a
few months ago, Mina was significantly faster than Capistrano as Mina
opened only one SSH connection per deploy as opposed to Capistrano which
opened a new SSH connection per command. [But Capistrano has catched
up, and now it resuses SSH connections making deploy process much
faster](https://github.com/capistrano/sshkit/pull/70). 
[Also, Capistrano supports multiple server deploys which Mina
doesn't](https://github.com/mina-deploy/mina/issues/8).

In this document, I will use Capistrano as it is a more feature rich
tool with a larger ecosystem.
