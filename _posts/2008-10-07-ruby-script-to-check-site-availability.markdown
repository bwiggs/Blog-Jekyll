--- 
wordpress_id: 39
layout: post
title: Ruby Script to Check Site Availability
date: 2008-10-07 17:18:34 -06:00
wordpress_url: http://www.bwigg.com/?p=39
---
Recently, I've been looking around for scripts to check the availability of websites I'm hosting, then email me a status message if there is anything I should be worrying about. There are many services out there, but none are free. Fortunately this kind of thing can be done with a little ruby script and cron job.

Requirements
<ul>
	<li>SMTP server.</li>
	<li>Ruby 1.8.6 - This script uses core classes of Ruby and doesn't require any rubygems</li>
	<li>Ability to add cron jobs to your machine</li>
</ul>
<strong>Plan</strong>

Now, let's think out some solutions for how we can check on a site's availability. We could ping the server, but that only checks that the box is up and running. It doesn't necessarily mean that our HTTP Service is serving up pages correctly. We could use curl or wget to pull down the default page being served, but what if it's a holding space page, or a 404 error? This method would basically only tell us if the webserver is running or not, same as the ping method. Alternatively, we could test the to see what the response code of the page is. This way we can also find out what type of issue we are having on the server.

I also want this script to be a little more robust. Instead of having a hardcoded URL in my script,  I want it to pull a list of urls from a txt file. I also want each url report to be emailed to different email addresses since some of these sites are for work and some are personal.

<strong>Checking the Response Codes</strong>

There are lots of different response codes that we could receive from the server. The most familiar for anyone who's ever browsed the net is the infamous 404 Page Not Found error. Let's quickly take a look at what this error code means. Wikipedia states a 4** message means that "the request contains bad syntax or cannot be fulfilled.", in english, something went wrong on the web surfers side. This can be because of a mistyped URL or the result of a clicked link that took us to a page that's no longer available or has been moved. Now, the second two numbers tell us more specifically what kind of issue we've found, in our case *04, which Wikipedia explains as "The requested resource could not be found but may be available again in the future. Subsequent requests by the client are permissible." or in other words, it's lost, maybe only temporarily, but we can keep trying if we want.

Other response code groups include the following:
<ul>
	<li class="toclevel-1"><a href="http://en.wikipedia.org/wiki/List_of_HTTP_status_codes#1xx_Informational"><span class="tocnumber">1</span> <span class="toctext">1xx Informational</span></a></li>
	<li class="toclevel-1"><a href="http://en.wikipedia.org/wiki/List_of_HTTP_status_codes#2xx_Success"><span class="tocnumber">2</span> <span class="toctext">2xx Success</span></a></li>
	<li class="toclevel-1"><a href="http://en.wikipedia.org/wiki/List_of_HTTP_status_codes#3xx_Redirection"><span class="tocnumber">3</span> <span class="toctext">3xx Redirection</span></a></li>
	<li class="toclevel-1"><a href="http://en.wikipedia.org/wiki/List_of_HTTP_status_codes#4xx_Client_Error"><span class="tocnumber">4</span> <span class="toctext">4xx Client Error</span></a></li>
	<li class="toclevel-1"><a href="http://en.wikipedia.org/wiki/List_of_HTTP_status_codes#5xx_Server_Error"><span class="tocnumber">5</span> <span class="toctext">5xx Server Error</span></a></li>
</ul>
I'm not really interested in being notified of anything that starts with a 2**, since that means the site is up and functioning correctly. 3** are redirect codes, these might be useful, but I don't really care about them. I should know if one of my sites is being redirected because I would have had to set it up that way. I am only going to check if the response code starts with a 2xx or 3xx, if it does then were fine and I dont need to be notified. If it doesn't, then there's an issue and I need to know about it.

To check the reponse code of the page, Ruby has a class in it's core called Net:HTTPResponse, which will go out and touch a URL and return the reponse code the server supplied.

<strong>Sending the Email</strong>

This script uses the Ruby class Net::SMTP, which allows communication with an SMTP server. On my box I have postfix setup, which runs on port 25. When I send the email I want to be notified which url went down and what the response code is. I also added the HTTP version in there just in case.

<strong>Automatically Running the Script</strong>

To get this script to run automatically, I'm going to need to setup a cron job. To create a cron job we need to add a line to our crontab file, which can be found at /etc/crontab.
<pre lang="bash"># [minutes] [hours] [day of month] [month] [day of week] [user] [command]
0,15,30,45 * * * * bawigga ~/scripts/check_uptime.rb</pre>
Above we can see that I set the script to run on the 0th, 15th, 30th, and 45th minute, of any (*) hour of any day of the month, of any month, of any day of the week, under the username bawigga, and we want to execute the check_uptime.rb script in my home/scripts directory.

<strong>The Script</strong>
<pre lang="ruby">#!/usr/bin/ruby

require 'net/http'
require 'net/smtp'

# Brian Wigginton
# http://www.bwigg.com/2008/10/ruby-script-to-check-site-availability/
# 10/7/2008
#
# Check's availabilty of a website. Needs to be run via a cron job.
# Example cron job line to be placed in crontab
#
# 0,15,30,45 * * * * username ~/scripts/check_uptime.rb
#
# This script uses a txt file to look for urls and email addresses.
# This text file needs to be in the following format
# -- lines with beginning with # signs will be ignored
# -- first thing should be the url
# -- then a space
# -- then email addresses seperated by commas, no white space.
# EXAMPLE
#       example.com admin@axample.com,bob@example.com

File.open("/home/user/scripts/sites.txt").each { |line|
	# get rid of CRLF
	line.chomp!

	next if(line[0..0] == '#' || line.empty?)

	url, emails = line.split(' ')
	emails = emails.split(",")

	# check if http:// was in the url if not add it in there
	url.insert(0, "http://") unless(url.match(/^http\:\/\//))

	# Get the HTTP_RESPONSE from the site we are checking
	res = Net::HTTP.get_response(URI.parse(url.to_s))

	# Check the response code and send an email if the code is bad
	unless(res.code =~ /2|3\d{2}/ ) then
		from = "admin@example.com"
		message = "From: admin@example.com\nSubject: #{url} Unavailable\n\n#{url} - #{res.code} - #{res.message}\nHTTP Version - #{res.http_version}\n\n"
		begin
			Net::SMTP.start('localhost',25 , 'example.com') do |smtp|
			smtp.send_message(message, from, emails)
		end
		rescue Exception => e
			print "Exception occured: " + e
		end
	end
}</pre>
