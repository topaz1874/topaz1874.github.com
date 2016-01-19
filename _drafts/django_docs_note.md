---
title: django_docs_note
date: 2015-08-13 15:44:17
layout: post
category: 
tags: 
summary: 
---

###Retrieving objects

####Retrieving specific objects with filters

filter(**kwargs)

exclude(**kwargs)

The lookup parameters should be in the format described in [Field lookups][Field_lookups] below.

Chaining filter
	
	Entry.objects.filter(
	
	headline__startswith='What'.
	exclude(pub_date__gte=datetime.date.today().
	filter(pub_date__gte=datetime(2005, 1, 30))


The final result is a QuerySet containing all entries with a headline that starts with “What”, that were published between January 30, 2005, and the current day.

####Retrieving a single object with get

There is a difference between using **get()** and using **filter()**.If there are no results that match query, **get()
** will raise a **DoesNotExist** exception.

more than one item matches will raise **MultipleObjectsReturned**

####Limiting QuerySets

	Entry.objects.all()[:5]

	Entry.objects.all()[:10:2]





[Field_lookups]: 
####Field lookups

Basic lookups keyword arguments take the form **field__lookuptype=value**


	Entry.objects.filter(pub_date__lte='2006-01-01')


There's one exception though, in case of a **ForeignKey** you can specify the field name suffixed with **_id**.

	Entry.objects.filter(blog_id=4)
	
Here's some common lookups you'll probably use:

**exact**
	
	Blog.objects.get(id__exact=14)
	Blog.objectsget(id=14)
	
**iexact**
A case-insensitive match.

	Blog.object.get(name__iexact="beatles blog")


Woud match "Beatles Blog", "beatles blog",etc.


**contains**


**icontains**


**startswith**,**endswith**


**istarswith**,**iendswith**



####Filters can reference fields on the model

Instances of **F()** act as a reference to a model field within a query. These references can be used in query filters to compare the values of two different fields on the same model instance.


	from django.db.models improt F
	Entry.objects.filter(n_comments__gt=F('n_pingbacks'))
	
	Entry.objects.filter(n_comments__gt=F('n_pingbacks')*2)	
	
	Entry.objects.filter(rating__lt=F('n_comments') + F('n_pingbacks'))
	
####The pk lookup shortcut

	Blog.objects.get(id___exact=14)
	Blog.objects.get(id=14)
	Blog.objects.get(pk=14)

	# Get blogs entries with id 1,4 and 7
	Blog.objects.filter(pk__in=[1,4,7])
	
	# Get all blog entries with id>14
	Blog.objects.filter(pk__gt=14)

###Complex lookups with Q objects

A **Q object(django.db.models.Q)** is an object used to encapsulate a collection of keyword arguments.


	form django.db.models import Q
	Q(question__startswith='What')

	Q(question__startswith='Who') |
	~Q(pub_date__year=2005)

	Poll.objects.get(Q(question__startswith='Who'), Q(pub_date=date(2005, 5, 2))| Q(pub_date=date(2005,5,6)))



















