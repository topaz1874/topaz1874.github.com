---
layout: post
title: "diveintopython notes chapter 4"
description: ""
category:[diveintopython,chapter 4]
tags: [python]
---
{% include JB/setup %}

###Getting Object Reference

####The *type* Function

	type(1)
	li = []
	type(li)

####The *str* Fucntion
the str coerces data into a string

the dir returns a list of the attributes and methods of any of object

	li = []
	dir(li)
	
the callable fucntion takes any object and return True if the object can be called

	callable(string.join)
	
	prin string.join.__doc__

####The *getattr* Function

	li = ["larry", "curly"]
	li.pop
	getattr(li, "pop")
	getattr(li, "append")("moe")
***
	
###Filtering Lists

Here is the list filtering syntax:

[mapping-expression for element in source-list if filter-expression]

	[elem for elem in li fi li.count(elem) == 1]
---

###The Peculiar Nature of *and* and *or* 
	
	1 and a or b # "1" a ; "0" b
	0 and a or b # a must be true 
___
###Using *lambda* Function
	g = lambda x: x*2
	g(3)
	processFunc = coolapse and (lambda s:" ".join(s.split()) or (lambda s:s)

