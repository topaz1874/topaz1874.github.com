---
layout: post
title: "diveintopython notes chapter 5"
description: ""
category: [diveintopython, chapter 5]
tags: [python]
---
{% include JB/setup %}
###Importing Modules Using from *module* *import*

	from UserDict import UserDict
	from UserDict import *

### Defining Classes

	class Loaf:
		pass

####Initializing and Coding Classes
	class FileInfo(UserDict):
		"store file metadata"
		def __init__(self, filename=None):
			UserDict.__init__(self)
			self['name'] = filename


	class FileInfo(Dict):
		"store file metadata"
		def __init__(self, filename=None):
			Super(FileInfo, self)__init__()
			self['name']= filename

####Knowing When to Use *self* and *__init__*
When you call a method of an ancestor class from whithin your class, you *must* include *self* argument.
But when you call your class method from outside, you do not specify anything for the *self* argument;

###Instantiating Classes
pass

####Garbage Collection
In general, there is no need to explicitly free instances,because they are freed automatically.

	def leakmem():
		f = fileinfo.FileInfo('/music/_singles/kairo.mp3')
		
	for i in range(100):
		leakmem()

No matter how many times you call the  *leakmem* function, it will never leak memory, because every time, Python will destroy newly created FileInfo class before returning from *leakmem*

###Exploring *UserDict*: A Wrapper Class
	class UserDict:
		def __init__(self, dict=None):
			self.data = {}
			if dict is not None: self.updat(dict)
---

	def clear(slef): self.data.clear()
	
	def copy(self):
		if self.__class__ is UserDict:
			return UserDict(self.data)
		import copy
		return copy.copy(self)
		
	def keys(self): return self.data.keys()
	def items(self): return self.data.items()
	def values(self): return self.data.values()
	
###Special Class Methods

####Getting and Setting Items

	def __getitem__(self, key): return self.data[key]
	
	f = fileinfo.FileInfo("/music/_singles/kairo.mp3")
	f.__getitem__("name")
	f["name"]
---

	def __setitem__(self, key, item):self.data[key] = item
	
	f.__setitem__("genre", 31)
	f.["genre"] = 32
---

	def __setitem__(self, key, item):
		if key == "name" and item:
			self.__parse(item)
		FileInfo.__setitem__(self, key, item)	

####Advanced Special Class Methods
	def __repr__(self): return repr(self.data)	

	def __cmp__(self, dit):
		if isinstance(dict, UserDict):
			return cmp(self.data, dict.data)
		else:
			return cmp(self.data, dict)
	def __len__(self): return len(self.data)
	def __delitem__(self, key): del self.data[key]
	
For class instances, define the __len__ method and code the length calculation yourself, and then call len(instance) and Python will call your __len__ special method for you.

###Introducing Class Attributes

	class MP3FileInfo(FileInfo):
		tagDataMap = {"title" : (3, 33, stripnulls),
			"artist" : ( 33 ,63, stripnulls),}
	
	import fileinfo
	fileinfo.MP3FileInfo
	fileinfo.MP3FileInfo.tagDataMap 
	# tagDataMap is a class attribute
	
In Python, only class attributes can be defined here;data attributes are defined in the __init__ method.
Class attributes can be changed.

	class counter:
		count = 0
		def __iniit__(self):
			self.__class__.count +=1
			
	couter.count # 0
	c = counter()
	c.count # 1
	d = counter()
	d.count # 2
	c.count # 2
	couter.count # 2
	
	
###Private Functions
>Private functions, which can't be called from outside their module

>Private class methods, which can't be called from outside their class

>Private attributes, which can't be accessed from outside their class.

If the name of a Python function, class method, or attribute stats with(but doesn't end with) two underscores, it's private;everything else is public.
	