--- 
wordpress_id: 389
title: Filename Cache Busting with CakePHP
wordpress_url: http://www.bwigg.com/?p=389
date: 2012-07-18 20:38:14 -06:00
layout: post
---
<em>This assumes you're using CakePHP 2.x</em>

CakePHP let's you use timestamp based cache busting on your assets by enabling <code>Asset.timestamp</code> in your <code>core.php</code> file. By enabling this and using the builtin <code>HtmlHelper::css</code> and <code>HtmlHelper::script</code> methods, your assets will have a timestamp appended to them as a query string. For example:
<pre>/app.js?123456789</pre>
However, older squid proxies will not cache anything that has a query string, thus preventing your files from being cached. An alternative solution is to move the timestamp into the filename, and then use htaccess or similiar rewrite rule system to map those files back to the actual files on disk. The end output would be something like the following:
<pre>/app.123456789.js</pre>
To implement this in CakePHP, you need to override the assetTimestamp in AppHelper.php

[gist id=3140348 file=AppHelper.php]

Use the following in your .htaccess file. If you're not using Apache, see the <a title="HTML5 Boilerplate" href="http://html5boilerplate.com/">html5 boilerplate</a> for nginx or web.config files.

[gist id=3140348 file=.htaccess]

Read more about filename cache busting at the Html5 Boilerplate Wiki <a title="Cachebusting w. HTML5 Boilerplate" href="https://github.com/h5bp/html5-boilerplate/wiki/cachebusting" target="_blank">entry on cache busting</a>.
