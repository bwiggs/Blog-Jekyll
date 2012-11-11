--- 
wordpress_id: 430
title: npm jshint - command not found after install
wordpress_url: http://www.bwigg.com/?p=430
date: 2012-10-20 23:06:27 -06:00
layout: post
---
I was getting a new machine up and running and decided to play with zsh. Later that evening I was going to write some JavaScript and wanted to lint my code with the handy jshint tool. So I installed it with NPM:
<pre>$ npm install jshint -g</pre>
And was then greeted with a command not found error:
<pre>$ jshint
zsh: correct jslint to slit [nyae]? n
zsh: command not found: jslint</pre>
Turns out I needed to add the npm bin directory to my PATH, as it didn't get added there automatically since I installed zsh after node/npm.
<pre>export PATH="/usr/local/share/npm/bin:${PATH}"</pre>
