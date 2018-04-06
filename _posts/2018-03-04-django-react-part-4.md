---
layout: post
title: Django + React (part 4 of 4)
---

### **[Back to Part 3]({% post_url 2018-03-03-django-react-part-3 %})**



## Step 6: Routing!

We'll use React-Router to organize the paths to our various React components.  

From your NPM server install react-router.

$ `npm install --save react-router-dom`

#### **src/index.js**
#### DELETE:
{% highlight html %}

import App from './App';
import registerServiceWorker from './registerServiceWorker';

ReactDOM.render(<App />, document.getElementById('root'));
registerServiceWorker();

{% endhighlight %}


import ReactDOM from 'react-dom';
import { BrowserRouter as Router, Route } from 'react-router-dom';


#### **src/index.js**
#### ADD:
{% highlight html %}

ReactDOM.render(
  <Router>
    <div>
    </div>
  </Router>,
  document.getElementById('root')
);

{% endhighlight %}

Delete src/App.js - we aren't referencing it any more.

$ `rm src/App.js`



## Step 7: Components!

Up to now everything has been generic as we get React set up.  A number of my examples from here on will be code from my actual project.  

Create a folder to hold our components.

$ `mkdir src/components`

In AngularJS I have a file: comic.module.js.  It handles all of my angular component routing.  Its code looks like this:

{% highlight html %}

const ComicModule = angular.module('comic', [
    angularResource,
])
    .config(($resourceProvider) => { })
        .factory('comicAPIService', comicAPIService)
        .component('comicPage', comicPageComponent)
    ;

{% endhighlight %}

We need equivalent component routing in our React files. 

Create src/components/Comic.js

Add routing to our new component.

#### **src/index.js**
{% highlight html %}

import Comic from './components/Comic';

{% endhighlight %}

Inside <div></div>:
{% highlight html %}

<Route path="/comic" component={Comic} />

{% endhighlight %}

In AngularJS our component has 3 parts: the component, the controller, and the html.  React combines all of those onto the same 'component' page.

Create the skeleton of the component.

#### **component/Comic.js**
{% highlight html %}

import React, { Component } from 'react';

class Comic extends Component {
  constructor(props) {
    super(props);

    this.state = {
    };
  }

  render () {
    return (
      <div></div>
    );
  }
}

export default Comic;

{% endhighlight %}

Inside our class, functions (for example 'constructor') will handle our logic.  Inside 'return' in the 'render' we'll put our HTML. 


# FIN





