+++
title = "Resource Oriented Computing"
date = "2010-03-29T12:01:00-07:00"
tags = ["NetKernel"]
categories = ["resource oriented computing"]
menu = ""
bannerSmall  = "http://www.resourcefulcomputing.com/images/nothing.png"
bannerMedium = "http://www.resourcefulcomputing.com/images/nothing.png"
banner       = "http://www.resourcefulcomputing.com/images/nothing.png"
originalurl  = "https://resourcefulcomputing.wordpress.com/2010/03/29/resource-oriented-computing/"

[author]
	url = "http://www.randolphkahle.info"
	name = "Randolph Kahle"

[dates]
	published = "2010-03-29T12:01:00-07:00"
	modified =  "2016-02-02T12:00:00-07:00"

[publisher]	
	name = "Resourceful Computing"
	logoUrl = "/images/nothing.png"
+++ 

Resource Oriented Software allows us to build simpler software.
What exactly is ROC? 

The following diagram provides a high-level view of ROC.


<figure>
  <figcaption>Resource Oriented Computing Abstraction</figcaption>
<svg 
  width="360px" 
  height="180px" 
  viewBox="0 0 360 180" 
  version="1.1" 
  xmlns="http://www.w3.org/2000/svg" 
  xmlns:xlink="http://www.w3.org/1999/xlink" 
  xmlns:sketch="http://www.bohemiancoding.com/sketch/ns">
    <!-- Generator: Sketch 3.5.1 (25234) - http://www.bohemiancoding.com/sketch -->
    <title>Artboard 1</title>
    <desc>Created with Sketch.</desc>
    <defs></defs>
    <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd" sketch:type="MSPage">
        <g id="Artboard-1" sketch:type="MSArtboardGroup" stroke="#979797">
            <rect id="Rectangle-1-Copy-2" fill="#CAA6BC" sketch:type="MSShapeGroup" x="40" y="20" width="140" height="70" rx="20"></rect>
            <rect id="Rectangle-1-Copy" fill="#A9CCA7" sketch:type="MSShapeGroup" x="180" y="90" width="140" height="70" rx="20"></rect>
        </g>
    </g>
</svg>
</figure>

In ROC, requests are made for information by a Requestor by issuing a request into a Space. 
Information is modeled as Resources and each resource has one or more Identifiers. 
The diagram shows a resource request for a member of a Resource Set being resolved to an Endpoint. 
When the endpoint evaluates the request it returns an immutable copy of the information in the form of a Representation.

This diagram illustrates how the World Wide Web works! 
A browser (requestor) issues a request for information and identifies that information with a URI (the URL of the resource). 
The domain name service (DNS) resolves the identifier (within a global space) to a web server (endpoint) and then the browser
issues a request directly to the endpoint for evaluation. 
The web server (endpoint) evaluates the request and returns a copy of the current information in the form of a web page (representation).

The ROC abstraction becomes particularly interesting when implemented as a very small software 
platform to support software applications development. The resulting software takes on characteristics of the Web, 
such as scaling, flexibility and malleability.

In the next several posts we will explore many of the facets of ROC and see how they impact the 
quality and simplicity of software.