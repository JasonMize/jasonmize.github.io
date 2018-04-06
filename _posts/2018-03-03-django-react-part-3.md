---
layout: post
title: Django + React (part 3 of 4)
---

### **[Back to Part 2]({% post_url 2018-03-02-django-react-part-2 %})**



## Step 5: Sass!

Sass is a delightful extension to the CSS language.  It allows the nesting of CSS classes and selectors and the variables.  Can't live without it. 

Stop your NPM server via "ctrl + c" and then install Sass.

$ `npm install --save node-sass-chokidar`

Create-React-App can't read Sass. We need to convert our Sass files into a standard CSS file. Then we'll want to watch all of our Sass files and keep the CSS file up to date whenever we make changes. 

#### **package.json**
In the "scripts" section:
{% highlight html %}

"build-css": "node-sass-chokidar --include-path .src/ --include-path ./node_modules src/ -o src/",

"watch-css": "npm run build-css && node-sass-chokidar --include-path ~./client/js/sass/ --include-path ./src --include-path ./node_modules src/ -o src/ --watch --recursive",

{% endhighlight %}


Install npm-run-all so we can run multiple commands in our npm server with a single command.

$ `npm install --save npm-run-all`

Add "start-js to our npm build.

#### **package.json**
"scripts":
{% highlight html %}

"start-js": "react-scripts start",

{% endhighlight %}

Change "start" command so when we start our server it will build and watch our CSS.

#### **package.json**
"scripts":
{% highlight html %}

"start": "npm-run-all -p watch-css start-js",

{% endhighlight %}

Change "build" command so when we build a file for production it will include our CSS and Sass. 

#### **package.json**
"scripts":
{% highlight html %}

"build": "npm run build-css && react-scripts build",

{% endhighlight %}

Rename src/app.css to src/app.scss.

$ `mv App.css App.scss`

Add a folder for all of our Sass files.

$ `cd frontend/src`

$ `mkdir styles`

$ `mkdir styles/images`

$ `mkdir styles/scss`

Move App.css and App.scss into the styles folder.

$ `mv App.css styles/App.css`

$ `mv App.scss styles/App.scss`

Start up NPM server.

$ `npm start`

Now our NPM server will watch "App.scss" and update "App.css" to match it.  

We need to tell React where to look for our styles.

#### **src/index.js**
#### DELETE:
{% highlight html %}

import './index.css';

{% endhighlight %}

#### ADD:
{% highlight html %}

import './styles/App.css';

{% endhighlight %}

$ `rm index.css`

Divide your CSS into individual files so they are shorter and more categorized.  For example: layout/header/footer/etc.

These are the files that we will "include" in our src/App.scss file.  The naming convention for files that are included is to have an '_' in the beginning of the file name. I created the following:

    src/styles/_layout.scss
    src/styles/_comic.scss
    src/styles/_header.scss
    src/styles/_footer.scss
    src/styles/_about.scss

Edit App.scss and delete everything in the file.  These were generic styles added by create-react-app.  We're going to import our new Sass files instead.

#### **App.scss**
{% highlight html %}

@import './scss/_layout.scss';
@import './scss/_comic.scss';
@import './scss/_header.scss';
@import './scss/_footer.scss';
@import './scss/_about.scss';


{% endhighlight %}






