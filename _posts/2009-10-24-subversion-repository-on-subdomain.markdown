--- 
wordpress_id: 93
layout: post
title: "Subversion: Repository on Subdomain"
excerpt: Tutorial explaining how to setup a Subversion repository on a subdomain using Apache2.
date: 2009-10-24 14:54:00 -06:00
wordpress_url: http://www.bwigg.com/?p=93
---
This is a tutorial on how to setup a Subversion repository on a subdomain with Apache. This assumes you have Subversion and Apache already installed on your system.

<strong>Subversion Setup</strong>

First you need to create a repository somewhere in your file system. Then grant apache permissions on that directory.

<code>svnadmin create /var/svn/repository
sudo chown -R www-data:www-data /var/svn/repository</code>

<strong>Controlling Access</strong>

Access to the repo via the web will be controlled by an htpasswd file located at /<code>var/svn/svn-auth-file</code>. Use the <code>htpasswd</code> command to create the file.

<code>htpasswd -c /var/svn/svn-auth-file &lt;username&gt;</code>

Execute the script again without the -c argument to add more people to the list.

<code>htpasswd /var/svn/svn-auth-file &lt;username_two&gt;</code>

<strong>Apache Setup</strong>

I usually setup Apache to use Names VirtualHosts to handle multiple websites. We'll make a new named virtualhost for subversion repository.

<code>&lt;VirtualHost *:80&gt;
ServerName svn.your_domain.com
&lt;location /&gt;
DAV svn
SVNPath /var/svn/repository</code>

<code> AuthType Basic
AuthName "Subversion repository"
AuthUserFile /var/svn/svn-auth-file
Require valid-user
&lt;/location&gt;
&lt;/VirtualHost&gt;</code>
