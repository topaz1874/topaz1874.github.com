---
title: MongoDB_notes_3rd
date: 2015-06-29 22:24:38
layout: post
category: [Mongodb]
tags: [python, MongoDB]
summary: 
---
###Mongo Shell command

####Using $push,$pop,$pull,$pushAll,$pullAll,$addToSet

	db.arrays.insert({_id:0, a:[1,2,3,4]})
	db.arrays.find()
	# {"_id": 0, "a": [1,2,3,4]}
	db.arrays.update({_id:0},{$set:{'a.2':5})
	
	db.arrays.update({_id:0},{$push:{a:6}})
	
	db.arrays.update({_id:0},{$pop:{a:1}})
	db.arrays.update({_id:0},{$pop:{a:-1}})
	
	db.arrays.update({_id:0},{$pushAll:{a:[7,8,9]}})
	
	db.arrays.update({_id:0},{$pull:{a:5}})
	# $pull remove value from the array
	db.arrays.update({_id:0},{$pullAll:{a:[7,8,9]}})

	