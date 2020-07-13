---
layout: post
title:  "Starting and ending handlebars do not match"
date:   2020-07-13 19:48:27 +0530
categories: c# .net-core handlebar
---

I was going through the handle bar setup to compile and render a html template for email services. It was working well. But all of a sudden I got this weird error. 

```starting and ending handlebars do not match```

At first it does not seem much, as the message clearly states that the starting and ending handlebars don't match. I checked all the starting and ending curly braces, but they are all perfectly balanced. 

So I did a guess work. <b>Illegal characters in html.</b> Clean them up, you have good running template.