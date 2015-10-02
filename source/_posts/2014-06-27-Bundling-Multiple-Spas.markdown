---
layout: post
title: "Durandal: Bundling Multiple Spas With Weyland"
date: 2014-06-27 13:00
comments: true
categories: ['durandal', 'javascript', 'weyland']
---
[Durandal.js: Get Started](http://bit.ly/DurandalBlog)

By default Weyland expects a configuration file called `weyland-config.js` This caused me problems when I added multiple SPA pages to my solution.  I wanted to create two different bundled files.  One for each SPA.

Its not in the documentation, but you can specify the config file as an optional parameter `--config`.  To make this work I also had to use the trick of appending `%~dp0` to the config file name to include the full path of the config file.  More information on `%~dp0` can be found by typing `for /?` into a command prompt and reading the help. If you don't do this weyland will not find your config file and continue to look for the default config.

In the end this worked for me.

	weyland build --config "%~dp0customConfigFile1.js"
	weyland build --config "%~dp0customConfigFile2.js"

##Important
I was also reminded the hard way that JavaScript is case sensitive.  This mean that node is also case sensitive.  So I spent several hours trying to figure out why my bundling was not excluding the files I told it to exclude.  It turns out I had a mismatch in the casing of a directory name.  On the file system its name was capitalized, and in the config file I had used lowercase.  It was very frustrating, but looking on the bright side, I was forced to learn how to debug node application using the console.

[Durandal.js: Get Started](http://bit.ly/DurandalBlog)