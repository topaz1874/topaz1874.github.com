---
layout: page
title: "About"
description: ""
---
{% include JB/setup %}
###Case Study:Street Addresses

matching at the end of a string

	s = ' 100 NORTH BROAD ROAD'
	import re
	re.sub('ROAD$', 'RD.', s)
---
matching whole words
	
	s = '100 BROAD ROAD APT. 3'
	re.sub(r'\bROAD\b', 'RD.', s)
	
###Case Study:Roman Numerals

* I = 1
* V = 5
* X = 10
* L = 50
* C = 100
* D = 500
* M = 1000

general rules:

* characters are additive.
* the tens characters can be repeated up to three times.
* at 4, you need to subtract from the next higest fives character.(40 XL 10 less than 50, 44 XLIV )
* at 9, you need to subtract from the next highest tens character.(90 XC 10 less than 100, 900 CM)
* the fives characters can not be repeated.
* roman numerals are always written highest to lowest ,and read left to right, so the order of characters matters very much.(600 DC, 400 CD)

#### Checking for Thousands
checking for thousands

	import re
	pattern = '^M?M?M?$'
	re.search(pattern, 'M')

^ to match what follows only at the beginning of the string 

M? to optionally match a single M character.

$ to match what precedes only at the end of the string.When combined with the ^ character at the beginning, this means that the pattern must match the entire string, with no other characters before or after the M characters.

--- 	
checking for hundreds 

* 100 = C
* 200 = CC
* 300 = CCC
* 400 = CD
* 500 = D
* 600 = DC
* 700 = DCC
* 800 = DCCC
* 900 = CM

---
	
	import re
	pattern = '^M?M?M?(CM|CD|D?C?C?C?)$'
	re.search(pattern, 'MCM')
	re.search(pattern, 'MD')
	re.search(pattern, 'MMMCCC')
	re.search(pattern, 'MCMC') # failed
	re.search(pattern, ' ')

the(A|B|C)syntax means "match exactly one of A, B, or C".
	
###Using the {n, m} Syntax

the new way:from n o m
	
	pattern = '^M{0, 3}$'
	# match the satrt of the string ,then anywhere from zero # to three M characters, the the end of the string.
	
####Checking for Tens and Ones

checking for tens
	
	pattern = '^M?M?M?(CM|CD|D?C?C?C?)(XC|XL|L?X?X?X?)$'
	re.search(pattern, 'MCMXL') # 1940
	re.search(pattern, 'MCML') # 1950
	re.search(pattern, 'MCMLX') # 1960
	re.search(pattern, 'MCMLXXX') # 1980
	re.search(pattern, 'MCMLXXXX') # NONE
	
	pattern = '^M?M?M?(CM|CD|D?C?C?C?)(XC|XL|L?X?X?X?)(IX|IV|V?I?I?I?)$'
	
validating roman numerals with {n, m}

	pattern = '^M{0,3}(CM|CD|D?C{0,3})(XC|XL|L?X{0,3})(IX|IV|V?I{0,3})$'
	
	
###Verbose Regular Expressions
* whitespace is ignored.
* comments are ignored.

regular expressions with inline comments
	
	pattern = """
	^ 					# beginning of string
	M{0,4}				# thousands - 0 to 4 M's
	(CM|CD|D?C{0,3})	# hundreds - 900 (CM), 400(CD), 0-300 					
						# ( 0 to 3 C's), or 500-800 
						# (D, followed by 0 to 3 C's)
						
	(XC|XL|L?X{0,3})	# ...
	(IX|IV|V?I{0,3})	# ...
	$					# end of string
	"""
	
	re.search(pattern, 'M', re.VERBOSE)
	

---
	
	pattern = re.compile( """
	^ 					# beginning of string
	M{0,4}				# thousands - 0 to 4 M's
	(CM|CD|D?C{0,3})	# hundreds - 900 (CM), 400(CD), 0-300 					
						# ( 0 to 3 C's), or 500-800 
						# (D, followed by 0 to 3 C's)
						
	(XC|XL|L?X{0,3})	# ...
	(IX|IV|V?I{0,3})	# ...
	$					# end of string
	""", re.VERBOSE)
	
	pattern.search("MCMLXXX")
	
###Case Study: Parsing Phone Numbers
finding numbers

	phonePattern = re.compile(r'^(\d{3})-(\d{3})-(\d{4})$')
	phonePattern.search('800-555-1212').groups()
	
	phonePattern.search('800-555-1212-1234')
	
{3} means "match exactly three numeric digits";

\d means "any numeric digit" (0 through 9);

Putting it in parentheses means "match exactly three numeric digits, and then remember them as a group that I can ask for later".

---

finding the extension
	
	phonePattern = re.compile(r'^(\d{3})-(\d{3})-(\d{4})-(\d+)$')
	phonePattern.search('800-555-1212-1234').groups()
	
	phonePattern.search('800 555 1212 1234')
	phonePattern.search('800 555 1212')
	
---
	
handling different separators
	
	phonePattern = re.compile(r'^(\d{3})\D+(\d{3})\D+(\d{4})\D+(\d+)$')
	phonePattern.search('800 555 1212 1234').groups()	phonePattern.search('800−555−1212−1234').groups()
	
	phonePattern.search('80055512121234')
	
\D matches any character except a numeric digit, and + means "1 or more".

---

handling numbers without separators
	
	phonePattern = re.compile(r'^(\d{3})\D*(\d{3})\D*(\d{4})\D*(\d*)$')
	phonePattern.search('80055512121234').groups()	phonePattern.search('800.555.1212 x1234').groups()	phonePattern.search('800−555−1212').groups()
		phonePattern.search('(800)5551212 x1234')
	
\* means "zero or more"

---

handling leading characters
	
	phonePattern = re.compile(r'^\D*(\d{3})\D*(\d{3})\D*(\d{4})\D*(\d*)$')
	phonePattern.search('(800)5551212 ext. 1234').groups()	phonePattern.search('800−555−1212').groups()
		phonePattern.search('work 1−(800) 555.1212 #1234')
---
parsing phone numbers (final version)
	
	phonePattern = re.compile(r"""
				# don't match beginning of string, number 
				# can start anywhere)
	(\d{3})		# area code is 3 digits (e.g '800')
	\D*			# optional separator is any number of 
				# non-digit
	(\d{3})		# trunk is 3 digits
	\D*			# optional separator
	(\d{4})		# rest of number is 4 digits
	\D*			# optional separator
	(\d*)		# extension is iptional 
	$
	""", re.VERBOSE)
	
	phonePattern.search('work 1−(800) 555.1212 #1234').groups()
	
	
###Summary

* ^ matches the beginning of a string 
* $ matches the end of a string
* \b matches a word boundary
* \d matches any numeric digit
* \D matches any non-numeric character
* x? matches an optional x character (x zero or one times)
* x* matches x zero or more times
* x+ matches x one or more times
* x{n,m} matches an x character at least n times, but no more than m times
* (a|b|c) matches either a or b or c
* (x) in general is a remembered group.Youcan get the value of what matched by using the groups() method of the boject returned by re.search



	
