---
author: jagira
layout: post
title: "A Guide to Deploying Rails Apps"
date: 2011-12-04 15:09
comments: true
tags:
- devops
- ruby
---

Note _This is not a definitive guide or a tutorial. I have simply collected all my notes related to deployment and compiled them into a blog post. Please check the documentation of the tools/apps mentioned here._

There are three major steps in the deployment process.

###A) Choosing the host / server

There are different types of hosting options that suit different people. You should choose an option depending on your requirements, budget and skills.

These are the popular hosting options in rails community 

+ Shared Hosts e.g Dreamhost
+ VPS providers  e.g Linode, Slicehost
+ Cloud servers e.g EC2, Rackspace Cloud (More or less similar to VPS boxes)
+ Dedicated Servers e.g Softlayer, Rackspace
+ PaaS Providers e.g Heroku, EngineYard

Choose _shared hosts_ for extremely small personal projects only.
These are _cheaper_ than the rest. However, these are _extremely
low on resources_ and the host may terminate your account if your
scripts are consuming resources more than what your account is allotted. Additionally, you _do not get root access_ to your server.

_Dreamhost_ is a popular shared host for rails.

I tend to put _VPS boxes_ and _cloud servers_ in the same
basket. Although their underlying technologies are different, as an end
user ,I get more or less the same functionalities and same performance.
Simply put these are virtual machines hosted on large hardware boxes and
behave almost like a typical UNIX box. You can get memory configurations ranging from _64 MB_ to _68.4 GB_ (on EC2). The _CPU power usually is directly proportional to your memory size_. Only exception to this case is EC2 where you get to choose the CPU configuration as well.

These are the popular VPS and cloud server hosts -

<table class="table">
  <tr>
    <th><strong>Host</strong></th>
    <th></th>
    <th><strong>Notes</strong></th>
  </tr>


  <tr>
    <td>Prgmr</td>
    <td></td>
    <td>Cheap, Good for small projects and side projects. Difficult to manage as it comes with a management console.</td>
  </tr>
  <tr>
    <td>Linode</td>
    <td></td>
    <td>Cheap, good for most projects. Nice service and consistent performance. High Memory / $</td>
  </tr>  	
  <tr>
    <td>Rackspace Cloud</td>
    <td></td>
    <td>Per hour billing. Easy to setup. Costlier than Linode. Good performance.</td>
  </tr>
  <tr>
    <td>Amazon EC2</td>
    <td></td>
    <td>Per hour billing. Expensive. Performance depends on configuration. CPU on micro instance sucks. Variety of configuration options. Choose Medium or Large CPU instance for CPU intensive apps.</td>
  </tr>
</table>


There are few other providers as well. The only negative point about VPS / cloud servers is _poor disk I/O_. Get a dedicated server if I/O causes to be a bottleneck.

_Dedicated servers_ are _expensive_  than the above mentioned options. They are full fledged hardware boxes. They usually come with a fully managed contract or self managed contract. Choose fully managed servers if you do not have in-house server admin skills. Some providers also let you choose an SSD (albeit at a premium). SSD's provide a remarkable performance improvement in disk I/O.

Softlayer and Rackspace are popular options for dedicated boxes.

_PaaS_ providers are the _most expensive_ among the above mentioned hosts. They provide a fully managed ruby/rails stack built on top of services like Amazon EC2. Their _performance is similar to the underlying cloud service_. Choose them if you want to focus only on coding and _forget about server admin stuff_. Engine Yard and Heroku are the most popular options. 

_Engine Yard_ provides you a platform along with an EC2 instance. You
can ssh into that instance and install anything. 

_Heroku_ is a bit more restrictive. You get a _read only file system_, you can _only use postgres_ as your db and you _do not get ssh access_ to the server. You can, however, install addons to overcome these restrictions.


#### A1) Choosing the server config

Determine the number of users and the max number of requests/s that you want to handle.

Determine the amount of memory consumed by your services and application server instances.

Number of application server instances required  = (Max requests/s) / (Time taken by the longest request)

Minimum memory required = Memory consumed by the services + number of application server instances required * memory consumed by each application server instance

#### A2) Free servers

If you are running short on cash, or you simply want a server to learn stuff or demonstrate a prototype, buying a server does not make sense. There are few providers that let you test the water before you dive.

_EC2 Micro_ - Free for first year. Around 600 MB of RAM / Low CPU

_Heroku_ - One free dyno (presumably forever). Good for a small site. No background jobs.

_Engine Yard_ - 500 hours free with a new account. One medium CPU EC2 instance. Extremely powerful.

____________________________________________________________________________


### B) Setting up the server

The hosts typically let you choose the operating system and mail you the root access details. In case of EC2, you have to download the .pem files.

_The next steps are based for Ubuntu Server 10.04. Refer to the
documentation of the OS in case you choose another OS. The steps, however, remain the same._
 
#### B1) Setup user accounts

_Do not deploy using the root account or your personal account. It is recommended that you create a separate account for deploying apps._

##### 1) ssh into the system using root credentials

##### 2) Change the root password
{% highlight bash %}
passwd
{% endhighlight %}

##### 3) Create new group for deployers
{% highlight bash %}
/usr/sbin/groupadd deployers
{% endhighlight %}

##### 4) Give sudo privileges to users of deployers group

Issue the visudo command
{% highlight bash %}
/usr/sbin/visudo
{% endhighlight %}
Scroll down to the bottom and add the following two lines
{% highlight bash %}
## Allows people in group deployers to run all commands
%deployers  ALL=(ALL)       ALL
{% endhighlight %}
Save and Exit.

##### 5) Create new user/s
{% highlight bash %}
/usr/sbin/adduser deployer
{% endhighlight %}
	
