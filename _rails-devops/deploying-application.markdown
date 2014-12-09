---
author: jagira
layout: post
title: Deploying Application
sitemap: true
lastmod: 2014-09-30 00:00:07
---

Deploying a Rails application is a 3 step process:

1. Upload application code to server
2. Run database migrations, compile assets and perform other tasks
   required by your Application
3. Restart application server (and other services like background
   workers)

### Automating deploys using [Capistrano](http://capistranorb.com/)

You can do all these steps manually by logging into server using ssh,
copying code using git or scp, running migrations and other tasks, and
restarting application server. But this becomes a big time sink when you
are deploying code frequently (say multiple times a day) or to multiple
servers. Doing these tasks manually also carries a risk of committing errors.

Hence, devs prefer automating deploy tasks. Capistrano is the most
popular deploy automation tool for Rails apps. [Capistrano lets you
define your deploy tasks using a simple DSL](http://capistranorb.com/).
A lot of ruby libraries (Sidekiq, Airbrake, etc.) come with extensions
for Capistrano which provide ready to use tasks in your deploy script.

### Deploying application code

You need to install and setup your app for Capistrano before you can use
it.

1\. Install Capistrano

Add `capistrano-rails` to your Gemfile.

```
gem 'capistrano-rails'
```

We will also add [`capistrano-rbenv`](https://github.com/capistrano/rbenv) to 
Gemfile as it adds rbenv support to capistrano.

```
gem 'capistrano-rbenv'
```

And run bundle install.

2\. Initialize Capistrano

From your application directory, run `cap install` command. It will
generate a few files.

```
|---Capfile
|
|---config
|    |
|    |---deploy
|    |   |
|    |   |---production.rb
|    |   |---staging.rb
|    |
|    |---deploy.rb
|
|---lib
     |
     |---capistrano
         |
         |---tasks
```

3\. Configure server details in generated files

Specify your server address in `config/deploy/production.rb` file.
Capistrano assumes three roles for your servers - `web, db and app`. In
a simple one server setup, all of these will have same address.

```
role :app, %w{SERVER-ADDRESS}
role :web, %w{SERVER-ADDRESS}
role :db,  %w{SERVER-ADDRESS}
```

Capistrano also generates a config file for staging environment. You may
generate and configure files for other environments if you want.

4\. Set shared details in deploy.rb

Place configuration common to all environments (like repository url) in deploy.rb.

In a barebones setup, we need to set the following -

```
# Application name
set :application, 'myapp'

# Git repository url
set :repo_url, 'git@github.com:me/myapp.git'

# Directory to deploy the app
set :deploy_to, '/home/deploy/apps/myapp'

# Symlink database.yml
set :linked_files, %w{config/database.yml}
```

This configuration is enough to get us started.

5\. Run deploy command

At this stage, you are ready to run deploy command. 

Run `cap production deploy` to deploy code to production.

### Configuring web server to serve Application

After we deploy code, we need to tell Apache and Passenger to serve our
application to web.

We will use a virtual host file to set it up. 

Create a virtual host file in `/etc/apache2/sites-available` directory.

```
sudo vi /etc/apache2/sites-available/myapp_production
```

And enter the following -

```
<VirtualHost *:80>
    ServerName myapp.com
    
    # !!! Be sure to point DocumentRoot to 'public'!
    DocumentRoot /home/deploy/apps/myapp/current/public/
    
    <Directory /home/deploy/apps/myapp/current/public>
    
    # This relaxes Apache security settings.
    AllowOverride all

    # MultiViews must be turned off.
    Options -MultiViews

    </Directory>

</VirtualHost>
```

Save the file and enable the site.

```
sudo a2ensite myapp_production
```

Reload Apache to get this site served on web.

```
sudo service apache2 reload
```
