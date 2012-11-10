--- 
wordpress_id: 364
layout: post
title: Web.config User Specific appSettings
date: 2012-03-20 15:27:51 -06:00
wordpress_url: http://www.bwigg.com/?p=364
---

On an MVC.NET project at work, we came across the need to be able to override the `web.config`'s `appSettings` uniquely on each of our local environments. Turns out there's a very easy way to do just that inside the `web.config` file.

Just add the file attribute to the `<appSettings>` node in your `web.config`, and add a `local.config` file to the project root. 

**web.config**
{% highlight XML %}
<configuration>
  ...
  <appSettings file="local.config">
    ...
  </appSettings>
  ...
</configuration>
{% endhighlight %}

**local.config**
{% highlight XML %}
<?xml version="1.0" encoding="utf-8"?>
<appSettings>
	<add key="MySQLConnectionString" value="..." />
</appSettings>
{% endhighlight %}


Now when your app loads, it will check to see if there's a `local.config` file relative to the `web.config` file. If there is a file, then it overrides any `web.config` settings it finds duplicates for in the `local.config`. If the `local.config` file does not exist, then it is ignored.

Pro tip: Keep `local.config` out of your code repo and boom! Easily manageable local dev environment settings.

See Also:

+ [MSDN: appSettings Element (General Settings Schema)](http://msdn.microsoft.com/en-us/library/ms228154.aspx) 
