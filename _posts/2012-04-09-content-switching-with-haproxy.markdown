--- 
wordpress_id: 339
layout: post
title: Content Switching with HAProxy
date: 2012-04-09 10:08:07 -06:00
wordpress_url: http://www.bwigg.com/?p=339
---

Content-switching is an [OSI layer 7](http://en.wikipedia.org/wiki/OSI_model#Layer_7:_application_layer) application switching technique, which in laman's terms means that it has access to the HTTP requests and responses of your application. By inspecting each request and response, it make decision on where to send those requests and how to handle responses.

[HAProxy](http://haproxy.1wt.eu/) is an open source load balancing tool that also has the ability to implement content switching.

## Proxies & Reverse Proxies

You might be familiar with a Proxy, which is basically something that sits between the client and the server. When the client makes a request it first goes through the proxy then to the server. The response from the server then goes back through the proxy  to the client. These are usually implemented by the client and completely transparent to the server. A great example of this is when someone configures their browser to use a proxy to access the internet, with a tool such as Tor.

A Reverse Proxy is the same thing except it acts on behalf of the server. One example of a Reverse Proxy is a load balancer, which receives requests from a users and usually proxies them to one of several backend servers. This process is completely transparent to the client. Content switching is also a form of Reverse Proxy.

## Content-Switching Scenario

Let's say you're in charge of a large social bookmarking application, `http://delicioso.com`. The shelf life for that system has come to and end and you're team has decided to rewrite everything in Rails. It's going to take a long time to migrate everything over to the new application, and you want to reduce the risk involved with a huge release cycle. Instead you want to take an agile approach and roll out bits and pieces over time in the following order:

1. Configure HAProxy as a Reverse Proxy
2. Phase in user profiles - http://delicioso.com/profile/...

For the purpose if this example, let's call the current application *Legacy*. Image your current infrastructure is setup like the following:

<pre>
                        +-------------------+
                        |    example.com    |
  User  +----------&gt;    |-------------------|
                        |     legacy:80     |
                        +-------------------+
</pre>

### Step 1: Configure HAProxy as a Reverse Proxy

The first step is to setup HAProxy to be reverse proxy for *Legacy*, so later we can implement content-switching. You'll need to have HAProxy installed.

<pre>
                      +-------------------+
                      |    example.com    |
User  +----------&gt;    |-------------------|
                      |    HAProxy:80     |
                      |     legacy:8001   |
                      +-------------------+
</pre>

Here's the config file to configure HAProxy in this way:

<pre>
global
	#debug                                   # uncomment to enable debug mode for HAProxy

defaults
	mode http                                # enable http mode which gives of layer 7 filtering
	timeout connect 5000ms                   # max time to wait for a connection attempt
																					 #  to a server to succeed
	timeout client 50000ms                   # max inactivity time on the client side
	timeout server 50000ms                   # max inactivity time on the server side

backend legacy                             # define a group of backend servers
	server legacy_server 127.0.0.1:8001      # add a server to this backend

frontend app *:80                          # define what port to listed to for HAProxy
	default_backend legacy                   # set the default server for all request
</pre>

### Step 2: Phase in User Profiles
All of your user's profile pages have a URL format as follows: /profile/username. Your team finished implementing this in the new Rails framework and it's time to roll it out. The rails app is running on port 8002.
<pre>
                      +-------------------+
                      |    example.com    |
User  +----------&gt;    |-------------------|
                      |    HAProxy:80     |
                      |     legacy:8001   |
                      |      rails:8002   |
                      +-------------------+
</pre>

Here's the new config file for content-switching all requests starting with /profile to the new rails application

<pre>
global
  #debug

defaults
  mode http
  option httpclose                         # close the tcp connection after every request
  timeout connect 5000ms
  timeout client 50000ms
  timeout server 50000ms

backend legacy
  server legacy_server 127.0.0.1:8001

backend rails                              # define a group of backend servers
  server rails_server 127.0.0.1:8002       # add a server to this backend

frontend app *:80
  default_backend legacy
  acl rails_path path_beg /profile         # acl rule for rails supported paths
  use_backend rails if rails_path          # use rails if the rules match.
</pre>
Check out the HAProxy documentation for more keywords your can use to route requests. This was a pretty simple example, this get's more challenging when you need to handle session state across the legacy and new systems.

{{ tags }}
