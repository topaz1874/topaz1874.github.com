---
title: 2015-03-05-diveintopython-notes-chapter-9
date: 2015-03-05 11:39:04
layout: post
category: [diveintopython, chapter 9]
tags: [python]
summary: 
---

###Parsing XML documents using *minidom*

Parsing XML document is very simple:one line fo code.

	from xml.dom import minidom
	xmldoc = minidom.parse('.../kgp/binary.xml')
	print xmldoc.toxml()
	
toxml is a method of the Node class.
toxml prints out the XML that this Node represents.

###Searching through the parsed documents

There is a shortcut you can use to find something in particular quickly:*getElementsByTagName*

	from xml.dom import minidom
	xmldoc = minidom.parse('binary.xml')
	reflist xmldoc.getElementsByTagName('ref')
	reflist
	print reflist[0].toxml()
	print reflist[1].toxml()
	
	firstref = reflist[0]
	plist = firstref.getElementsByTagName('p')
	plist
	print plist[0].toxml()
	print plist[1].toxml()

###Accessing arbitrary element attributes and element children

Accessing element attributes

	xmldoc = minidom.parse('binary.xml')
	reflist = xmldoc.getElementsByTagName('ref')
	bitref = reflist[0]
	print bitref.toxml()
	
	bitref.attributes
	bitref.attributes.keys()
	bitref.attributes.values()
	bitref.attributes["id"]
	
Each *element* object has an attribute called *attributes*, which is a *NamedNodeMap* objec.A *NamedNodeMap* is an object that acts like a dictionary.

You can get a list of the names of the attributes of this element by using *attributes.keys()*

You can also get a list of the values of the attributes by using *attributes.values()*.
The values are themselves objects ,of type *Attr*

Accessing individual attributes

	a = bitref.attributes["id"]
	a.name    #u'id'
	
	a.value	    #u'bit'

The *Attr* object represents a single XML attribute of a single XML  element. The name of the attribute is stored in a.name.

The actual text value of this XML attribute is stored in a.value.

###Organizing complex libraries into packages

	from xml.dom import minidom

xml is what is known as a  package, dom is a nested package within xml, and minidom is a module within xml.dom.

Packages are little more than directories of modules; nested packages are subdirectories.The modules within a package are still just .py files.
	
	from xml.dom import minidom
	minidom   # module
	
	minidom.Element  # class
	
	from xml.dom.minidom import Element
	Element  # class
	
	minidom.Element   # class
	
	from xml import dom
	dom    # module
	
	import xml 
	xml    # module
	
Not only can you import entire modules contained within a package , you can selectively import specific calsses or functions from a module contained within a package.

You can also import the package itself as a module.

A package is a directory with special __init__.py file in it. The init.py file defines the attributes and methods of the package. It doesn't need to define anything; it can just be an empty file , but it has to exist.

###Converting unicode strings to different character encodings

When Python parses an XML document, all data is stored in memory as unicode.

	s = u'Dive in '
	s
	print s

When printing a string, Python will attempt to convert it to your default encoding, which is usually ASCII.

	s = u'La pe\xf1a'
	print s
	# UnicodeError
	
	print s.encode('latin-1')

You  can customize default encoding schenme

	import sys
	sys.setdefaultencoding('iso-8859-1')

If you are going to be storing non-ASCII strings within your Python code, you'll need to specify the encoding of each individual .py file by putting an encoding declaration at the top of each file.This declaration defines the .py file to be UTF-8:
	
	#! /usr/bin/env python
	# -*- coding: UTF-8 -*-
	