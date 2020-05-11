---
title: Core Framework
keywords: WeGO core model framework fw
sidebar: wego_sidebar
toc: true
permalink: wego_core.html
folder: wego
---
## The WEGO Core Service Mesh Framework 
WEGO can be thought of as an in process service mesh. It completely abstracts the service from the complexity of dealing with multiple transports, handling transformations and implementing horizontal requirements. On the client side, It can allow clients to access remote services by providing a proxy. 

The concept of transports in WeGO is much more generic and will be discussed in a separate document

For now, let us discuss this in the context of HTTP.  Look at the diagram below:

![Interaction Diagram](images/wego/wego_interaction.png)

WeGO library is present in both the client and server process. In the client process, WeGO provides a proxy to interact with the service. The WeGO proxy interacts with the server process via HTTP. In the server process, WeGO provides the transport end point. Upon invocation, the HTTP end point invokes the actual service.

In this way, the WeGO library completely abstracts the service invocation from both the client and the server side. The core framework allows the registration of services and their operations. It provides the fundamental edifice on which the entire framework rests. Here are the salient features of the core framework:

1. Provide ability for a service to register itself with the framework. Services expose operations which accept parameters that can be encoded in multiple ways.  See the http module for a more detailed discussion.
2. Provide ability for a transport such as HTTP to register itself as an extension to the WeGO framework.
3. Definition and registration of middlewares. These middlewares are invoked whenever the service is invoked. Middlewares can be registered for both client (proxy) and server.
 
## Service Registration

A service can have one or more multiple methods called Operations. Each operation can in turn accept parameters which can be encoded in multiple ways in the incoming request. 

WeGO services can register themselves in two modes:
1. Client Mode - the actual service information is registered. However the service is not exposed in this mode using any transport. e
2. Server Mode -  the service is not only registered. It is also exposed via a transport layer such as HTTP. 

Client mode is useful if it is intended to invoke the service using the Proxy framework that WeGO supports.However in this mode,  transports are not supported. Consequently, the service will not be available for 
remote invocation via HTTP or other means.  Services can only be registered in server mode in one executable. In other executables, the service is registered only as in client mode.

## How do services register themselves?

A service is registered using a model object called _Service Descriptor_ that describes the service. It contains Name and a description. Besides that it contains an array of operations that describe the operations that the service supports. Each operation is described using OperationDescriptor.
 
If the service is registered in server mode, then a reference to the actual service is expected to be populated in the  the service descriptor. If service reference is null then it is assumed that the service 
is registered in client mode. Otherwise, the service reference is required since the transport needs to 
call the actual service when a request for service is received by the transport end point.

## Operation Descriptor

Operation descriptors describe the operation that belongs to the service. The operation would be typically
exposed by the transport. Since an operation is accessible from outside, it will expose a URL. Operations
are invoked by WeGO when the transport is accessed. (for example when someone invokes the HTTP URL)

Operation descriptor gives details about the operation such as the following:
name - used to actually invoke the operation from the service reference in ServiceDescriptor
description, Request Description, Response Description - used to describe the service in generated swagger 
documentations
URL - used to expose the operation via HTTP
OpRequestMaker , OpResponseMaker - These functions generate empty request and response objects. useful to 
populate the objects from serialized streams of data (such as JSON streams)
OpMiddleware, ProxyMiddleware - the middleware functions that will be invoked when the operation is invoked

## Transports 

Transport provides the end point that initiates all service invocations. Example: an HTTP end point which when invoked, invokes the operation. There are properties in Operation Descriptor which can be used to configure the transport end points. For example, the HTTP end point is configured using the URL property.

Transports are extensions to the core WeGO framework amd expose the service using an end point such as a URL. Transports take care of serialization and de-serialization of requests and responses.

## Param Descriptor

ParamDescriptor allows the registration of each parameter that needs to be passed to the operation on invocation.
An operation can have the following types of parameters as determined by ParamOrigin:
Context - a mandatory first parameter for every operation. It should always be with name ctx. This
contains the entire context of the request.
HEADER - a parameter that is derived from a header in the request. In HTTP, HEADER parameters are 
passed as a HTTP HEADER attribute. Alternately, HTTP query params and PATH params are also 
available as header params.
PAYLOAD - the payload parameter is passed as part of the body of the request. (for example HTTP 
request body) Payload will be de-serialized from a resource stream such as JSON.

## Middleware Chain

The framework package defines a Middleware Chain. This implements the chain of responsibility pattern.
Please see middleware_test.go to get an idea about the usage of this class.
