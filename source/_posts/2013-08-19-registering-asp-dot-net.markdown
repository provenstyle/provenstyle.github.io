---
layout: post
title: "Registering ASP.NET in IIS"
date: 2013-08-19 16:32
comments: true
categories: 
---

Every time I have set up a new development evnironment in the last three years I have forgotten to register Asp.net with IIS.  When I was first learning it would derail me for for a quite a while.  I have done it so many times now that I recognize what is going on when I see the errors, but it is still irritating.  What was that command again?  I am putting it here so I never have to google it again!

	aspnet_regiis

"Not a recognized command!", you say?
Yes, you have to change directories into the .net version that you are using.

	cd C:\Windows\Microsoft.NET\Framework\v4.0.30319

Now run the command.


What? That didn't do it either?  Don't forget to add the install flag.

	aspnet_regiis -i

Still not working?  Don't forget to start your command prompt as admin, and yes now you have to cd directories all over again into your .net version.  

