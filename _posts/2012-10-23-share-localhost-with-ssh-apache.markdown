--- 
wordpress_id: 434
title: Share localhost with SSH & Apache
wordpress_url: http://www.bwigg.com/?p=434
date: 2012-10-23 01:11:34 -06:00
layout: post
---
There's been a lot of services released lately that aim to assist developers share their local development environments over the internet. localtunnel, showoff.io and pagekite come to mind, just to name a few. However, you can roll your own forwarding service if you have SSH access to a server running Apache fairly easily.

<h2>The Big Picture</h2>

First, setup a webserver to listen for connections to demo.example.com. When a user connects to the webserver, it will attempt to proxy their request to port 1337 (demo.example.com port 1337).

Second, a reverse proxy initiated with SSH on your localhost creates a connection to demo.example.com, and then has all traffic on demo.example.com port 1337 forwarded to localhost port 80.

<pre>User =&gt; demo.example.com:80 &lt;=&gt; demo.example.com:1337 &lt;=&gt; localhost:80</pre>

<h2>Requirements</h2>

<h3>Server</h3>

<ul>
<li>Apache Web Server</li>
<li>Apache mod_proxy module</li>
<li>OpenSSH</li>
</ul>

<h3>Client</h3>

<ul>
<li>Local web application</li>
<li>SSH client</li>
<li>SSH public/private keys to login to the remote server</li>
</ul>

<h3>Server Setup (Ubuntu)</h3>

<em>You should be able to login to your server over SSH using public/private key authentication. How to do that is out of scope for this post, however there are plenty of guides available online that explain in detail how to do this.</em>

<h3>Apache: Enable mod_proxy</h3>

$ sudo a2enmod proxy_http

<h3>Apache: Configure VirtualHost</h3>

Setup Apache to use a VirtualHost for URL you want to use for sharing your work:

<p>&lt;VirtualHost *:80&gt;
ServerName "demo.example.com"
ProxyPass / http://127.0.0.1:1337/
ProxyPassReverse / http://127.0.0.1:1337/
&lt;/VirtualHost&gt;</p>

The <a href="http://httpd.apache.org/docs/2.2/mod/core.html#servername"><code>ServerName</code></a> Directive tells Apache to listen for client requests for demo.example.com. <a href="http://httpd.apache.org/docs/2.2/mod/mod_proxy.html#proxypass"><code>ProxyPass</code></a> and <a href="http://httpd.apache.org/docs/2.2/mod/mod_proxy.html#proxypassreverse"><code>ProxyPassReverse</code></a> configure Apache to forward all requests to the local port 1337.

<h3>Apache: Restart to load VHost</h3>

$ sudo apachectl restart

<h2>Localhost Setup</h2>

<h3>Make sure you're running a local web application</h3>

This should be pretty obvious :) It doesn't matter what port it's running, you can configure that when you start the SSH reverse proxy.

<h3>Create an SSH Reverse Proxy</h3>

To actually enable the connection form your public web server to your localhost, you have to create a reverse proxy using SSH. The reverse proxy will create a connection from your localhost to the remote server (demo.example.com), then have all traffic from a designated remote port forwarded to a local port.

$ ssh -fNR 1337:localhost:80 demo.example.com

<ul>
<li><code>-f</code> puts ssh into the background. If you want to be able to easily kill the Proxy later, omit the -f and use <code>ctrl + c</code>.</li>
<li><code>-N</code> disables all output keeping things clean. If you omit this, you'll see SSH connect to your remote server.</li>
<li><code>-R 1337:localhost:80</code> Tells SSH to create a reverse proxy and that all traffic on the remote port 1337 should be forwarded to localhost's port 80.</li>
<li><code>demo.example.com</code> is the remote server you want SSH to connect to.</li>
</ul>

<em>You may need to change your localhost port depending on what port your local environment is running on. For example: if you're developing a rails application using the builtin webserver, you might need to use port 3000 instead of port 80.</em>

You should now be able to hit demo.example.com in your browser, and see your application load.
