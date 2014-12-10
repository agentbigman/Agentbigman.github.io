---
author: jagira
layout: devops
title: Preparing The Server
sitemap: true
lastmod: 2014-09-30 00:00:08
---

When you purchase a VPS or a server from a hosting company, they usually
give you an IP address and root account access. Before you can deploy
your application to server, you will need to secure the server and
install necessary tools on server.

*Steps mentioned in this document are performed on Ubuntu 14.04 server
edition on a DigitalOcean VPS server.* 

### Securing the server

Securing a server is a continuous process as [new vulnerabilities are
discovered everyday](http://en.wikipedia.org/wiki/Shellshock_(software_bug)). 
But you will be relatively safe if you take some
basic security measures like locking root access, not allowing password
authentication, setting up a basic firewall and log watching mechanism
and enabling automatic updates.

1\. Login as root and update package list and upgrade tools.

```
apt-get update
apt-get upgrade
```

2\. Change root password

Change root password to something long and complex and note it
somewhere. We are not going to use root account.

```
passwd
```

3\. Create deploy user

We will be using deploy user to peform all of our deploy activities.

```
adduser deploy
```

4\. Give root privileges to deploy user

Open sudoers file using `visudo` command.

Add the following line to sudoers file.

```
deploy      ALL=(ALL:ALL) ALL
```

5\. Test deploy account

Test deploy user by switching user account and issuing some commands
that require superuser access.

```
su deploy
```
```
sudo apt-get updates
```

6\. Enabling passwordless logins to deploy account

Add your ssh key to authorized_keys on server so that you can login
without passwords.

Run the following command from your dev machine.

```
ssh-copy-id deploy@SERVER-IP
```

7\. Secure SSH and Lockdown SSH

Standard port for SSH is 22 and most attackers attack on that port. Change it 
to some random port number to increase security. 

Also, restrict root login and password authentication.

Open `sshd_config`

```
sudo vi /etc/ssh/sshd_config
```

and make the following changes -

```
Port 23432
PermitRootLogin no
PasswordAuthentication no
```

8\. Setup firewall

Ubuntu comes with `ufw` - a nice utility to configure firewall.

Only allow the ports you are planning to use.

```
ufw allow 80
ufw allow 443
ufw allow 23432
```

9\. Install fail2ban

`fail2ban` monitors your log files and ban hosts that cause multiple
authentication errors.

```
sudo apt-get install fail2ban
```

Default configuration is enough. 

10\. Install and setup logwatch to monitor intrusion attempts

`Logwatch` is a log analysis system. You can configure it to mail log
analysis reports.

Install logwatch

```
sudo apt-get install logwatch
```

Setup a cron task for logwatch to mail you its daily reports

```
sudo vim /etc/cron.daily/00logwatch 
```

add the following line:

```
/usr/sbin/logwatch --output mail --mailto test@gmail.com --detail high
```

11\. Enable automatic security updates

After heartbleed and shellshock bugs, it makes sense to have automatic
security updates enabled.

Install `unattended-upgrades`

```
apt-get install unattended-upgrades

vim /etc/apt/apt.conf.d/10periodic
```

Edit the file to look like this -

```
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Download-Upgradeable-Packages "1";
APT::Periodic::AutocleanInterval "7";
APT::Periodic::Unattended-Upgrade "1";
```

### Install and setup your git account

Install git -

```
sudo apt-get install git
```

[Generate your ssh key and upload it to your git host (Github/Bitbucket).
Make sure everything
works.](https://help.github.com/articles/generating-ssh-keys/)

### Install ruby and other build tools

We will install ruby using rbenv. [Checkout rbenv's readme file for more
details](https://github.com/sstephenson/rbenv)

1\. Install rbenv -

```
# Checkout rbenv into ~/.rbenv

git clone https://github.com/sstephenson/rbenv.git ~/.rbenv

# Add ~/.rbenv/bin to your $PATH for access to the rbenv command-line
utility.

echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
```

2\. Install necessary build tools -

```
sudo apt-get install curl autoconf bison build-essential libssl-dev 
libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev
```

3\. Install ruby-build -

```
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
```

4\. Install ruby ([see available
definitions](https://github.com/sstephenson/ruby-build/tree/master/share/ruby-build)) -

```
rbenv install 2.1.0

# set global ruby version

rbenv global 2.1.0
```

6\. Install bundler to manage gems

```
gem install bundler
```
### Install Apache and Passenger

1\. Install Apache

```
sudo apt-get install apache
```

2\. Install Passenger gem

```
gem install passenger

```

3\. [Install Passenger module for
Apache](https://www.phusionpassenger.com/documentation/Users%20guide%20Apache.html)

```
passenger-install-apache2-module
```

Follow the instructions. Leave the create a virtual host part. We will
deal with it later.

### Install and setup database

#### A\. Instructions for MySQL

1\. Install MySQL

```
sudo apt-get install mysql-server
```

2\. Install dev libs for mysql2 gem

```
sudo apt-get install mysql-client libmysqlclient-dev
```

3\. [Secure MySQL
installation](http://manpages.ubuntu.com/manpages/precise/man1/mysql_secure_installation.1.html)

```
secure_mysql_installation
```

This utility secures MySQL installation by setting root password,
removing anonymous accounts and removing other unnecessary privileges.


4\. Create database and database user

Connect to database using MySQL client

```
mysql -u root -p

```

Create a new database for your Rails application

```
create database myapp_database;
```

Create a new user with local access

```
CREATE USER 'rails_application_user'@'localhost' IDENTIFIED BY 'PASS';
```

Grant database privileges to new user and flush privileges

```
GRANT ALL ON myapp_database.* TO 'rails_application_user'@'localhost';
```

Flush privileges

```
FLUSH PRIVILEGES;
```

#### B\. Instructions for PostgreSQL

1\. Install PostgreSQL

```
sudo apt-get install postgresql-9.3 pgadmin3
```

2\. Install dev lib for pg gem

```
sudo apt-get install libpq-dev
```

3\. Configure and Secure PostgreSQL installation

Change the default `postgres` user password

```
# Issue the following command

sudo -u postgres psql postgres

# Change password in postgres console

\password postgres
```

Configure PostgreSQL to accept local connections using password
authentication (md5 encrypted)

Open `hba` file -

```
sudo nano /etc/postgresql/9.3/main/pg_hba.conf
```

And change the following line -

```
host    all             all             127.0.0.1/32            trust
```

to 

```
host    all             all             127.0.0.1/32            md5
```

Restart PostgreSQL service

```
sudo service postgresql restart
```

4\. Create database and database user

Create user

```
CREATE USER myapp_database_user WITH PASSWORD 'pwd';
```

Create database with new user as its owner

```
CREATE DATABASE myapp_database OWNER myapp_database_user;
```

### Directory structure

Create a directory for your rails application. Some people prefer
to put it in `/var/www` directory while some prefer putting it in `home`
directory.

I prefer deploying to `home` directory.

```
mkdir home/deploy/apps/myapp
```

Since we will be using `capistrano` to deploy our application, we will
also create releases and shared directories inside our application
folder.

```
cd ~/apps/myapp
mkdir releases shared
```

Now is probably the right time to put your database.yml file and other
production config files in shared folder.

In next section, we will create a deploy script using capistrano and
deploy code to server.

