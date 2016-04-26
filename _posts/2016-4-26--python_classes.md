---
title: Python class
date: 2016-04-26 14:10:42
layout: post
category: [python]
tags: [python]
summary: 
---

####static methods

Class attributes are attributes that are set at the class-level.Normal attributes are introduced in the *__init__* method, but some attributes of a class hold for all instances in all cases.

	class Car(object):
		wheels =4
		
		def __init__(self,make, model):
			self.make = make
			self.model = model
			
	mustang = Car('Ford', 'Mustang')
	
	print mustang.wheels
	# 4
	print Car.wheels
	# 4
A Car always has four wheels, regardless of the make or model.Instance methods can access these attributes in the same way they access regular attributes:through self.wheels.

	
*static methods* don't have access to *self*. Just like class attributes, they are methods that work without requiring an instance to be present.Since instances are always referenced through *self*,static methods have no *self* parameter.

	class Car(object):
		...
		@staticmethod
		def make_car_sound():
			print 'VRoooommmmm!'

####Class method
A variant of the static method is the class method.Instead of receiving the instance as the first parameter, it is passed the class.
	
	class Vehicle(object):
		...
		@classmethod
		def is_motorcycle(cls):
			return cls.wheels ==2
			
			
####Abstract Classes
Abstract Base Classes are classes that are only meat to be inherited from;you can't create instance of an ABC.That mean that, if Vehicle is an ABC, the follow is illegal:
	
	v = Vehicle(4,0,'Honda','Accord',2014,None)

So how do we make a class an ABC?
The abc module contains a metaclass called ABCMeta.Setting a class's metaclass to *ABCMeta* and making one of its methods *virtual* makes it an ABC.

	from abc import ABCMeta, abstractmethod
	
	class Vehicle(object):
		
		__metaclass__ = ABCMeta
		
		def sale_price(self):
			if self.sold_on is not None:
				return 0.0
			return 5000.0 * self.wheels
		
		def purchase_price(self):
			if self.sold_on is None:
				return 0.0 
			return self.base_sale_price - (.10 * self.miles)
		
		@abstractmethod
		def vehicle_type():
			pass
			

Now, since *vehicle_type* is an *abstractmethod*, we can't directly create an intance of Vehicle.As long as Car and Truck inherit from Vehicle and define vehicle_type, we can instantiate those classes just fine.
	