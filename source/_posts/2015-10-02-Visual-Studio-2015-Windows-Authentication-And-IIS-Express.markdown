---
layout: post
title: "Visual Studio 2015, Windows Authentication, And IIS Express"
date: 2015-10-02 13:00
comments: true
categories: ['Visual Studio 2015', 'Asp.net', 'IIS Express']
---

I finally upgraded my Visual Studio to 2015 and the transition has been pretty smooth!  However, today I had an issue that took me a little while to solve.  An Asp.net MVC web app that uses Windows Authentication, had been working great, but was suddenly gave me the following error:

	Access is denied.
	
	Description: An error occurred while accessing the resources required to serve this request. 
	The server may not be configured for access to the requested URL. 
	
	Error message 401.2.: Unauthorized: Logon failed due to server configuration.  
	Verify that you have permission to view this directory or page based on the credentials 
	you supplied and the authentication methods enabled on the Web server.  Contact the Web 
	server's administrator for additional assistance.

I learn several things trouble shooting this.  The first is that IIS Express configuration has moved from `C:\Users\YourUserName\Documents\IISExpress\config\applicationhost.config` out of the My Documents IISExpress folder and into the new `.vs/configuration` folder.

The second thing I was reminded of is that there is another place to edit properties for your project.  I almost never press f4 on my project.  I right click and go down to properties, but that is a very different set of properties than you get when you press f4.

The solution to my authorization issue was to go into the f4 project properties and set the following:

Anonymous Authentication - Disabled

Windows Authentication - Enabled

Apparently these properties update the IIS applicationHost.config directly.  It adds the following to the config.

	<location path="Project.Name.Here">
        <system.webServer>
            <security>
                <authentication>
                    <windowsAuthentication enabled="true" />
                    <anonymousAuthentication enabled="false" />
                </authentication>
            </security>
        </system.webServer>
    </location>

It is irritating that you can't save these somewhere in the .csproj file instead, but not a big deal once you know it is there.