--- 
wordpress_id: 110
layout: post
title: "Subversion: Continuous Integration with a PHP Application"
date: 2009-10-27 15:13:32 -06:00
wordpress_url: http://www.bwigg.com/?p=110
---
<strong>Abstract</strong>

This tutorial explains how to update a staging site whenever there is a commit to the SVN repository. This assumes basic knowledge of Subversion, Apache and C and that the staging site is running a working copy checked out from the repository.

<strong>Problem &amp; Solution
</strong>

When working close with designers or other non-coders it's often most productive to be able to make quick changes live to the box serving up the dev site, then to commit a change then update the server over and over. The bigger issue is that there is now modified code on the dev site that's not revisioned with subversion. To solve this problem we are going to update the codebase for the development site whenever anyone commits changes to the subversion repository.

<strong>update_svn</strong>

update_svn is a C program that will make a call to the svn command with our designated username and password and update the working copy. This program should live inside your applications directory.

<em>update.c</em>

<pre lang="ruby">
#include <stddef.h>
#include <stdlib.h>
#include <unistd.h>
int main(void)
{
     execl("/usr/bin/svn", "svn", "update", "--username",
     "USERNAME", "--password", "PASSWORD",
     "/var/www/website/",  (const char *) NULL);
     return(EXIT_FAILURE);
}</pre>

Compile and add permissions

<code>gcc svn_update.c -o /var/www/website/update_svn
chmod +s update_svn</code>

<strong>Subversion post-commit hook</strong>

In our subversion repository's post-commit script we want to add a call to update_svn. Add the following line to your post-commit script:

<code>/var/www/website/update_svn</code>

Resources:
<ul>
	<li><a title="SVN Post Commit script to update php code" href="http://dtbaker.com.au/random-bits/svn-post-commit-script-to-update-php-code.html">David Baker: SVN Post Commit script to update php code</a></li>
</ul>
