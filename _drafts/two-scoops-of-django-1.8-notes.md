---
title: two scoops of django 1.8 notes
date: 2015-08-11 16:24:46
layout: post
category: 
tags: 
summary: 
---

###5 Settings and Requirements Files

####5.1 Avoid non-versioned local settings

got it .

####5.2 Using Multiple Settings Files
	
####5.3 Separate Configuration From Code

#####5.3.2 Set Environment Variables Locally

	$ export SOME_SECRET_KEY=1C3-1E123C-C23R-2341
	$ export AUDREY_FREEZER_KEY=Y234-2342-234SDF-23414	
	###6 Model Best practices
####6.1 Basics
####6.1.2 Be Careful With Model Inheritance
don't know what the hack is model inheritance
####6.1.3 Model Inheritance in Practice:The TimeStampedModel
	#core/models.py
	from django.db import models
	class TimeStampedModel(models.Model):
		created = models.DateTimeField(auto_now_add=True)

	modified = models.DateTimeField(auto_now=True)


	class Meta:
		abstract = True
		

---


	# flavors/models.py
	
	from django.db import models
	
	from core.models import TimeStampedModel
	
	class Flavor(TimeStampedModel):
		title = model.CharField(max_length=200)
		
####6.2 Django Model Design


###7 Queries and the Database Layer

####7.1 Use get_object_or_404() for Single Object

Only use it in views.

####7.2 Be Careful With Queries That Might Throw Exception

####7.2.1 ObjectDoesNotExist vs. DoesNotExist

ObjectDoesNotExist can be applied to any models.

	from django.core.exceptions import ObjectDoesNotExist
	from flavors.models import Flavor
	from store.exceptions import OutOfStock
	
	def list_any_line_item(model, sku):
		try:
			return model.object.get(sku=sku, quantity__gt=0)
		except ObjectDoesNotExist:
			msg = "We are out of {0}".format(sku)

		rasie OutOfStock(msg)
		

		
