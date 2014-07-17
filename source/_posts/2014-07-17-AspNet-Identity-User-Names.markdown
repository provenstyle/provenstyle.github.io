---
layout: post
title: "Asp.net Identity User Names"
date: 2014-07-17 13:00
comments: true
categories: Asp.net
---

I've been working on the web site for the [2014 Dallas Tech Fest](https://DallasTechFest.com "Dallas Tech Fest") and it has been such a fun project! A huge part of that is because of [Brandon Ward's ](https://twitter.com/uxward)amazing designs.

One unexpected challange was using twitter for the authorization.  After 30 people had already signed up on the site with no issue, there was one user that was stuck in an authentication loop.  Everytime he authenticated with twitter, he was asked to log in again.

It turns out he was the first user on the site, that I know of, to have a user name starting with an underscore.  By default Asp.net Identity will only accept alphanumeric user names, and I was using his twitter username as the username for the site.  You would have the same issue if you tried to allow an email as the username.

To fix it, I went into the constructor for the Account controller and created a new UserValidator.
 
	public AccountController(UserManager<ApplicationUser> userManager)
    {
        UserManager = userManager;
        UserManager.UserValidator = 
            new UserValidator<ApplicationUser>(UserManager)
                {AllowOnlyAlphanumericUserNames = false};
    }

