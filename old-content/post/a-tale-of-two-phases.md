+++
title = "A Tale of Two Phases"
date = "2010-04-01T16:03:00-07:00"
tags = []
categories = []
menu = ""
bannerSmall = "http://www.resourcefulcomputing.com/banners/rust-logo-blk-80x80.png"
bannerMedium = "http://www.resourcefulcomputing.com/banners/rust-logo-blk-400x400.png"

[author]
	url = "http://www.randolphkahle.info"
	name = "Randolph Kahle"

[dates]
	published = "2010-04-01T16:03:00-07:00"
	modified =  "2016-02-07T12:00:00-07:00"

[publisher]	
	name = "Resourceful Computing"
	logoUrl = "/banners/placeholder.png"
+++ 



Thoughts about using Resource Oriented Computing to build simpler software
ROC – A Tale of Two Phases

with 3 comments

In the resource oriented computing (ROC) abstraction, resource requests are processed in two phases – the Resolution and Evaluation phase. In the resolution phase a search is conducted to find an endpoint that will resolve the request. During the evaluation phase, the endpoint found in the resolution phase is asked to perform whatever computation it requires to create and return a representation of the requested information.

Each request triggers these two phases, which means that a subsequent request for a particular information identifier could resolve to a different endpoint. This is a critical feature of ROC that we will argue later provides architectural flexibility.

The following diagram illustrates the resolution phase. The endpoint acting as a requestor issues a request into its containing space and a  search is conducted to find the first endpoint that resolves the request. In the diagram, the search reaches the Import endpoint which forwards the search into a related space. Within that space we see an endpoint finally resolving the request, which ends the search.

<figure>
</figure>

The next diagram illustrates the start of the evaluation phase. From the perspective of the endpoint acting as the requestor, a temporary relationship is established with the resolved endpoint and the request is sent for evaluation.

<figure>
</figure>

When the endpoint creates a representation, it returns it to the requestor and the relationship between the requestor and the endpoint is forgotten.

<figure>
</figure>

The cycle of searching for an endpoint followed by request evalution and then amnesia repeats for each request. This is fundamentally different from what most of us are used to. Most software systems work hard to optimize run time efficiency by remembering relationships instead of forgetting about them. Traditional compiled languages such as C, C++, Pascal, etc. generate compiled object libraries that are linked together by a linker. In Java, a Classloader brings class definitions into memory and created objects are linked through method address resolution tables when methods are first called. If we want to introduce dynamic relationships in these systems, we have to work at it and introduce points of indirection. Some languages make this easier, such as Groovy and its Meta Object Protocol (MOP); however, what is different with ROC is that the separation of a requestor and a service endpoint is fundamental.

The implication is that with ROC, we can create software that is reconfigurable at run time. At any level of an application architecture. In fact, all facets and any level of a system can be reconfigured dynamically. In the next post we will look at an implementation of ROC and explore why its performance is not all that bad while future posts will examine how we can harness this flexibility to our advantage.