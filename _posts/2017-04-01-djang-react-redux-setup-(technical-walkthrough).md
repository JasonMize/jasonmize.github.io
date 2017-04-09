---
layout: post
title: Django + React + Redux Setup (technical walkthrough)
---

## Introduction:

[Django](https://www.djangoproject.com/ "Django") is a web framework. It can be complicated to learn but is both powerful and versatile as a projects back end. A good choice for complex apps. It is written in Python.

[React](https://facebook.github.io/react/ "React") is a library for front end user interfaces. It is written in Javascript. 

[Redux](http://redux.js.org/ "Redux") is a framework that tracks object state. It is used in conjunction with user interfaces like React or Angular. It is written in Javascript.

This is a learning project for me. I'll try to identify parts that I am less sure of. Please let me know anything I can correct. 


## Directions: 

This post is a technical walkthrough for setting up Django + React + Redux on a Macbook Pro.  It assumes you have already set up a basic Django + Django Rest Framework 'Hello World'.  We'll be starting from there. 

Any line preceeded by a "$" is intended as a terminal command. For example:

$ `line you should copy into terminal`

Lines to insert into files will be shown as a code snippet. For example:

**filename:**
{% highlight html %}
line you should copy into filename 
{% endhighlight%}

Anytime you see "project_name", change it to the name of your project. 


## Confession:

I was unsuccessful. This walkthrough succeeds in creating an environment that bundles your information, watches it with webpack, and seems to be working correctly. So the hard part is done.  However, there is an error in the url paths.  The user gets to the Django back-end index.html but fails to be forwarded to the React front-end index.js. As soon as I fix this I will update this tutorial. If you figure it out first, let me know.  :)


# Start: 


## Step 1: Install Packages

$  `pip install django-webpack-loader`
  [Django-Webpack-Loader](https://pypi.python.org/pypi/django-webpack-loader/ "django-webpack-loader"): The magic of React/Redux is that they allow a website to be dynamically refreshed without loading a new url. This is done by creating a static bundle of information and then updating it as information changes. Webpack watches that bundle and updates Django when it changes. 

$  `pip install dj-database-url`
  [Dj-Database-Url](https://pypi.python.org/pypi/dj-database-url/ "dj-database-url"): My impression is that this package is what allows us to pass information in variables embedded inside urls. 

$  `pip install gunicorn`
  [Gunicorn](http://docs.gunicorn.org/en/stable/ "Gunicorn"): My impression is that Gunicorn acts like a lightweight version of Apache for local development. An http server. 

$  `pip install python-decouple`
  [Python-Decouple](https://pypi.python.org/pypi/python-decouple/ "Python-Decouple"): This package lets you set environment variables in a .env file so your configuration parameters are easily accessible. 

$  `pip install requests`
  [Requests](https://pypi.python.org/pypi/requests/ "Requests"): No clue what this does. Anybody? 

$  `pip install whitenoise`
  [Whitenoise](http://whitenoise.evans.io/en/stable/ "Whitenoise"): This package lets you serve static files (images, etc.) and store them inside your own file structure instead of relying on Amazon S3 buckets and similar options.

$  `pip install unipath`
  [Unipath](https://pypi.python.org/pypi/Unipath): No idea. Sorry.

$  `pip freeze > requirements.txt`
  Save your packages to a 'requirements.txt' file so that other users know what you have installed and can quickly replicate your build.

**.gitignore**   Update .gitignore so your Git repository only has what it should.
{% highlight html %}
venv
staticfiles
.env
env
node_modules
<project-name>/static/build
{% endhighlight%}







