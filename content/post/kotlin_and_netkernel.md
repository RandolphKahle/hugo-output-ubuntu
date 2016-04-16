+++
title = "Kotlin and NetKernel"
description = "An exploration of Kotlin as a programming language for NetKernel endpoints"
date = "2013-06-14T14:42:00-07:00"
tags = ["kotlin", "language", "NetKernel"]
categories = ["programming"]
menu = ""

banner       = "images/kotlin/kotlin-logo-200x200.png"
bannerSmall  = "images/kotlin/kotlin-logo-200x200.png"
bannerMedium = "images/kotlin/kotlin-logo-200x200.png"

[rdfarticleimage]
  url = "http://www.resourcefulcomputing.com/images/kotlin/kotlin-logo-200x200.png"
  height = "256"
  width = "256"

[dates]
	published = "2013-06-14T14:42:00-07:00"
	modified =  "2016-02-12T12:00:00-07:00"
+++ 

> This article was written more than three years before the pending release of Kotlin.
> The Kotlin language has evolved and I have removed the NetKernel modules from GitHub while
> I await the final release. I will provide a link to an updated post in the preface of this post
> when the dust settles. -- Randolph Kahle, February 12, 2016.

Kotlin is a new programming language from JetBrains, the publishers of IntelliJ 
and other development tools. Their goal is to make Kotlin an industrial strength 
replacement for Java.

I am intrigued because Kotlin has type safety, modern language features, 
very good IDE support, and can interoperate easily with existing Java programs and libraries.

To facilitate experimentation, I developed a NetKernel module `urn:org:netkernelroc:lang:kotlin` 
and made it available on the NetKernelROC community Apposite repository. 
The current release (version 0.1.0 on June 14, 2013) includes Kotlin M5.2 (the 0.5.748 build). 
This is a Kotlin milestone release. 
While I have found the NetKernel code I've written with it to run just fine, you should probably 
consider this experimental. 
I will update the NetKernel module as Kotlin releases are published. 
Once you connect your NetKernel instance with the NetKernelROC Apposite repository you should be 
automatically notified of the updates.

The NetKernel Kotlin module is hosted on GitHub at 
<https://github.com/netkernelroc/urn.org.netkernelroc.lang.kotlin> 
Currently the module contains Kotlin JAR files with language runtime support. 
My goal is to also include Kotlin code (in the form of classes and extension functions) 
to extend internal NetKernel code such as the NKF API and HDS to make programming easier 
than when using Java. 
If you are interested in working at this level, then you should fork and clone a copy of 
this module from GitHub and propose updates. 
You will find that I have written two Kotlin based HDS builders. 
The first is a Kotlin replacement for HDSBuilder (facilitated by the IntelliJ "convert Java code to Kotlin" service). 
The second simply wraps the NetKernel HDSBuilder and delegates to it. 
I wrote both because I'm not certain which will be the better approach.

My first experimentation with a NetKernel module that uses Kotlin can be found in the 
NetKernel module `urn:org:netkernelroc:util:file`, also hosted on GitHub, at 
<https://github.com/netkernelroc/urn.org.netkernelroc.util.file>. 
This module, also in a very early release, contains the service `active:fileList` 
which returns a list of files in a specified directory. 
It uses the Kotlin replacement for HDSBuilder. 
If you look at the module `urn:org:netkernelroc:util:file:test` you will find an experimental Kotlin DSL 
to facilitate the creation of XUnit testlist.xml resources.

My hope is that Kotlin will turn out to be a good language to standardize on for NetKernel system development. 
I experimented with Groovy, but found it frustrating (that's another discussion). 
I wrote a Scala language module for NetKernel a while ago, but Scala programs seem too opaque.

I released this module on GitHub and through the NetKernelROC community to make it easy 
for anyone in the community to join me in exploring Kotlin.

