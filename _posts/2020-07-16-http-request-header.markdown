---
layout: post
title:  "Shot my foot with HttpRequestHeaders"
date:   2020-07-16 21:11:00 +0530
categories: coding
---

Shot my foot with HttpRequestHeaders. Not exactly, someone else shot someone else's foot. I am just a by watcher. Hehe!

Lets consider a nice microservice scenario. We have an UI, Service A and Service B.

UI --> Service A --> Service B

Mind you, its so basic, I don't want go into the whole microservice mesh scenario with whole API hell. Lets assume this is the service we have.

Our product we have a lot of custom headers that flies around with each request. During some point in development it grows to 20-ish `X-Custom-*` headers. Don't ask why. Its the way it is. 

One good guy thought, rather than having these 20 headers pass around let us put a unique request id per user and have this 20 header like values in the redis. All is well, it saves us few kbs of data.

As a aside this is a part of code looks like in Service B which is problematic on its own.

In the Service B we have this little line. 

{% highlight csharp %}
var customProductId = httpContextAccessor.HttpContext.Request.Headers["X-Custom-ProductId"];
int productId = Convert.ToInt32(customProductId); // Oh no no no.. You should not do this, particularly here

// do some work with the product id
{% endhighlight%}

So when the Service A calls the Service B using a httpClient they usually do something like this

{% highlight csharp %}

// Set the native headers associated to incoming request
foreach (KeyValuePair<string, string> item in httpContextAccessor.HttpContext.Request.Headers)
{
    httpRequestMessage.Headers.Add(item.Key, item.Value);
}

// set the custom headers using the request id
var requestId = httpContextAccessor.HttpContext.Request.Headers["X-Custom-RequestId"];
foreach (KeyValuePair<string, string> item in GetCustomHeaders(requestId))
{
    httpRequestMessage.Headers.Add(item.Key, item.Value);
}

{% endhighlight%}

Bang, here comes the problem. What if the UI sends the request with the request id and also the 20-ish headers. They did not take that into account. So that messed up. Always try to write a foolproof code. Gaurd against all situations. In the above code each header may get added twice, When a same key is added multiple times in HttpRequestHeaders it will concat them with a comma. 

In the Service B the nice `Convert.ToInt32` blow up.

So whats the solution. `Int.TryParse` and `!httpRequestMessage.Headers.Contains()`.

What to say? 🤒

