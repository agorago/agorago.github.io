---
title: "WeGO -  The In Process Service Mesh & Application Development Framework "
keywords: homepage
tags: [getting_started, introduction]
sidebar: wego_sidebar
permalink: index.html
summary: These brief instructions will help you get started quickly with the WeGO framework.
---

{% include note.html content="An introduction to WeGO." %}

# Motivation 
Go Lang has a great ecosystem with multiple frameworks and tools. One of the professed objectives behind creating the language is the creation of a set of tools and more importantly creating a mindset around using them. There exists a lot of literature that defines what constitutes idiomatic Go code. 

The [Go Programming Language](https://www.gopl.io/) book best explains it as follows:

> The Go project includes the language itself, its tools and standard libraries, and last but not least, a cultural agenda of radical simplicity.

The idea to develop a framework came from a recent engagement where I was looking to see how we can enable developers of a start up to become extremely productive in developing a complex application. As a firm believer of [Occam's Razor](https://en.wikipedia.org/wiki/Occam%27s_razor) I believe in simplicity of design to solve even complex problems. 

Go seems to support that thinking. As Rob Pike puts it, "Complexity is multiplicative". 

<a name='thewhat'/>
# Why do we need a framework?
In my experience of working almost three decades in application development, I have observed a few things that most organizations struggle with:

* Very often programmers write a ton of similar boiler plate code to achieve logging, monitoring, and a myriad of other horizontal concerns. They implement them by duplicating code rather than re-using it. This is because it is difficult to find a framework that incorporates them seamlessly.

* Design paradigms like modularization, dependency injection, testability, SOLID principles etc. very often take a back seat since programmers are busy solving functional problems. This lapse tends to manifest itself at a later stage leading to substantial re-writes

* Standardization is another illusory pursuit. Many people use different open source frameworks. Sometimes, they use the same technology differently in the absence of an established standard. 

* DevOps requires a lot of effort. This includes the time spent in writing build and deployment scripts, achieving versioning, achieving docker builds etc. Programmers tend to focus on these rather than spending time creating functionality.

* Developers also spend a lot of time in exposing transports such as HTTP, writing JSON encoders and de-coders and stitching middleware with the services. 

* Similarly concerns like circuit  breaking, rate limiting etc. tend to get de-prioritized. They are implemented as a kludge much later.

* Very often, there is not much of a distinction between an architect role and a developer role. Application developers must concentrate on one important thing -  i.e. understand the domain and write application code. Architects must choose the frameworks, integrate them together, establish standards and write framework code.

So, my idea was to build a RAD (Rapid Application Development) framework to get all developers immediately productive. The RAD must also establish standards, conventions and provide common services required throughout the estate.  After a little consideration, it was evident that the framework must also act like a service mesh since it abstracts access to all services. 

# What does WeGO do?
* WeGO is conceived as an in process service mesh. It allows developers to write services without worrying about transports, middlewares, horizontal services, transformations etc.
* WeGO is a Go Lang Library that contains framework code. But the framework code is just the tip of the iceberg.
* WeGO comes bundled with a code generator that generates typical boiler plate code that conforms to a stereotype. (such as a HTTP over JSON Micro Service for example)
*  WeGO comes with documentation that talks about coding principles and how to achieve these principles.
*  WeGO ships with examples that contain a lot of example code.
*  WeGO's code generator also creates Makefiles, Dockerfile etc.
*  WeGO  identifies common usecases that can be solved (eg. developing microservices, creating executables etc.) These use cases become WeGO stereotypes. Code generators help in generating code that conforms to these stereotypes
* WeGO helps in the solution of horizontal concerns. It integrates with myriads of different frameworks that facilitate logging, auditing, i18n etc.
* WeGO establishes standards and conventions that ease code development and maintenance.



