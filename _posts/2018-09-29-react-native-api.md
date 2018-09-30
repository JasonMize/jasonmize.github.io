---
layout: post
title: React Native - API Calls
---

## Fetch
React-Native uses the Fetch API to handle API calls.  I'm going to show a few examples of how to handle various API calls.

[React-Native docs about Fetch](https://facebook.github.io/react-native/docs/network.html){:target="_blank"}

[MDN docs about Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API){:target="_blank"}

## Get
Get requests are very straightforward using Fetch.  You include the url of the API and then handle the response.  

First we include the url. 
{% highlight html %}
  fetch("http://fakewebsite.com/example")
{% endhighlight %}

Then we convert our response to JSON data.  
{% highlight html %}
  .then(exampleResponse => exampleResponse.json())
{% endhighlight %}

We'll want to catch any errors that occur.
{% highlight html %}
  .catch((error) => {
    console.error('Example error: ', error);
  })
{% endhighlight %}

Then we assign the response to our components state.  
{% highlight html %}
  .then(exampleJSON => {
    console.log('Example response in JSON format: ', exampleJSON);

    this.setState({
      paramA: exampleJSON.paramA,
      paramB: exampleJSON.paramB
    });
  });
{% endhighlight %}

Here's the whole enchilada: 
{% highlight html %}
 fetch("http://fakewebsite.com/example")
 .then(exampleResponse => exampleReponse.json())
 .catch((error) => {
   console.error('Example error: ', error);
 })
 .then(exampleJSON  => {
   console.log('Example response in JSON format: ', exampleJSON);

   this.setState({
     paramA: exampleJSON.paramA,
     paramB: exampleJSON.paramB
   });
 });
{% endhighlight %}

## Post
Posting to an API using Fetch is similar to Get.  We're still going to have a url.  We'll convert data to JSON format, catch any errors, and then handle the response from the API.

In addition, we'll have information about our post that we'll need to send. This is info that the API will need in order to process our request and send us back a response.  

First, the url.  
{% highlight html %}
  fetch("http://fakewebsite.com/example")
{% endhighlight %}

Next the information for the API.  We'll include:
  
  * 'method': to let the API know to expect a 'POST'
  * 'headers': to let the API know that it should expect our data in JSON format
  * 'body': this is where we send any parameters the API needs to process our post
{% highlight html %}
  {
    method: 'POST',
    headers: {
      "Accept": 'application/json',
      "Content-Type": application/json
    },
    body: JSON.stringify({
      paramA: this.state.paramA,
      paramB: this.state.paramB
    })
  }
{% endhighlight %}

Then we'll handle the response - the info that the API sends back to us.
{% highlight html %}
  .then(exampleResponse => exampleResponse.json())
{% endhighlight %}

We catch any errors.
{% highlight html %}
  .catch((error) => {
    console.error('Example error: ', error);
  })
{% endhighlight %}

Then we assign the response to our components state.  
{% highlight html %}
  .then(exampleJSON => {
    console.log('Example response in JSON format: ', exampleJSON);

    this.setState({
      paramA: exampleJSON.paramA,
      paramB: exampleJSON.paramB
    });
  });
{% endhighlight %}

Put it all together and it looks like this:
{% highlight html %}
  fetch("http://fakewebsite.com/example", {
    method: 'POST',
    headers: {
      "Accept": 'application/json',
      "Content-Type": application/json
    },
    body: JSON.stringify({
      paramA: this.state.paramA,
      paramB: this.state.paramB
    })
  })
  .then(exampleResponse => exampleResponse.json())
  .catch((error) => {
    console.error('Example error: ', error);
  })
  .then(exampleJSON => {
    console.log('Example response in JSON format: ', exampleJSON);

    this.setState({
      paramA: exampleJSON.paramA,
      paramB: exampleJSON.paramB
    });
  });
{% endhighlight %}


## IOS vs. Android
In local development your IOS simulator will only work if you make your url point to localhost.
{% highlight html %}
  http://localhost:5000
{% endhighlight %}

In local development your Android simulator will only work if you give your url an explicit address.
{% highlight html %}
  http://10.0.2.2:5000
{% endhighlight %}


# FIN