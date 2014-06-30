---
layout: post
title: "FFMPEG Where Is The Documentation?"
date: 2014-06-30 13:00
comments: true
categories: FFMPEG
---

It seems like the version of FFMPEG that I am using on projects is never that same version covered in the documentation on the website.  You can get version specific documentation with FFMPEG from the console with the -h argument.

`ffmpeg -h`

You can dump the ffmpeg documentation to a text file for easier reading and searching by redirecting the output. 
	
`ffmpeg -h > c:\temp\ffmpeg.txt`

You can also view a list of the supported codecs for your version of FFMPEG
`ffmpeg -codec > c:\temp\supportedCodecs.txt`