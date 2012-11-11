--- 
wordpress_id: 406
title: SVN Remove & Ignore Causing Tree Conflict
wordpress_url: http://www.bwigg.com/?p=406
date: 2012-09-10 12:27:35 -06:00
layout: post
---
We has some files that were being tracked in SVN that we needed to be untracked and ignored, specifically a web.config file, which was being dynamically generated. We removed and ignored the file in a feature branch. We then merged the feature branch into trunk. Then ran svn update on our servers, which caused a tree conflict.

After some googling, the resolution was the following command on the server:
<pre>svn resolve --accept working -R .</pre>
Thanks to http://www.learnaholic.me/2009/10/04/subversion-resolve-for-tree-conflict/ for the answer.
