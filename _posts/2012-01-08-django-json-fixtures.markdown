--- 
wordpress_id: 318
layout: post
title: "Django Fixtures: JSON Formatting"
date: 2012-01-08 22:25:41 -07:00
wordpress_url: http://www.bwigg.com/?p=318
---
Came across an issue with Django Fixtures using the JSON format. Here was my fixture:

[gist id=d71de9702a353ebe37b7 file=initial_data_broken.json]

Here's the error I was getting:

[gist id=d71de9702a353ebe37b7 file=error.txt]

The fix was simple, wrap the object in an array.

[gist id=d71de9702a353ebe37b7 file=initial_data_fixed.json]

Found the fix thanks to the following StackOverflow post: <a title="Type error trying to load fixtures with Content_type natural keys in Django" href="http://stackoverflow.com/a/4939147" target="_blank">http://stackoverflow.com/a/4939147</a>
