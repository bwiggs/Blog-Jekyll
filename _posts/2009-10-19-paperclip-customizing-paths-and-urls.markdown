--- 
wordpress_id: 64
layout: post
title: Paperclip - Customizing Paths and URLs
date: 2009-10-19 19:08:35 -06:00
wordpress_url: http://www.bwigg.com/?p=64
---
To customize the paths and urls of paperclip objects in your rails app you need to modify both the :path and :url options for  has_attached_file in your models. Here's an example...
<code> </code>

<code>
<pre lang="ruby">class SomeModel < ActiveRecord::Base

  has_attached_file :image_one,
    :path => "public/system/:class/:id/:filename",
    :url => "/system/:class/:id/:basename.:extension"

  has_attached_file :image_two,
    :path => "public/system/:class/:id/:filename",
    :url => "/system/:class/:id/:basename.:extension"
end</pre>
</code>
By default Paperclip will store your files in <code>/system/:attachment/:id/:style/:filename</code>. By passing the <code>:path</code> and <code>:url</code> options to the has_attached_file method in your model you can change where the uploaded files will be stored as well as where they can be accessed. The <code>:path</code> is the directory in your application where your files will be stored. The <code>:url</code> is the url that users will use to render the image.

Paperclip Resources
<ul>
	<li>Paperclip RDoc - <a title="Paperclip RDoc" href="http://dev.thoughtbot.com/paperclip/" target="_blank">http://dev.thoughtbot.com/paperclip/</a></li>
	<li>Paperclip Railscasts - <a title="Paperclip Railscasts" href="http://railscasts.com/episodes/134-paperclip">http://railscasts.com/episodes/134-paperclip</a></li>
	<li>Jim Neath - Paperclip: Attaching Files in Rails - <a title="Paperclip: Attaching Files in Rails" href="http://jimneath.org/2008/04/17/paperclip-attaching-files-in-rails/">http://jimneath.org/2008/04/17/paperclip-attaching-files-in-rails/</a></li>
</ul>
