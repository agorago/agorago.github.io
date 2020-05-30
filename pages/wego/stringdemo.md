---
title: "String Demo - A first micro service in WeGO"
keywords: WeGO initialization stringdemo
sidebar: wego_sidebar
toc: true
permalink: stringdemo.html
folder: wego
summary: Writing your first Micro Service in WeGO. 
---

## Introduction

Stringdemo will be the first micro service that we will write in WeGO. It does the following:
1. Demonstrates how to write a simple micro service that exposes three operations.
2. Shows how to register a service in WeGO so it can expose its operations through HTTP.
3. Shows how to use WeGO features such as exception handling, i18n etc.
4. Shows how to handle different types of accepting input parameters and returning responses. Both JSON encoding/decoding as well as header parameters are demonstrated.
5. Writes a middleware - both at the client (proxy) side and server side.
6. Demonstrates how to use DI features.
7. Utilizes built in support for BDD
8. Shows how to package the application and dockerize it
9. Demonstrates the separation of concerns between API and service. The demo is split into API and service.

## Defining & Implementing Behavior
String demo consists of three operations defined in the interface below :
```go
// UppercaseRequest - the payload for Uppercase service
type UppercaseRequest struct {
	S string `json:"s"`
}

// UppercaseResponse - the  Uppercase service response
type UppercaseResponse struct {
	V string `json:"v"`
}

// CountRequest - the payload for Count service
type CountRequest struct {
	S string `json:"s"`
}

// CountResponse - the  Count service response
type CountResponse struct {
	V int `json:"v"`
}

// AddNumbersResponse - the  AddNumbers service response
type AddNumbersResponse struct {
	Sum int `json:"sum"`
}
type StringDemoService interface {
	// Uppercase - Converts the input string into upper case
	Uppercase( ctx context.Context,ucr *UppercaseRequest) (UppercaseResponse, error)
	// Count - returns the length of the input string
	Count(  ctx context.Context,  cr *CountRequest) (CountResponse, error)
	// AddNumbers - adds two numbers and returns the result
	// This method illustrates a GET method implementation in WeGO since there is no request payload required
	AddNumbers(ctx context.Context, arg1 int, arg2 int) (AddNumbersResponse, error)
}
```
Nothing fancy! All the operations accept an input and return an output after performing a trivial operation. The first two operations accept a proper request and give out a response. The __AddNumbers__ operation does not accept a payload as request. It instead accepts primitives as input and returns an output.

A few ponts about the definition:
* All  exposed operations are public i.e. they start in Caps. This is essential and quite easy  to understand.
* All operations must accept context as the first input. This is enforced in WeGO. Context provides additional input to the request that would be required to implement horizontal services.
* If there is a payload, it is accepted as a pointer. 
* Other parameters can be any of the primitive GO types such as int (and its variants), bool, string, float (with all its variants)
* Two return values - one the actual response and the other an error. Again this is mostly standard GO convention.

Now, to implement the interface:
## The Implementation
Implementation is absurdly simple. Here it is
```go
package service

import (
	"context"
	api "github.com/agorago/stringdemoapi/api"
	"strings"
)

type stringdemo struct{
}

func MakeStringdemoService() stringdemo {
	return stringdemo{}
}

func (stringdemo) Uppercase(_ context.Context, ucr *api.UppercaseRequest) (api.UppercaseResponse, error) {
	return api.UppercaseResponse{V: strings.ToUpper(ucr.S)}, nil
}

func (stringdemo) Count(_ context.Context, cr *api.CountRequest) (api.CountResponse, error) {
	return api.CountResponse{V: len(cr.S)}, nil
}

func (stringdemo) AddNumbers(_ context.Context, arg1 int, arg2 int) (api.AddNumbersResponse, error) {
	return api.AddNumbersResponse{Sum: arg1 + arg2}, nil
}
```
Thus far, it is all simple. Now we need to expose this service via HTTP (or any other WeGO transports available). Typically, with most frameworks this requires us to write a HTTP handler. 

But in WeGO, we dont want to write boilerplate code which looks similar for all transports and also be involved in setting up different middlewares individually.  Instead, we do this by registering this service with WeGO.  

Let us take a brief pause and show a recommended structure for the project in WeGO.

## API & Service
The code for stringdemo would be spread across _stringdemoapi_ and _stringdemoservice_. API contains code that is intended to be distributed to all the service consumers. SERVICE contains code that is intended to be deployed in a server. 

###  What is the use of a separate API module?
* API module provides a GO interface to invoke the service. We dont have to deal with abstract JSONs or other encodings. Instead, we use GO interfaces and models.
* API module uses client side proxies. This allows the module to enforce checks like circuit breakers etc. consistently across clients.
* API removes a lot of boiler plate and promotes consistency on the client side. 
* It supplies a service proxy. The proxy handles error handling, is capable of being intercepted etc.

### Can a WeGO service be invoked without the API module?
Of course. There is nothing special in a WeGO service. It is like any other service available through a protocol (such as HTTP) and that supports various kinds of encodings (like JSON)

## API Module structure
API has the following packages:
```
.
├── api
│   └── api.go
├── configs
│   ├── bundles
│   │   └── en-US
│   │       └── stringdemoapi.toml
│   └── env
│       ├── default
│       │   └── stringdemoapi.toml
│       ├── dev
│       │   └── stringdemoapi.toml
│       └── prod
│           └── prod.toml
├── go.mod
├── go.sum
├── internal
│   └── err
│       ├── codes.go
│       └── wegoerrorcode_string.go
├── proxy
│   ├── count-proxy-middleware.go
│   └── proxy.go
├── register
│   └── register.go
└── stringdemoapi-init.go
```

(to be continued)

