---
title: MongoDB_notes
date: 2015-05-24 21:02:10
layout: post
category: [MongoDB]
tags: [python,MongoDB]
summary: 
---

###Intro Pymongo
	
	from pymongo import MongoClient
	
	client = MongoClien('mongodb://localhost:27017/')
	tesla_s = {
		"manufacturer": "Tesla Motors",
		...
		}
		
	db = client.examples
	db.autos.insert(tesla_s)
	
	for a in db.autos.find():
		print a
		
###Querying using field selection

	autos = db.autos.find({"manufacturer": "Toyota"})
	
###Multiple Field Queries

	autos = db.autos.find({"manufacturer": "Toyota", "class": "mid-size car"})	
	

###Projection queries
	
	query = {"manufacturer": "Toyota", "class": "mid-size car"}
	projection = {"_id": 0, "name": 1}
	
	autos = db.autos.find(query, projection)
	
###Using mongoimport

	mongoimport  -d examples -c myautos --file autos.json
	
	-d database
	-c collections
	--file file paths

###Range queries

Inquality operators

- $gt
- $lt
- $gte
- $lte
- $ne

---
	query = {"population": {"$gt": 250000}}
	cities = db.cities.find(query) 
	
	query = {"name" : {"$gte" : "X", "$lt" :"Y"}}
	cities = db.cities.find(query)
	
	
	query = {"foundingDate" : {"$gte" : datetime(1837, 1,1), "$lte" : datetime(1873,12,31)}}
	
###Exists

using mongoDB shell

	db.cities.find({"govermentType" : {"$exists": 1}})
	
###Regex operator

	db.cities.find({"moto": {"$regex": "[Ff]riendship"}})
	
###Querying arrays using scalars

	db.autos.find({"modelYears": 1980})

###in operator
	
	db.autos.find({"modelYears": {"$in": [1965, 1966, 1967]}})
	
###all operator

	db.autos.find({"modelYears": {"$all":[1965, 1966]}})

###Dot notaion

Mongodb can query for values inside nested documents. Using a dot notaion syntax.

	db.autos.find({"dimensions.weight": {"$gt": 50000}})
	
###Updates

	city = db.cities.find_one({"name" : "Munchen", "country": "Germany"})
	city["isoCountryCode"] = "DEU"
	db.cities.save(city)
	
###Set unset

	city = db.cities.update({"name": "Muchen", "country": "Germany"},
	{"$set": {"isoCountryCode": "DEU"}}
	)
---
unset

	city = db.cities.update({"name": "Muchen", "country": "Germany"},
	{"$unset": {"isoCountryCode": ""}}
	)
---
do not do this

	city = db.cities.update({"name": "Muchen", "country": "Germany"},
	{"isoCountryCode": "DEU"}
	)
	
###Multi update
	
	db.cities.update({"country": "Germany"},
	{"$set": {"isoCountryCode": "DEU"}},
	multi=True)

###Removing documents

remove the entire collection
	
	db.cities.drop()
remove individual documents 

	db.cities.remove({"name": "Chicago"})
	db.cities.remove({"name": {"$exists": 0}})
	