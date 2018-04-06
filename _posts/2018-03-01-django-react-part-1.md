---
layout: post
title: Django + React (part 1 of 4)
---


## The Overall Plan:

Take an existing single-page-app built with Django 1.11 and AngularJS and replace AngularJS with React.

Use Create React App to manage the React installation.

Add Sass, Bootstrap, and other goodies.




### Instructions:

Any line preceeded by a "$" is intended as a terminal command. For example:

$ `line you should copy into terminal`

Lines to insert into files will be shown as a code snippet. For example:

#### **filename:**
{% highlight html %}
line you should copy into filename 
{% endhighlight %}



# Start:

## Step 1: In Git We Trust

Work on a new git branch based on our master branch.  This is a major shift, let's keep the dev and master branches free to handle anything that comes up while we're working.  

Create a new branch from your 'master' branch.

$ `git branch react`

Move to the new branch.

$ `git checkout react`

Add the new branch to github.

$ `git push --set-upstream origin react`


## Step 2: Win The Easy Way

Install React via Create React App. It will handle all of our dependencies and greatly simplify our configuration.  This is a very good thing.  

In your terminal navigate to the project root and install create-react-app. 

$ `npm install -g create-react-app`

Create your react app - naming it 'frontend' is fairly common.

$ `create-react-app frontend`

Your new directory structure:

    DjangoProjectName

        bunch of folders (api, core, conf, media, etc.)

        frontend

            node_modules (folder)

            public (folder)

            src (folder)

            .gitignore

            package.json

            README.md

We've installed React!  Ta-Da!  Let's make a commit and then we'll dive into the configuration.

First see what has changed on our branch.

$ `git status`

Stage the changes to be pushed to github.

$ `git add .`

Double check that we've staged the right stuff.

$ `git status` 

Add a commit message

$ `git commit -m "initial install of react"`

Commit!

$ `git push`


### **[Continued in Part 2]({% post_url 2018-03-02-django-react-part-2 %})**
