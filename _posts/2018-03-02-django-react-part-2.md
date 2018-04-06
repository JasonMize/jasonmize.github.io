---
layout: post
title: Django + React (part 2 of 4)
---

### **[Back to Part 1]({% post_url 2018-03-01-django-react-part-1 %})**



## Step 3: Start Your Servers

From the root directory of your Django project start your Django server.

$ `./manage.py runserver 0.0.0.0:8000`

In a separate terminal window navigate to our new react app.

$ `cd frontend`

Start our NPM server.

$ `npm start`

In your browser see your default react app at http://localhost:3000/



## Step 4: Bootstrap!

Install Bootstrap and React-Bootstrap.  Bootstrap will add regular access to bootstrap styles.  The biggest difference being that in React you have to reference styles in the html by using "className" instead of "class".

React-Bootstrap provides a bunch of awesome pre-made react components that have bootstrap styling built in.  Navbar, Footer, Panel, Button, etc.  Similar to the way moving inline styles out of your HTML and into your CSS will allow you to edit many elements from a single line, React-Bootstrap components (and React components in general) will let you all of your Buttons (for example) to be uniform in the way they look and act.

In your NPM server:

$ `npm install --save bootstrap@3`

$ `npm install --save react-bootstrap`

#### **src/index.js**
{% highlight html %}

import 'bootstrap/dist/css/bootstrap.css';

{% endhighlight %}




### **[Continued in Part 3]({% post_url 2018-03-03-django-react-part-3 %})**


