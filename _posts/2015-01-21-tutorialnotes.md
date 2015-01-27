---
title: tutorialnotes
date: 2015-01-21 12:12:00
layout: post
category: [python, tutorial]
tags: [python]
summary: 
---
####*for* loops
using a *for* loop, you can print out each individual character in a string.
	
	word = "eggs!"
	for char in word:
		print char,
the , character after our print statement means that our next print statement keeps printing on the same line.

a weakness of using for-each style of iteration is that you don't know the index of the thing you're looking at.
*enumerate* works by supplying a corresponding index to each element in the list that you passit.
	
	choices = ['pizza', 'pasta', 'salad', 'nachos']
	for index, item in enumerate(choices):
		print index, item

It's also common to need to iterate over two lists at once.*zip* will creat pairs of elements when passed two lists, and will stop at the end of the shorter list.
	
	list_a = [1, 3, 5, 7]
	list_b = [2, 4, 6, 8, 9]
	print zip(list_a, list_b)
	
	
####list slicing

	[start:end:stride]
*start* inclusive

*end* exclusive

*stride* space between items in the sliced list

a negative stride progresses through the list from right to left.

	[::-1] # revesing a list

####anonymous functions
	
	evenly_divide =filter(lambda x: x % 2 == 0, range(100))
	
	# Filter elements greater than 4
	a = [3, 4, 5]
	b = [i for i in a if i > 4]
	# or
	b = filter(lambda x:x>4, a)
	
	# Add three to all list members
	
	
####introducing *super()*

sometimes you'll be working with a derived class and realize that you've overwritten a method or attribute defined in that class's base class that you actually need.
You can directly access the attributes or method of a superclass with Python's built-in *super* call.

	class Derived(Base):
		def m(self):
			return super(Derived, self).m()
where m() is a method from the base class.

####using *super()* with *__init__*

	class Child(Parent):
		
		def __init__(self,stuff):
			self.stuff = stuff
			super(Child, self).__init__()

