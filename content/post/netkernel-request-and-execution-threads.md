+++
title = "NetKernel request and execution threads"
description = "An exploration of the difference between physical and logical concurrent request processing"
date = "2012-09-18T05:04:00-07:00"
tags = ["netkernel"]
categories = ["programming"]
menu = ""
bannerSmall  = "images/netkernel/netkernel-logo-256x256.png"
bannerMedium = "images/netkernel/netkernel-logo-256x256.png"
banner       = "images/netkernel/netkernel-logo-256x256.png"
originalurl  = "https://resourcefulcomputing.wordpress.com/2012/09/18/netkernel-request-execution-and-threads/"

[rdfarticleimage]
  url = "http://www.resourcefulcomputing.com/images/netkernel/netkernel-logo-256x256.png"
  height = "256"
  width = "256"

[dates]
	published = "2012-09-18T05:04:00-07:00"
	modified =  "2016-02-07T12:00:00-07:00"
+++ 

One aspect of NetKernel that confuses developers is the relationship between a request and a thread.

A request is a demand placed on a software system. 
A request could be triggered by a user interacting with the system, 
a Cron timer reaching a particular time, or another system in need of information. 
At the completion of request processing either the state of the system has changed, 
an information representation is returned, a request is made of another system, or some combination of these occurs.

A thread is an abstract execution context which is implemented by a real instruction counter and registers in a physical CPU core. 
The Java thread abstraction is simple and relatively straight forward to use. 
A program (or container) can set up a pool of threads which can be executed on one, two, or more physical CPU cores. 
Because a thread is an abstraction, the physical level execution can employ highly optimized techniques which remain invisible to the developer.

The thread abstraction has worked really well. As we have progressed from single core computers to two and four cores, 
developers have used a model of “processing a request on a thread” to build reasonably performant systems. 
Complexities arise, however, when writing code running on multiple threads that mutates state. 
Doug Lea’s early work and the later incorporation of this thinking into the Java concurrency libraries 
has made multi-threaded development and state mutation tenable.

For most developers, this is the “state of the art”. 
Some have used more advanced patterns to achieve very high concurrency and performance. 
There are also libraries, frameworks, etc. that support these more sophisticated patterns, such as Node.js. 
Also of note is Rich Hickey’s brilliant work with the data structures in Clojure.

NetKernel provides a much simpler abstraction.

In NetKernel the concept of a request and thread are completely separate. 
There is no notion of “processing a request on a thread”. 
Instead, a request (implemented as a Java object, which, with other associated objects, 
contains the complete processing context) is issued and the client waits for the requested information to be returned. 
While a thread actually does the processing of this request, that abstraction is hidden, 
just as the optimized physical execution of a thread is hidden from a Java developer. 
The following code show how a request is created:

	INKFRequest request = context.createRequest("res:/helloworld.txt");

and this code tells the kernel that it should process the request:

	context.issueRequest(request);

Notice the method name – `issueRequest()`. 
This is similar to typing in a URL into a browser and requesting information. 
In both cases there is no notion of a thread – this is just a request for information (in our case for the resource “`res:/helloworld.txt`”). 
Of course these two methods calls must execute on a thread – it just doesn’t matter which thread. Requests processing and threads are completely orthogonal.

Compare this with traditional Java multi-thread coding. 
To create a new thread of execution, several things must happen: an instance of a Java object that 
implements the Runnable interface is created, the runnable is provided to a new Thread, and the thread is instructed to start:

	Runnable runnable = new MyRunnableClass();
	Thread runner = new Thread(runnable);
	runner.start();

In this model a thread is a visible abstraction – the exact opposite with NetKernel. 
In fact, threads and thread synchronization are primary and very necessary concerns for a developer.

What does this mean?

The implications of using the NetKernel Resource Oriented Computing abstraction are interesting to consider, 
especially as it relates to the emergence of 8-core, 16-core, 64-core, etc. computers and the importance of distributed computing. 
If a developer builds a system that uses ROC requests then the number and location of the threads that execute the work are not even visible.
They are decoupled from the request processing code. 
This means that the size of worker thread pools, the number of cores, and whether they are local or remote, 
are not coding concerns – they are architectural and deployment / configuration decisions. 
And all of these decisions can be changed for an already built application independent of the code.
