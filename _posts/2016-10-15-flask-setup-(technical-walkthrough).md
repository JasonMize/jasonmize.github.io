---
layout: post
title: Flask Setup (technical walkthrough)
---

[Flask](http://flask.pocoo.org/ "Flask") is a minimalist web framework. It helps the developer to build and maintain websites without having to reuse code or go to extra effort. 

The key takeaway from that description is "minimalist". Flask is quite powerful but doesn't come overloaded with features out of the box. Taking advantage of its full potential requires turning to third party plugins and [extensions](http://flask.pocoo.org/extensions/ "extensions").

Because of this, my advice is to use Flask (which I love) for simpler projects that only need a limited feature set. Anything more complicated, turn to [Django](https://www.djangoproject.com/ "Django").

## Directions

This post is a technical walkthrough for setting up Flask on a Macbook Pro.  It assumes you have already installed: 
1. [Pip](https://pip.pypa.io/en/stable/installing/ "Pip")
2. [Virtualenv](https://virtualenv.pypa.io/en/stable/ "Virtualenv")

Any line preceeded by a "$" is intended as a terminal command. 

Lines to insert into files will be shown as a code snippet. For example:

**filename:**
{% highlight html %}
line you should copy into file 
{% endhighlight%}

Anytime you see "project_name", change it to the name of your project. 


## Start

$ `mkvirtualenv -p python3 project_name`

$ `pip install flask`

$ `pip freeze > requirements.txt`

$ `touch .gitignore`

**.gitignore:**
{% highlight python %}
__pycache__
*.pyc
{% endhighlight %}

$ `mkdir app`

$ `mkdir app/static`

$ `mkdir app/templates`

$ `mkdir app/templates/includes`

$ `touch run.py`

**run.py:**
{% highlight python %}
import sys`
from app import app`
app.run(debug=True)`
{% endhighlight %}

$ `touch __init__.py`

**\__init__.py:**
{% highlight python %}
from flask import Flask
app = Flask(__name__)
from app import views
from app import filters
{% endhighlight %}

## CSS and HTML Templates

$ `touch app/static/styles.css`

$ `app/templates/base.html`

**app/templates/base.html:**
{% highlight html %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <header>something</header>
    {% raw %}
    {% block content %}
        
    {% endblock content %}
    {% endraw %}
</body>
<footer></footer>
</html>
{% endhighlight %}

$ `touch app/templates/index.html`

**app/templates/index.html:**
{% highlight html %}
{% raw %}
{% extends "base.html" %}

{% block content %}
    <h1>Hello World</h1>
{% endblock content %} {% endraw %}
{% endhighlight %}

## Routing URL's

$ `touch app/views.py`

**app/views.py**
{% highlight python %}
import os

from flask import render_template

from app import app

@app.route("/")
@app.route("/index")
def index():
    return render_template("index.html")
{% endhighlight %}

$ `touch app/filters.py`

**app/filters.py**
{% highlight python %}
from app import app
{% endhighlight %}


## Forms

$ `pip install wtforms`

$ `pip freeze > requirements.txt`

$ `touch app/forms.py`

**app/forms.py**
{% highlight python %}
from wtforms import Form, StringField, RadioField, validators, ValidationError

class FormName (Form):
    attribute1 = StringField ("Something", validators=[validators.DataRequired()])
    attribute2 = RadioField ( "Name",
        default = "option",
        choices = [
            ("choice1", "choice1",),
            ("choice2", "choice2",),
        ]
    )
{% endhighlight %}
    





