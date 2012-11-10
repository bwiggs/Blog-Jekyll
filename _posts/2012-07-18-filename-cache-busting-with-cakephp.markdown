---
layout: post
title Filename Cache Busting with CakePHP
---
This assumes you're using CakePHP 2.x

CakePHP let's you use timestamp based cache busting on your assets by enabling Asset.timestamp in your core.php file. By enabling this and using the builtin HtmlHelper::css and HtmlHelper::script methods, your assets will have a timestamp appended to them as a query string. For example:

/app.js?123456789
However, older squid proxies will not cache anything that has a query string, thus preventing your files from being cached. An alternative solution is to move the timestamp into the filename, and then use htaccess or similiar rewrite rule system to map those files back to the actual files on disk. The end output would be something like the following:

/app.123456789.js
To implement this in CakePHP, you need to override the assetTimestamp in AppHelper.php

[gist id=3140348 file=AppHelper.php]

Use the following in your .htaccess file. If you're not using Apache, see the html5 boilerplate for nginx or web.config files.

[gist id=3140348 file=.htaccess]

Read more about filename cache busting at the Html5 Boilerplate Wiki entry on cache busting.
