--- 
wordpress_id: 23
layout: post
title: "Ruby GUI Applications and Empty Console Window"
date: 2008-08-06 16:53:45 -06:00
wordpress_url: http://www.bwigg.com/?p=23
---
While writing a Ruby GUI app I kept seeing an empty console window. As it turns out there are two ruby executables that you can use to run your Ruby scripts: ruby, which is a CUI (Console User Interface) and rubyw, a GUI (Graphical User Interface) used to launch GUI Apps. Using rubyw will prevent that empty console from showing up. Hope this helps someone!<br /><br /># rubyw myapp.rb<br /><br />Edit - 8/7/08<br /><br />There are actually two filetypes you can use for your ruby scripts. .rb files will run with the ruby CLI interpreter, while .rbw scripts will run with the GUI interpreter. Remember though, with no console window you wont get any status messages or debug messages. While writing and testing your GUI apps use the ruby interpreter then when your finished change the filetype to .rbw.<br />
