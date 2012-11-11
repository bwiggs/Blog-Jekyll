--- 
wordpress_id: 420
title: Jenkins & JSHint - Integrating with Checkstyles and Violations
wordpress_url: http://www.bwigg.com/?p=420
date: 2012-10-04 19:41:40 -06:00
layout: post
---
<h2><strong>Prerequisite</strong></h2>
You need to have some form of jshint installed on your server. Instructions of how to do that are out of scope for this post, and should be able to be easily found on the internet. I used the version that installed with npm.
<h2>Jenkins Build Tasks</h2>
This is a very simple setup that uses 2 Build &gt; Execute Shell tasks. In the first shell task I created the checkstyle output:
<pre>jshint --checkstyle-reporter app/webroot/javascripts/mdc/ &gt; build/logs/checkstyle-jshint.xml || exit 0</pre>
In the second, I created the jslint output:
<pre>jshint --jslint-reporter app/webroot/javascripts/mdc/ &gt; build/logs/jslint.xml || exit 0</pre>
<img class="wp-image-422 alignnone" title="Build Task Settings" src="http://www.bwigg.com/wp-content/uploads/2012/10/Screen-Shot-2012-10-04-at-7.28.24-PM.png" alt="Build Task Settings" width="572" height="340" />
<h2>Checkstyle Plugin</h2>
Configure the checkstyles plugin to reference the created checkstyle-jshint.xml file:

<a href="http://www.bwigg.com/wp-content/uploads/2012/10/Screen-Shot-2012-10-04-at-7.37.12-PM.png"><img class="wp-image-423 alignnone" title="Checkstyle Settings" src="http://www.bwigg.com/wp-content/uploads/2012/10/Screen-Shot-2012-10-04-at-7.37.12-PM.png" alt="Checkstyle Settings" width="580" height="111" /></a>
<h2>Violations Plugin</h2>
Configure the violations plugin to reference the created checkstyle-jshint.xml and the jslint.xml file:

<a href="http://www.bwigg.com/wp-content/uploads/2012/10/Screen-Shot-2012-10-04-at-7.38.20-PM.png"><img class="wp-image-424 alignnone" title="Violations Setup" src="http://www.bwigg.com/wp-content/uploads/2012/10/Screen-Shot-2012-10-04-at-7.38.20-PM.png" alt="Violations Setup" width="567" height="223" /></a>
