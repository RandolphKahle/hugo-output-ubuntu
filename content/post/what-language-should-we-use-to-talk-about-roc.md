+++
title = "What language should we use to talk about ROC?"
description = "A discussion about a language to describe Resource Oriented Computing"
date = "2012-09-01T07:29:00-07:00"
tags = ["language"]
categories = ["complexity"]
menu = ""
bannerSmall  =  "images/cloud-100x100.png"
bannerMedium =  "images/cloud-100x100.png"
banner       =  "images/cloud-100x100.png"
originalurl  =  "https://resourcefulcomputing.wordpress.com/2012/09/01/what-language-should-we-use-to-talk-about-roc/"

[rdfarticleimage]
  url = "http://www.resourcefulcomputing.com/images/roc/roc-space-132x176.png"
  height = "200"
  width = "200"

[dates]
	published = "2012-09-01T07:29:00-07:00"
	modified =  "2016-02-07T12:00:00-07:00"
+++ 


Humans thrive on communication and we have invented many different forms. 
For example – music, dance, written communication which includes novels, poetry, short stories, essays, blogs, etc., as well as visual forms such as paintings. 
For particular domains, such as mathematics, we have developed symbolic languages that allows us to succinctly express our ideas. 
In the field of information systems we have different ways of expressing ourselves from the highest altitude view to executable code.

Resource Oriented Computing is an approach to conceive and implement information systems. 
It is natural to start with a familiar language to discuss the abstraction and then extend the language when required. 
In the eight years I have been working with Resource Oriented Computing I have wrestled with this language. 
It is frustrating because the concepts behind ROC are very simple and yet, we do not have an agreed upon set of words or notational system. 
I now think that the ROC community needs a new language.

I realize that the language we use helps us articulate out thoughts and shapes our thinking by facilitating and constraining. 
A language facilitates thought by making available words or symbols that have an agreed upon meaning and which allow us to easily note what we want to communicate. 
If that language lacks words or symbols for a particular new thought, 
then it constrains us and it forces us to either extend the language or take more time to build a line of reasoning.

Which languages have we tried? I have used pictures with address spaces (that look like clouds), 
endpoints (that look like rounded rectangles), and requests (that look like arcs). 
These symbols were first designed by Peter Rodgers for the NetKernel documentation. 
In conversations with Peter he has shown me a more mathematical and symbolic way to describe ROC. 
Tom Hicks has shared his ideas about a Lisp-like notation. 
I have experimented with a DSL to extend the Groovy programming language. 
The NetKernel explorer tool uses a graphical rendering. 
The NetKernel nCode composition tool presents a graphical view of endpoints and relationships but ignores address spaces. 
And finally, to create a running NetKernel system, one uses a text editor to create an XML document.

My final complaint about our current state is that no language supports a discussion about information flow through a NetKernel system. 
All of the languages are about the composition of spaces, endpoints, and request / response processing. 
What actually happens in a NetKernel system is that information flows through the ROC architecture. 
Isn’t that the whole point of an information system? If so, why is that not the primary focus of a language?

I leave this as a challenge to the NetKernel / ROC community. 
Let us create a new language for ROC.
