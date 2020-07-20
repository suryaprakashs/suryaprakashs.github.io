---
layout: post
title:  "Thread Pool Inc., - Part 1"
date:   2020-07-19 15:05:00 +0530
categories: .net-core threading
---
> This post is (maybe) first of series of posts that covers thread pool and related topics

#### Thread Pool

Let us consider ***Thread Pool Inc.,*** a nice mid-sized industry who manufactures cars. That company has 500 employees, but all are on paid leave because of low demand on cars. But whenever order for car comes in an employee is assigned to assemble a car. (For simple analogy let us assume that one person is enough to assemble a car, but in real life it has a more complex assembly line process)

So if another order for a car comes in the next employee is assigned to assemble a car, if the previous one is still working in the process of assembly. Then after that another order comes in and another employee is assigned to that order. This process continuous till all the employees are occupied.

Once all the employees are occupied with work but still more order for a car comes, they will start to hire more employees. But the hiring process is not that easy, it will take some time. Put on the advertisement. Schedule an interview. Select the candidate and do on boarding. While the management is looking for new people, at the same time if one of their employees finished their work then the work to assemble car is assigned to that employee. But if none of the employees available throughout the hiring process they call the one the selected candidate for on boarding.

#### Enter Thread Pool!!

With this same analogy against the ***Thread Pool Inc.,*** each web app is created with the thread pool and the thread pool is instantiated with the minimum number of threads which usually same as the core count. Till the thread count is below the [minimum thread count]((https://docs.microsoft.com/en-us/dotnet/api/system.threading.threadpool.setminthreads?view=netframework-4.7.2#remarks)), whenever new request comes in the thread pool creates a new thread immediately without any delay and assign the work to that thread.

Once the number of threads reaches minimum count then the thread creation is stalled by the [thread pool algorithm](https://mattwarren.org/2017/04/13/The-CLR-Thread-Pool-Thread-Injection-Algorithm/). Just like the hiring employees which takes some time. Usually in the thread pool the creation of new thread is stalled by *500ms*. While in the wait mode if any of the worker threads are freed-up of work then the pending work is assigned to the freed up thread. Even after the wait time if no thread is available then a new thread is spawned to do the work.

Thread pool is responsible for creating new threads when needed and to retire the threads when the are no longer necessary. The thread pool algorithm is pretty much enhanced to take care of all the workload scenarions. It is advised to leave the default values of the thread pool.

But the minimum and maximum thread count can be configured using the `ThreadPool.SetMinThreads` and `ThreadPool.SetMaxThread` methods. Too many threads will also degrade the system performance because of the too many thread context switching.

To be continued!!!
