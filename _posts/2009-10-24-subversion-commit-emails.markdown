--- 
wordpress_id: 89
layout: post
title: "Subversion: Commit Emails"
date: 2009-10-24 12:29:46 -06:00
wordpress_url: http://www.bwigg.com/?p=89
---
This is a tutorial on how to setup Subversion to email a team when there is a commit to the repository.

<strong>Subversion Hooks</strong>

Hooks are what Subversion executes upon certain events. Within your SVN directory you should see the following items
<ul>
	<li>README.txt</li>
	<li>conf</li>
	<li>dav</li>
	<li>db</li>
	<li>format</li>
	<li><strong>hooks</strong></li>
	<li>locks</li>
</ul>
Go into the hooks directory and you will see a bunch of files ending in .tmpl. These are template scripts that are prebaked for you. Within each hook file are calls to scripts you want to be executed. To get one to execute you need to remove the .tmpl extension and then make it executable.

<code>mv post-commit.tmpl post-commit
chmod +x post-commit</code>

<strong>The Post Commit Hook &amp; commit-email.pl</strong>

In post-commit there is a call to <code>commit-email.pl</code>. (Be sure that this calls to the absolute path of the script)

I didn't have <code>commit-email.pl</code> on my server but acquired it from the Subversion Tools Repository <a title="Subversion commit-email.pl" href="http://svn.collab.net/repos/svn/trunk/contrib/hook-scripts/commit-email.pl.in" target="_blank">here</a>.Save that file to your hooks directory. Also, be sure to edit that file and rename all the <code>@SVN_BINDIR@</code> instances to the directory of you subversion executable. To find out type <code>which svn</code> and use the path that returns. Mine returned <code>/usr/bin/svn</code> to I used <code>/usr/bin/</code> for @SVN_BINDIR@.

Run commit-email.pl without any arguments to see a list of options. To test this script point it to a repository and a revision number.

<code>./commit-email.pl /var/svn/repository 1 --from "Subversion Gate Keeper &lt;subversion@example.com&gt;" youremail@example.com anotheremail@example.com</code>

I like to prepend a subject line prefix so I can filter it a little easier when it comes through to my email client. Adding the <code>-s</code> argument to the command will allow you to specify a subject line prefix.

<code>./commit-email.pl /var/svn/repository 1 --from "Subversion Gate Keeper &lt;subversion@example.com&gt;" -s "Commit Activity: " youremail@example.com anotheremail@example.com</code>

Here are some more resources to help you out.
<ul>
	<li><a title="Subversion Tools" href="http://subversion.tigris.org/tools_contrib.html" target="_blank">Subversion Tools</a></li>
	<li><a title="Pete Freitag: Using Subversion Hooks to send out build emails" href="http://www.petefreitag.com/item/244.cfm" target="_blank">Pete Freitag: Using Subversion Hooks to send out build emails</a></li>
</ul>
<strong>Edit - October, 27:</strong> You have to call your scripts within post-commit by their absolute path or else they wont run. Code above has been modified.
