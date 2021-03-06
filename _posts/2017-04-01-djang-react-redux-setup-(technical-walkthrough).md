---
layout: post
title: Django + React + Redux Setup (technical walkthrough)
---

## Introduction:

[Django](https://www.djangoproject.com/ "Django") is a web framework. It can be complicated to learn but is both powerful and versatile as a projects back end. A good choice for complex apps. It is written in Python.

[React](https://facebook.github.io/react/ "React") is a library for front end user interfaces. It allows components on a webpage to be updated without sending the user to a different url. It is written in Javascript. 

[Redux](http://redux.js.org/ "Redux") is a framework that tracks object state. It is used in conjunction with user interfaces like React or Angular. It is written in Javascript.

This is a learning project for me. I'll try to identify parts that I am less sure of. Please let me know anything I can correct. 


## Goals: 

This post is a technical walkthrough for setting up Django + React + Redux on a Macbook Pro.  It assumes you have already set up a basic Django + Django Rest Framework 'Hello World'.  We'll be starting from there. You're in charge of your own GitHub commits. 

Any line preceeded by a "$" is intended as a terminal command. For example:

$ `line you should copy into terminal`

Lines to insert into files will be shown as a code snippet. For example:

#### **filename:**
{% highlight html %}
line you should copy into filename 
{% endhighlight %}

Anytime you see "project_name", change it to the name of your project. 


## Confession:

I was unsuccessful. This walkthrough succeeds in creating an environment that bundles your information, watches it with webpack, and seems to be working correctly. However, there is an error somewhere. The user gets to the Django back-end index.html but fails to be forwarded to the React front-end index.js. As soon as I fix this I will update this tutorial. If you figure it out first, let me know.  :)


# Start: 



## Step 1: Install Packages

$  `pip install django-webpack-loader`
[Django-Webpack-Loader](https://pypi.python.org/pypi/django-webpack-loader/ "django-webpack-loader"): The magic of React/Redux is that they allow a website to be dynamically refreshed without loading a new url. This is done by creating a static bundle of information and then updating it as information changes. Webpack watches that bundle and updates Django when it changes. 

$  `pip install dj-database-url`
[Dj-Database-Url](https://pypi.python.org/pypi/dj-database-url/ "dj-database-url"): This simplifies database configuration, making it a url instead of separate configuration parameters.

$  `pip install gunicorn`
[Gunicorn](http://docs.gunicorn.org/en/stable/ "Gunicorn"): Gunicorn is a wsgi (web server gateway interface) for Django. It runs python processes for the Django apps. Basically an app server behind the web server.

$  `pip install python-decouple`
[Python-Decouple](https://pypi.python.org/pypi/python-decouple/ "Python-Decouple"): This package lets you set environment variables in a .env file so your configuration parameters are easily accessible. 

$  `pip install requests`
[Requests](https://pypi.python.org/pypi/requests/ "Requests"): This is a library for making http requests from the back-end Django side instead of from your browser. This lets Django call other web service to get data.

$  `pip install whitenoise`
[Whitenoise](http://whitenoise.evans.io/en/stable/ "Whitenoise"): This package lets you serve static files (images, etc.) and store them inside your own file structure instead of relying on Amazon S3 buckets and similar options.

$  `pip install unipath`
[Unipath](https://pypi.python.org/pypi/Unipath): This is a wrapper library that gives a cleaner interface to the filesystem. It simplifies movement through directories, opening files, etc.

$  `pip freeze > requirements.txt`
Save your packages to a 'requirements.txt' file so that other users know what you have installed and can quickly replicate your build.

$  `mkdir requirements` 
Create folder structure to manage requirements for different environments.

$  `echo “r- base.txt” > requirements/production.txt`

$  `echo “r- base.txt” > requirements/local.txt`

$  `echo “r- base.txt” > requirements/testing.txt`

$  `cp requirements.txt requirements/base.txt`
Copy existing requirements into base.txt.


#### **requirements.txt**
Redirect requirements.txt to the full list for local and production environments. 
{% highlight html %}
-r requirements/production.txt
{% endhighlight %}


#### **.gitignore:**
Update .gitignore so your Git repository only has what it should.
{% highlight html %}
venv
staticfiles
.env
env
node_modules
<project-name>/static/build
{% endhighlight %}



## Step 2: Change Project Settings Into Package

Rename `<project-name>` folder to `config`

$  `mkdir config/settings`

$  `touch config/settings/__init__.py`

$  `touch config/settings/local.py`

$  `touch config/settings/production.py`

Move settings.py to config folder

Rename settings.py to 'base.py'



## Step 3: Update Django Settings

#### **.manage.py:**
Update your naming to reflect the change to `config`.

Change
{% highlight html %}
os.environ.setdefault(“DJANGO_SETTINGS_MODULE”, “<project-name>.settings”)
{% endhighlight %}
To
{% highlight html %}
os.environ.setdefault(“DJANGO_SETTINGS_MODULE”, “config.settings.base”)
{% endhighlight %}


#### **.config/wgsi.py**
This also updates naming to reflect the change to `config`.

Change
{% highlight html %}
os.environ.setdefault(“DJANGO_SETTINGS_MODULE”, “<project-name>.settings”)
{% endhighlight %}
To
{% highlight html %}
os.environ.setdefault(“DJANGO_SETTINGS_MODULE”, “config.settings.base”)
{% endhighlight %}


#### **.config/settings/base.py**
These are mostly changes that reconfigure paths and naming schemes.

Add to imports at top of file:
{% highlight html %}
from unipath import Path
{% endhighlight %}

Change
{% highlight html %}
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
{% endhighlight %}
To
{% highlight html %}
BASE_DIR = Path(__file__).ancestor(3)
{% endhighlight %}

Add line below `BASE_DIR`:
{% highlight html %}
DIST_DIR = BASE_DIR.ancestor(1).child("dist")
APPS_DIR = BASE_DIR.child(“apps”)
TEMPLATE_DIR = BASE_DIR.child(“templates”)
STATIC_FILE_DIR = BASE_DIR.child(“static”)
{% endhighlight %}

Change
{% highlight html %}
ROOT_URLCONF = ‘<project-name>.urls’
{% endhighlight %}
To
{% highlight html %}
ROOT_URLCONF = ‘config.urls’
{% endhighlight %}

Change
{% highlight html %}
WSGI_APPLICATION = ‘<project-name>.wsgi.application’
{% endhighlight %}
To
{% highlight html %}
WSGI_APPLICATION = ‘config.wsgi.application’
{% endhighlight %}

Change
{% highlight html %}
TEMPLATES = [
    {
        ‘DIRS’: [],
    },
]
{% endhighlight %}
To
{% highlight html %}
TEMPLATES = [
    {
        ‘DIRS’: [TEMPLATE_DIR],
    },
]
{% endhighlight %}

Beneath `TEMPLATES` add:
{% highlight html %}
STATICFILES_DIRS = (
    STATIC_FILE_DIR,
    DIST_DIR,
) 
{% endhighlight %}

Beneath `STATIC_URL` add:
{% highlight html %}
WEBPACK_LOADER = {
    'DEFAULT': {
        'BUNDLE_DIR_NAME': 'bundles/',
        'STATS_FILE': os.path.join(BASE_DIR, '../webpack-stats.json')
    }
}
{% endhighlight %}

Rename `INSTALLED_APPS` to `DJANGO_APPS`.

Below `DJANGO_APPS` add:
{% highlight html %}
THIRD_PARTY_APPS = [ 'webpack_loader', ]
PROJECT_APPS = []
INSTALLED_APPS = DJANGO_APPS + THIRD_PARTY_APPS + PROJECT_APPS
{% endhighlight %}

$  `touch .env` 
This file is where you will save environment configuration parameters.

#### **config/settings/production.py**
These are settings for your production environment.
{% highlight html %}
from .base import *

DEBUG = False
{% endhighlight %}


#### **config/settings/local.py**
These are settings for your local environment.
{% highlight html %}
from .base import *

DEBUG = True
{% endhighlight %}



## Step 4: Install NPM, Webpack, React, Redux
    
$  `npm init` 
[NPM](https://www.npmjs.com/ "NPM") stands for Node Project Manager. It handles front-end project dependencies for React/Redux. Just follow the terminal prompts to set it up.

$  `npm install --save-dev webpack webpack-bundle-tracker webpack-dev-server babel-core babel babel-loader babel-preset babel-preset-react babel-preset-stage react react-hot-loader react-dom react-router redux react-redux react-router-redux`
These are the project dependencies for React/Redux. 



## Step 5: Configure Webpack

$  `mkdir -p apps/static/js`
This is where our front end javascript files will live.

$  `touch webpack.config.js`
A file for our Webpack configuration.

$  `touch apps/static/js/index.js`
The root file that our site will point to. Our site "home".


#### **webpack.config.js**

{% highlight html %}
var path = require('path')

module.exports = {
    context: __dirname,
    entry: {
        'index': './apps/static/js/index.js',
    },
    module: {
        loaders: [{
            test: /.js?$/,
            loader: 'babel-loader',
            exclude: /node_modules/,
            query: {
                presets: [
                    'es2015', 'react', 'stage-0',
                ],
            }
        }],
    },
    resolve: {
        extensions: ['.js', '.jsx']
    },
    output: {
        path: path.resolve('./apps/static/bundles/'),
        publicPath: '/js',
        filename: 'bundle.js',
    },
}
{% endhighlight %}


#### **package.json**

Add to `scripts` dictionary 
{% highlight html %}
"build": "webpack --config webpack.config.js --progress --colors",
"build-production": "webpack --config webpack.prod.config.js --progress --colors",
"watch": "node server.js"
{% endhighlight %}


$  `touch server.js`
This will define our webpack server settings.


#### **server.js**
{% highlight html %}
var webpack = require('webpack')
var WebpackDevServer = require('webpack-dev-server')
var config = require('./webpack.config')

new WebpackDevServer(webpack(config), {
    publicPath: config.output.publicPath,
    hot: true,
    inline: true,
    historyApiFallback: true
}).listen(3000, '0.0.0.0', function (err, result) {
    if (err) {
        console.log(err)
    }

    console.log('Listening at 0.0.0.0:3000')
})
{% endhighlight %}

$  `mkdir apps/static/bundles`

$  `touch apps/static/bundles/bundle.js`
This file will hold the information about our current state.

$  `node server.js`
Start your server.

$  `npm run watch`
Start webpack watching our bundle file.



## Step 6: Django Setup

#### **`<project-name>`/templates/`<project-name>`/index.html**
This is the back-end Django file that the site lands on before forwarding us to our front-end index.js.

{% highlight django %}{% raw %}

{% load staticfiles %}

{% block page_content %}
    <div id="root">
        LOADING
    </div>
{% endblock page_content %}

{% block body_scripts %}
    <script type="text/jsx" src="{% static 'js/index.js'%}"></script>
{% endblock body_scripts %}
{% endraw %}{% endhighlight %}


#### **`<project-name>`/templates/views.py**

{% highlight django %}{% raw %}
from django.views.generic import TemplateView

class React(TemplateView):
    template_name = '<project-name>/index.html'
{% endraw %}{% endhighlight %}


#### **`<project-name>`/templates/urls.py**

Change `urlpatterns` to
{% highlight django %}{% raw %}
urlpatterns = [
    url(r'^index', views.React.as_view(), name='index'),
]
{% endraw %}{% endhighlight %}



## Step 7: React / Redux Setup

#### **`<project-name>`/apps/static/js/index.js**
This is the front-end landing page.

{% highlight django %}{% raw %}
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { Router, Route, hashHistory, Redirect } from 'react-router';
import { syncHistoryWithStore } from 'react-router-redux';

import indexStore from './stores/indexStore';
import ReactIndex from './components/ReactIndex';

global.reduxStore = indexStore;

const history = syncHistoryWithStore(hashHistory, indexStore);
ReactDOM.render(
    <Provider store={indexStore}>
        <Router history={history}>
            <Redirect from ="/" to="reactIndex/" />
            <Route path="reactIndex/" component={ReactIndex} /> 
        </Router>,
    </Provider>,
    document.getElementById('root'),
);
{% endraw %}{% endhighlight %}


### Create the file structure for the React/Redux files.

$ `mkdir apps/static/js/components`

$ `mkdir apps/static/js/constants`

$ `mkdir apps/static/js/actions`

$ `mkdir apps/static/js/middlewares`

$ `mkdir apps/static/js/reducers`

$ `mkdir apps/static/js/stores`

$ `touch apps/static/js/components/ReactIndex.js`

$ `touch apps/static/js/stores/indexStore.js`

$ `touch apps/static/js/reducers/indexReducer.js`


#### **apps/static/js/components/ReactIndex.js**
This is a 'Hello World' React/Redux component. 

{% highlight django %}{% raw %}
import React from 'react';

export class ReactIndex extends React.Component {

    render() {
        return (
            <div>
                <h1>This is a working react component.</h1>
            </div>
        );
    }
}

export default ReactIndex;
{% endraw %}{% endhighlight %}


#### **apps/static/js/reducers/indexReducer.js**
A basic Reducer.

{% highlight django %}{% raw %}
import { combineReducers } from 'redux';
import { routerReducer } from 'react-router-redux';

const indexReducer = combineReducers({
    routing: routerReducer,
});

export default indexReducer;
{% endraw %}{% endhighlight %}


#### **apps/static/js/stores/indexStore.js**
A basic store.

{% highlight django %}{% raw %}
import { compose, applyMiddleware, createStore } from 'redux';

import indexReducer from '../reducers/indexReducer';

const enhancer = compose(
    applyMiddleware(
    ),
);

const store = createStore(indexReducer, {}, enhancer);

export default store;
{% endraw %}{% endhighlight %}