##### 6) Add deployer user to deployers group
{% highlight bash %}
/usr/sbin/usermod -a -G deployers deployer
{% endhighlight %}
	
##### 7) Logout and login as deployer

#### B2) Preparing the stack

You can now install the tools/software necessary for your application. Replace the commands for tools like Apache/MySQL with the commands for your tools.

##### 1) Update the system
{% highlight bash %}
sudo apt-get update
{% endhighlight %}
	
##### 2) Install MySQL and libmysqlclient16-dev \[dependency for mysql2 gem\]
{% highlight bash %}
sudo apt-get install mysql-server mysql-client libmysqlclient16-dev
{% endhighlight %}
	
##### 3) Install Apache
{% highlight bash %}
sudo apt-get install apache2
{% endhighlight %}
	
##### 4) Install Git, Curl and build-essential \[to compile ruby\]
{% highlight bash %}
sudo apt-get install build-essential git-core curl
{% endhighlight %}
	
##### 5) Install rvm
{% highlight bash %}
bash < <(curl -s https://rvm.beginrescueend.com/install/rvm)
{% endhighlight %}
	
##### 6) Add the following line to ~/.bashrc
{% highlight bash %}
'[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm"'
{% endhighlight %}
	
##### 7) Install the necessary packages
{% highlight bash %}
sudo apt-get install build-essential bison openssl libreadline6 
libreadline6-dev curl git-core zlib1gzlib1g-dev libssl-dev 
libyaml-dev libsqlite3-0 libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf
{% endhighlight %}	

##### 8) Install ruby 1.9.2
{% highlight bash %}
rvm install 1.9.2
{% endhighlight %}
	
##### 9) Make 1.9.2 the default interpreter
{% highlight bash %}
rvm --default use 1.9.2
{% endhighlight %}
	
##### 10) Install bundler
{% highlight bash %}
gem install bundler
{% endhighlight %}
	
##### 11) Install passenger
{% highlight bash %}
gem install passenger
{% endhighlight %}
	
##### 12) Install Apache2 module
{% highlight bash %}
passenger-install-apache2-module
{% endhighlight %}
passenger-install will notify about the missing dependencies. Install them.

##### 13) Passenger install will complete with instructions about adding adding some lines to apache config file. Add them to apache config file.
{% highlight bash %}
sudo nano /etc/apache2/apache2.conf
{% endhighlight %}

##### 14) Restart Apache
{% highlight bash %}
sudo /etc/init.d/apache2 restart
{% endhighlight %}

Now your server is ready to serve rails apps.


### C) Deploying application

This part requires changes both to your application and to the server. I am also assuming that the app is to be deployed using capistrano. 

##### 1) Add capistrano gem to the application and capify the app

Add to the Gemfile
{% highlight bash%}
gem 'capistrano'
{% endhighlight %}

Run bundle install and then capify
{% highlight bash%}
bundle install

capify .
{% endhighlight %}

This will create a deploy.rb in the config folder. You can modify this file according to your config. The vanilla version is sufficient for normal apps.

##### 2) Setup the database on the server

*Change DATABASE_NAME and DB_USERNAME according to your settings.*

ssh into the sever

Login to the MySQL database using the root password (blank by default)
{% highlight bash%}
mysql -u root -p
{% endhighlight %}

Create database for the rails application
{% highlight bash%}
create database DATABASE_NAME;
{% endhighlight %}

Create user for database
{% highlight bash%}
grant all on DATABASE_NAME.* to 'DB_USERNAME'@'localhost' identified by 'DB_PASSWORD';
{% endhighlight %}

Flush privileges
{% highlight bash%}
flush priviliges;	
{% endhighlight %}
	
##### 4) Create folder for app
{% highlight bash%}
sudo mkdir /var/www/APPLICATION_NAME	
{% endhighlight %}

##### 5) Change ownership of the folder
{% highlight bash%}
cd /var/www
sudo chown deployer APPLICATION_NAME
{% endhighlight %}

##### 6) Update deploy.rb file

Update the server addresses, repository details and APPLICATION_NAME in your deploy.rb file.

##### 7) Update ssh keys on repository

Add server's ssh keys to your repository (Github)

##### 8) Setup folder for deploy
{% highlight bash%}
cap deploy:setup
{% endhighlight %}
	
##### 9) Create database.yml

Create database.yml in shared folder with proper credentials.

##### 10) Make a cold deploy
{% highlight bash%}
cap:deploy:cold
{% endhighlight %}
	
##### 11) Deploy with migrations
{% highlight bash%}
cap deploy:long
{% endhighlight %}
	
Your app is now deployed. You now need to configure Apache (or Nginx) so that it can serve your rails app.

##### 12) Create a virtual host
{% highlight bash%}
sudo nano /etc/apache2/sites-available/app-name
{% endhighlight %}

##### 13) Add the following to the virtual host

Change the IP address, domain name and the application name.

{% highlight bash%}
<VirtualHost *:80>  
  ServerName  XXX.XXX.XXX.XXX  
  ServerAlias subdomain.abc.com  
  DocumentRoot /var/www/APPLICATION_NAME/current/public
  RackEnv production
</VirtualHost>
{% endhighlight %}

##### 14) Enable the new site
{% highlight bash%}
sudo a2ensite demo-app
{% endhighlight %}

##### 15) Restart Apache
{% highlight bash %}
sudo /etc/init.d/apache2 restart
{% endhighlight %}

Your application is now deployed and live. :-)

______________________________________________________________________

_P.S Let me know if you find any errors. I will fix 'em._

_P.S If you get stuck with any of the steps or need some extra
guidance hit me on twitter. (<a
href="http://twitter.com/jagira" target="_blank">@jagira</a>). I will try to help if time permits._

_P.S I am not a professional server admin and the above mentioned steps may violate some best practices. Share such instances with me._
