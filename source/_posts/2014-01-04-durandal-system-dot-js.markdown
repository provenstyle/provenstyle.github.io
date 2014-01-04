---
layout: post
title: "Durandal - system.js"
date: 2014-01-04 12:36
comments: true
categories: 
---

The Durandal core is made up of 8 modules.  The system.js module provides core functionality to Durandal and it is also useful for any custom modules we write in our own apps.   

### Features
* Logging
* Debug mode
* Object Type Testing
* noop function
* console.log polyfill
* Object.keys polyfill
* gets and sets the \_\_moduleId\_\_ of an object
* Creates seam to use your preferred promise implementation

### Architecture
* AMD Module
* system is an object literal. There is only one instance.
* Lines: 427
* Documentation is done with [YUIDoc - Javascript Documentation Tool](https://github.com/yui/yuidoc)


### Dependencies
* requireJs
* jQuery
* No other durandal dependencies
* Every module in durandal depends on system


## Debug mode

	system.debug(true);	

* turns debug mode on
* logs to the console and throw errors

Debug is off by default

	system.debug(false);

* turns debug mode off        
* no logging and no error throwing
* uses the noop function until turned on

## Interesting uses of JavaScript

####Reflection

	Object.keys(obj);

    Returns an array of strings representing all the enumerable property names of the object


####Make shortcuts to native JavaScript functions     

     var nativeKeys = Object.keys,
	     hasOwnProperty = Object.prototype.hasOwnProperty,
	     toString = Object.prototype.toString,
	     nativeIsArray = Array.isArray,
	     slice = Array.prototype.slice;

####The noop function (No Operation)
     var noop = function() { };

     Define a function that does nothing and set it as a default 
     to be replaced later during configuration if desired.

#### Arguments and parameters

You can refer to a functions parameters by name or by using the arguments array

Test the number of arguments and take different actions. 

	if (arguments.length == 1) {};

Refer to arguments by index

	var first = arguments[0];		

Accept n number of arguments in a function. 

	function(obj) {			
		
		//do something with obj

	    var rest = slice.call(arguments, 1);	
	    for (var i = 0; i < rest.length; i++) {
	        var source = rest[i];
	
	        if (source) {
	            //do something with the rest of the arguments
	        }
	    }
	}



## Public Api
[Official system.js documentation is here >>](http://durandaljs.com/documentation/api/#module/system) 

#### My summary of the api
	version - durandal version
	noop - no operation function
	getModuleId - gets \_\_moduleId\_\_
	setModuleId - sets \_\_moduleId\_\_
	resolveObject - returns either the object or news it up from a function
	debug - turns debug mode on and off
	log - logs to console. noop by default
	error - noop by default
	assert - throws and error only in debug mode
	defer - wraps the jquery defer.  Lets you replace it with a different implementation
	guid - simple guid
	acquire - returns modules from requireJs
	extend - copies properties from n number of functions onto an object and returns the extended object
	wait - waits specified number of seconds and returns a promise
	keys- polyfills the getKeys behavior

###is Functions to test object types

	isElement
	isArray
	isObject
	isBoolean
	isPromise
	isArguments
	isFunction
	isString
	isNumber
	isDate
	isRegExp


