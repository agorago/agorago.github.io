---
title: Stringdemo generation from WeGO-Gen
keywords: wego-gen code generation stringdemo
sidebar: wego_gen_sidebar
toc: false
permalink: wego_gen_stringdemo.html
folder: wego-gen
summary: How to generate the stringdemo program using WeGo Gen
---

## Introduction
Stringdemo is a program that demonstrates the service features of WeGO. It has been discussed previously [here](stringdemo.html)

In this page, we will see how to generate a stringdemo completely using WeGO-Gen.

## Steps
### First download and set up wego-gen
Comments are given below each instruction preceded by #
```
$ mkdir stringdemo
$ cd stringdemo
$ # Hence forth all references will be with respect to this folder
$ git clone https://github.com/agorago/wego.git
$ git clone github.com/agorago/wego-gen.git
$ cd wego-gen
$ make 
$ # this will compile all the programs to wego-gen/bin
```
### Next create the interface file
We need to create a GO file that contains the interface for the new service that we are going to write. 
`cd stringdemo `
Next use your favorite editor to write the GO code below. The # preceded lines annotate the code and should not be literally typed.
`{% highlight GO linenos %}
package api

import "context"

// UpperCaseRequest - the payload for Uppercase service
type UpperCaseRequest struct {
	S string `json:"s"`
}

// UpperCaseResponse - the  Uppercase service response
type UpperCaseResponse struct {
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

// StringDemoService - the interface that is going to be implemented by the string demo service
// This has methods to illustrate features of the BPlus framework
type StringdemoService interface {
	// Uppercase - Converts the input string into upper case
	UpperCase( // the context
		ctx context.Context,
		// The upper case request
		ucr *UpperCaseRequest) (
		// the upper case response
		UpperCaseResponse, error)
	// Count - returns the length of the input string
	Count(ctx context.Context, cr *CountRequest) (CountResponse, error)
	// AddNumbers - adds two numbers and returns the result
	// This method illustrates a GET method implementation in BPlus since there is no request payload required
	AddNumbers(ctx context.Context, arg1 int, arg2 int) (AddNumbersResponse, error)
}
{% endhighlight %}

Let us look at the code line by line:

