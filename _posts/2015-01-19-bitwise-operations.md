---
title: bitwise operations
date: 2015-01-19 16:16:18
layout: post
category: [python, tutorial]
tags: python
summary: 
---

###The bin() Func

use the bin() function can return the binary representation of that integer.

	bin(2) # 0b10
	
keep in mind that after using the *bin()* function, you can no longer operate on the value like a number.

###int()'s Second Parameter

*int()* function actually has an optional second parameter.

	int("110", 2) # 6

when given a string containing a number and the base that number is in , the function will return the value of that number converted to base ten.

###bit mask

* turn it on
		
		a = 0b11 
		mask = 0b1
		a | mask # 0b111
		
* flip out

		a = 0b110
		mask = 0b111
		a ^ mask # 0b1
		
* slip and slide

		a = 0b101
		mask = (0b1 << 9) # tenth bit mask
		a ^ mask 


