---
title: MongoDB_notes_2nd
date: 2015-05-26 14:44:42
layout: post
category: [Mongodb]
tags: [python,MongoDB]
summary: 
---

###Example of Aggregation Framework

eg db.collections.aggregate

	result = db.tweets.aggregate([
		{"$group": {"_id": "$user.screen_name", "count": {"$sum": 1}}},
		{"$sort": {"count": -1}}])
		
		
###Aggregation operators

$project

$match -- filtering out documents

$group

$sort

$sk skip

$limit

---

$unwind 

For every value of an array field on which we're using unwind,it will create an instance of the document containing that array field, for every value in the array.

eg."arrayFieldName": ["val1","val2","val3"]

using unwind we get: {"arrayFieldName":"val1"},
{"arrayFieldName":"val2"},{"arrayFieldName":"val3"}.

###Match operator

eg $macth

	def highest_ratio():
		result = db.tweets.aggregate([
		{"$match": {"user.friends_count":{"$gt": 0},		   {"user.followers_count":{"$gt": 0}}},
		{"$project":{"ratio": {"$divide":["$user.followers_count","$user.friends_count"]},
		 		    "screen_name": "$user.screen_name"}},
		{"$sort" :{"ratio": -1}},
		{"$limit":1} ])
		
		return result

###Project operator

use $project to:

- include fields from the original document
- insert computed fields
- rename fields
- create fields that hold subdocuments

###Unwind operator

eg $unwind

	result = db.tweets.aggregate([
		{"$unwind" : "$entities.user_mentions"},
		{"$group": {"_id": "$user.screen_name",
					"count": {"$sum" :1}}},
		{"$sort":{"count" :-1}},
		{"$limit": 1} 
		])


###Group operators

- $sum
- $first
- $last
- $max
- $min
- $avg

eg $avg

	def hashtag_retweet_avg():
		result = db.tweets.aggregate([
				{"$unwind" : "$entities.hashtags"},
				{"$group": {"_id": "$entities.hashtags.text", "retweet_avg" : {"$avg": "$retweet_count"}}},
				{"$sort" :{"retweet_avg": -1}}
		])
		return result
		
---

- $push
- $addToSet

eg $addToSet
	
	def unique_hashtags_by_user():
		result = db.tweets.aggregate([
				 {"$unwind": "$entities.hashtags"},
				 {"$group": {"_id": "$user.screen_name", 
				 "unique_hashtags": { "$addToSet": "$entities.hashtags.text"}}},
				 {"$sort" : {"_id" : -1}}
				 ])
		return result
		
		
###Multiple stages using a given operator

eg $addToSet

	def unique_user_mentions():
		result = db.tweets.aggregate([
			{ "$unwind" : "$entities.user_mentions"},
			{ "$group": {
					"_id": "$user.screen_name",
					"mset" : {
						"$addToSet" :"$entities.user_mentions.screen_name"}}},
			{"$unwind" : "$mset" },
			{"$group" : {"_id" : "$_id", "count" : {"$sum" :1}}},
			{"$sort": {"count": -1}},
			{"$limit" : 10}
		])


	