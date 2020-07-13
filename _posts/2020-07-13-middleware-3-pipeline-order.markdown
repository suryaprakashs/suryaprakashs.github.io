---
layout: post
title:  "Identity Server and Status Code: 401; Unauthorized"
date:   2020-07-13 19:48:27 +0530
categories: c# .net-core identity-server
---

<h2> Status Code: 401; Unauthorized </h2>

I did a facepalme when I find the solution to this problem. This is not something straight forward. Atleast for me.

I was tasked with writing authentication module.

For an enterprise application security is the first and foremost of features and there by the Authentication comes along.

To enable the authentication I used the Identity Server 4. It is fairly simple to configure if you know how to  use it. But for me it took around 4 hours just to setup the identity server to provide the JWT alone. All is well. 

We have the UI which talks with the identity server on login get the JWT and pass it as Auth header on each request.

On the backend .net core app we configure the service like this in the `ConfigureServices`

{% highlight csharp %}

services.AddAuthentication("Bearer")
.AddJwtBearer("Bearer", options =>
{
    ...
});

{% endhighlight %}

And in the `Configure` method middleware we register the middleware

{% highlight csharp %}

app.UseAuthentication();

{% endhighlight %}

Thats all it is required.

So let back up a minute. Its the simplest process to setup and configure the identity server. But the real problem comes here. If you failed to register the `UseAuthentication` but the services are configured you will end up having the `Status Code: 401; Unauthorized` for all request. Well I am smart ass to enable the `UseAuthentication` alone based on a switch.

<b>Make sure you have both the AddAuthentication and UseAuthentication enabled or disabeld at the same time.</b>