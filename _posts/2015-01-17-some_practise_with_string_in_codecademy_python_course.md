---
layout: post
title: "diveintopython notes chapter 4"
description: ""
category:[diveintopython, chapter 4]
tags: [python]
---
{% include JB/setup %}

## some practise with string in codecademy python course

###reverse string without using reversed or [::-1]

there are two ways, one is using range from last one to first one:
	
	def reverse(text):
		return "".join(text[i] for i \
		in range(len(text)-1, -1, -1))
	
another way is do with recursion	:
	
	def reverse(text):
		# base case
		if len(text) <= 1:
			return text
		
		return reverse(text[1:]) + text[0]
		
simple example for the string "hello":
	
	reverse(hello)
	=reverse(ello) + h
	=reverse(llo)+ e + h
	=reverse(lo) + l + e + h
	=reverse(o) + l + l + e + h
	= o + l + l + e + h
	= olleh

###censor

	censor("this hack is wack hack", "hack")
	#should return 
	"this **** is wack ****"

	#string.split()
	#" ".join(list)
	
	
	def censor(text,word):
    lst = text.split()
    for i in range(len(lst)):
        if lst[i] == word:
            lst[i] = len(word)*'*'
    text = " ".join(lst)
    return text