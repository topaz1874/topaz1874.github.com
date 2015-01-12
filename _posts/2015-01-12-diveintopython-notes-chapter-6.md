---
layout: post
title: "diveintopython notes chapter 6"
description: ""
category: [diveintopython, chapter 6]
tags: [python]
---
{% include JB/setup %}
###Handling Exceptions

Python use *try...except* to handle exceptions and *raise* to generate them.

Accessing a noe-existent dictionary key will raise a *KeyError* exception.

Searching a list for a non-existent value will raise a *ValueError* exception.

Calling a non-existent method will raise an *AttributeError* exception.

Referencing a non-existent variable will raise a *NameError* exception.

Mixing datatypes without coercion will raise a *TypeError* exception.

If you know a line of code may raise an exception, you should handle the exception using a *try...except* block.

	fsock = open("/notthere", "r")
	
	try:
		fsock = open("/notthere")
	except IOError:
		print "The file doesn't exist, exiting gracefully"
	print "This line will always print"

####Using Exceptions For Other Purposes
Importing a module that does not exist will raise an ImportError exception. You can use this to define multiple levels of functionality based on which modules are available at run-time.

You can also define your own exceptions by creating a class that inherits from the built-in Exception class, and raise your exceptions with the *raise* command.

	# Bind the name getpass to the appropriate function
	try:
		import termios, TERMIOS
	except ImportError:
		try:
			import msvcrt
		except ImportError:
			try:
				from EasyDialogs import AskPassword
			except ImportError:
				getpass = default_getpass
			else:
				getpass = AskPassword
		else:
			getpass = win_getpass
	else:
		getpass = unix_getpass
		
###Working with File Objects
Python has a built-in function,*open* , for opening a file on  disk.

*open* method take up three paras: a filename(required),a mode, and a buffering parameter.print open.__doc__ displays a great explanation of all the possible modes.

	f = open("/music/_singles_kairo.mp3", "rb")
	f.mode
	f.name

####Reading Files

	f.tell() # tells you your current position 
	f.seek(-128, 2) # 2 means move to a pos to the end of the file
	f.tell()
	tageData = f.read(128)# optional para specifies the max num of bytes to read

####Closing Files
	f.closed
	f.close()
	

####Handling I/O Errors
	try:
		fsock = open(filename, "rb", 0)
		try:
			fsock.seek(-128, 2)	
			tagdata = fsock.read(128)
		finally:
			fsock.close()
	
	.
	.
	.
	except IOError:
		pass

code in the *finally* block will always be excuted, even if something in the *try* block raises an exception.

####Writing to Files
"append" mode will add data to the end of the file.  

"write" mode will overwrite the file.  

	logfile = open('test.log', 'w')
	logfile.write('test succeeded')
	logfile.close()
	print file('test.log').read()
	
	logfile = open('test.log', 'a')
	logfile.write('line 2')
	logfile.close()
	print file('test.log').read()
	
###Using sys.modules
	import sys
	print '\n'.join(sys.modules.keys())
	
	import fileinfo
	print '\n'.join(sys.modules.keys())
	# a new modules are imported , and added to sys.modules
	
	fileinfo
	sys.modules["fileinfo"]
	# given the name of import module, you can get a reference to the module itself
	
	from fileinfo import MP3FileInfo
	MP3FileInfo.__module__
	sys.modules[MP3FileInfo.__module__]
	# combining this with sys.modules dic, you can get ref to the module in which a class is defined
	
	
		
###Working wiht Directories
constructing pathnames

	import os
	os.path.join("/Users/tangpeng/Music", "mahadeva.mp3")
	# join just simply concatenates strings
	os.path.expanduser("~")
	# represent the current user's home directory
	
---
splitting pathnames

	os.path.split("/Users/tangpeng/Music/bsb.mp3")
	(filepath, filename) = os.path.split("/Users/tangpeng/Music/bsb.mp3")
	(shortname, extension) = os.path.splitext(filename)
	
___
listing directories:

		
	os.listdir("/Users/tangpeng/Music")
	
	dirname = "/Users/tangpeng"
	os.listdir(dirname)
	
	[f for f in os.listdir(dirname)
		if os.path.isfile(os.path.join(dirname, f))]
	#using os.path.join to ensure a full pathname
	
	[f for f in os.listdir(dirname)
		if os.path.isdir(os.path.join(dirname,f))]	
---
listing directories in fileinfo.py

	
	def listDirectory(directory, fileExtList):
		"get list of file info objects for files of particular extensions"
		fileList = [ os.path.normcase(f) 
				for f in os.listdir(directory)]
		# will convert the entire filename to lowercase
		# it'll return the filename unchanged		
		fileList = [ os.path.join(directory, f)
				for f in fileList
				if os.path.splitext(f)[1] in fileExtList]	    # extension is the thing you care about
	# and join construct the full pathname	of the file
	
	
	