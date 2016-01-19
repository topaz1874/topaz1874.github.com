---
title: trydjango 1.8
date: 2015-07-22 16:50:06
layout: post
category: 
tags: 
summary: 
---
# Bootstrap 3.3

# crispy_forms



# Django registration redux

install

	pip install djang-registration-redux
	
settings

		# settings
		INSTALLED_APPS = (
		'django.contrib.auth',
		'django.contrib.sites',
		'registration',
		)
		
		ACCOUNT_ACTIVATION_DAYS = 7
		REGISTRATION_AUTO_LOGIN = True
		SITE_ID = 1
		LOGIN_REDIRECT_URL = '/'
		
		
		#urls.py
	    url(r'^accounts/', include('registration.backends.default.urls')),
	    
	    # update to sqllite
	    python manage.py migrate


# Font Awesome

EASIEST: BootstrapCDN by MaxCDN


Paste the following code into the <'head'> section of your site's HTML.

	<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">

		
	