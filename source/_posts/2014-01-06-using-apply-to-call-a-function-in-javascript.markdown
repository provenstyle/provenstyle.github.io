---
layout: post
title: "Using apply in JavaScript"
date: 2014-01-06 18:33
comments: true
categories: javascript, jquery
---

Today I went a little deeper into JavaScript! The problem I was facing was: 

>"How do I call a function and provide the arguments as an array instead of a comma separated list?" 

My arguments were not know until runtime and there could be 0 to 4 arguments.


##Function.prototype.apply() to the rescue

The apply() method lets you invoke a function with arguments as an array.

Here I am making a service call to stopStreaming for every vehicle in my array.  This service call returns a promise that I push into an array.  I can then use `$.when.apply(this, promises).always()` to wait for *all* the calls to return before I resolve my deferred.


Here is the full code.

	vm.stopAll = function () {
      var deferred = $.Deferred();
      var promises = [];
      for (var i = 0; i < streamingVehicles.selectedVehicles.length; i++) {
         var vehicle = streamingVehicles.selectedVehicles[i];
         var promise = stopStreaming(vehicle);
         promises.push(promise);
      }

      $.when.apply(this, promises).always(function() {
         deferred.resolve();
      });

      return deferred.promise();
	};

Incidentally, this is also a nice demonstration of using the promise pattern and the jQuery Deferred object. 