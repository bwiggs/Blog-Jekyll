--- 
wordpress_id: 262
layout: post
title: LAMP Stack Optimizations for Small Servers
date: 2011-01-21 01:07:22 -07:00
wordpress_url: http://www.bwigg.com/?p=262
---
This Wordpress blog is running on a 256MB Slicehost VPS. Here are the settings I have in place to keep Apache and MySQL responsive. Without these settings, the server would often come to a grinding hault and SSH interactions would become very slow and sometimes hang.

<strong>Apache - httpd.conf - mpm_prefork_module Settings</strong>
<code>
<pre lang="apache">
<IfModule mpm_prefork_module>
    StartServers          2
    MinSpareServers       2
    MaxSpareServers       4
    MaxClients            50
    MaxRequestsPerChild   500
</IfModule>
</pre>
</code>

I found that I saved a bunch of memory just by using less apache processes. Instead of spawing 8 processes by default I'm only going to spawn one and limit the max spares to 4, which means after some load I should only have 4 processes lingering around waiting to serve pages.

<strong>MySQL - my.cnf settings</strong>
<code>
<pre lang="mysql">
skip-innodb
skip-bdb
skip-ndbcluster
</pre>
</code>

I dropped mysql's memory useage by about 11M (sitting right under 5M right now) just by disabling innodb support. I've also done some very heavy Wordpress caching using wp-supercache. this basically caches every page and post so that I'm just serving up static html content instead of processing the entire Wordpress stack for every page load.
