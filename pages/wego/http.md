---
title: HTTP transport
keywords: WeGO http transport
sidebar: wego_sidebar
toc: true
permalink: wego_http.html
folder: wego
---
## Introduction
One of the most important objectives of WeGO is to expose services via transports. The HTTP module exposes the HTTP transport to all services which have registered with a URL in WeGO. A URL exposes an operation in a service. As the service gets registered in WeGO, the http module exposes every operation via its URL (provided that the operation has a URL)

Like any transport, the HTTP registers itself in WeGO as an extension. During initialization, WeGO calls the http module for every operation exposed by every registered service. The HTTP module enables the HTTP transport for the operation if a URL is configured or the operation. A call to the URL triggers a call to the operation in the service/

The WeGO http module has methods to consume a HTTP service by providing a HTTP proxy.

## Exposing a service via HTTP

This is called by the BPlus framework only when it is in "server mode" i.e. when
a service exists and is configured in the ServiceDescriptor. For client side registrations, the service
is registered but no transport is exposed. The registration is only responsible for determining "proxy
calls"

The HTTP module currently supports JSON as the default encoding mechanism. Other encoders will be added 
in the future.

The transport uses go-kit and MUX to expose the HTTP transport. The MUX is enhanced with middleware 
that works well with New Relic so that the HTTP request/response statistics are automatically captured.
The HTTP transport uses the entry point to actually invoke the service. The entry point is generic and
can be used across protocols. 

The entry point is discussed in the [middlewares section](../internal/mw/README.md)

## Consuming an HTTP service

A client-side registration is sufficient to access a service using a HTTP Proxy. In this case,
the server will not register a handler. Instead, the client obtains the service descriptor to
know about how to access the service. In addition, it looks for a property called 
service name + "host_port". If it does not find this property, it defaults to localhost: + bplus_port 
to access the service. 

The proxy delegates to a proxy_end point which is discussed in the [Middlewares section](../internal/mw/README.md).

