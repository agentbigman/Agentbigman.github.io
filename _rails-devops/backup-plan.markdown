---
author: Jigar Patel
layout: devops
title: Setting Up A Backup Process
sitemap: true
lastmod: 2014-09-30 00:00:06
---

*Anything that can go wrong, will go wrong.*

When things go wrong, and they will, a solid backup process lets you
quickly restore your services. A good backup process should cover all
parts of your application, should be periodic, should be well defined 
and should be tested frequently.

There should be at least one offsite copy of backup.

### Backup Strategies

For a typical Rails application, you will need to backup -

1. Application code
2. Database
3. Configuration files
4. User generated assets

We will see tools for automating backups for each of these.

Storage of generated backup files also has multiple options -

1. Onsite on server
2. Offsite copy on your development machine
3. Offsite copy on another server (*via rsync / sftp*)
4. Offsite copy on a cloud storage service (*S3, Dropbox, etc.*)

You need to consider availability, redundancy and budget while choosing
the storage strategy. An offsite copy on a cloud storage service like S3
is the best option as it is relatively inexpensive and they maintain
*backups of your backup data*.

### Backing up entire server

Almost all server hosts provide a server backup option in which they
take snapshots of your entire server and offer them as restore points.
Restoring a server from a snapshot is usually a one click process.

These options are inexpensive compared to server cost and it is
recommended that you enable backups of your server.

### Backing up code

If you are following good development practices, then all of your code
changes are checked into a version control system and all changes to
code are committed to version control system before being deployed to server.

With a distributed version control system, you probably have multiple
copies of your code repository. Also, you should be using a code hosting
service like Github which takes care of repository backups. If you are
planning to setup your own code hosting service, make sure it has a
foolproof backup plan.

### Backing up database and configuration files

For taking database and file backups, [using Backup gem is a good
option](http://meskyanichi.github.io/backup/v4/). The gem is thoroughly
documented.

Installing and configuring backup gem is pretty simple. [Checkout their
docs for a sample backup config
file](http://meskyanichi.github.io/backup/v4/getting-started/).

### Backing up images and other user generated assets

Backing up of user generated assets like images and videos require
a slightly different approach. Backing up and uploading gigabytes of
assets everytime we perform a backup isn't efficient as cloud storage
services charge for bandwidth consumed.

We can perform periodic syncs of folders containing user generated
assets to S3 or some other host. Only the contents that are changed
during that period are transferred to host.

Same Backup gem provides syncers for Cloudfiles, S3 and rsync.
[Configuring them is also well
documented](http://meskyanichi.github.io/backup/v4/syncers/). 

### Testing Backups

Backups are worth nothing, if you can't restore your service from them.
Test for flaws in backup process or corruption in backed up files
periodically by restoring full service from backed up files. 

You may test backups by restoring data on staging servers.
