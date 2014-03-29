---
author: jagira
date: '2010-09-15 06:24:00'
layout: post
slug: rails-3-does-not-auto-load-classesmodules-from-lib-folder
status: publish
title: Rails 3 Does Not Auto-Load Classes/Modules From "/lib" Folder
tags:
- programming
- ruby
---

In Rails 3 classes and modules from "**/lib**" folder are not
automatically loaded. If you create any module/class in lib folder
and try to use it, the server will throw an
"**uninitialized constant XYZ (NameError)**".

The change is deliberate and there is a configuration setting in
***application.rb*** to specify the folders which are to be auto
loaded.

{% highlight ruby %}
# Custom directories with classes and modules you want to be autoloadable.
# config.autoload\_paths += %W(\#{config.root}/extras)
{% endhighlight %}

Uncomment these lines and modify the path to 

{% highlight ruby %}
config.autoload\_paths += %W(\#{config.root}/lib)
{% endhighlight %}

This will autoload all the classes and modules in the "**/lib**"
folder.



