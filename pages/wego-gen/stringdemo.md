---
title: Stringdemo Generation Using WeGO-Gen
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
{% highlight bash %}
$ mkdir stringdemo
$ cd stringdemo
$ # Hence forth all references will be with respect to this folder
$ git clone https://github.com/agorago/wego.git
# Download the WeGO library
$ git clone https://github.com/agorago/togo.git
# Download a set of DevOps scripts that can be used across WeGO projects. This avoids script duplication between projects
$ git clone github.com/agorago/wego-gen.git
# Download the WeGO generator which will be used to generate services
$ cd wego-gen
$ make 
$ # this will compile all the programs to wego-gen/bin
{% endhighlight %}

### Next create the interface file
We need to create a GO file that contains the interface for the new service that we are going to write. 
`cd stringdemo `
Next use your favorite editor to write the GO code below. The # preceded lines annotate the code and should not be literally typed.
{% highlight GO %}
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
// This has methods to illustrate features of the WeGO framework
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

## Interface, Methods and Parameters
Let us look at the code line by line: The chief objective of the code is to define the interface called StringdemoService. This interface defines three methods which have the following properties:
* All of the methods accept a context as the first param.
* The second param for the first two methods is accepted as a pointer. They are of a non primitive GO type defined in the same file. The parameter is expected to be passed as a payload in the HTTP request. There cannot be more than one parameter of a custom type in the signature of the method 
* The third method accepts only primitive types - hence there are two of them. These are expected to be passed as headers. (or query params or path params) in the HTTP request.
* All of the methods are returning two values - one a response object which will be serialized using an encoding mechanism (by default JSON) and an error object. 
* The Request params and response params are defined in the same file. 

## Comments in the file
The interface file is expected to have comments which will eventually make it into the Swagger description. The following comments are currently supported in WeGO-Gen.
* Interface type comments. These get put into the service description.
* The comments for each of the request and response parameters. - these become the parameter documentation
* Method comments - these become the operation documentation. 

## Preparation for Generating the service
First checkout the contents of config/setenv.sh. It should look like the following:
{% highlight text %}
# All generated modules will have the prefix shown below.
# Tweak this if you want it to be something different
export URLPrefix="github.com/agorago"
#  The default start error  code if not specified in gen.sh
export default_start_error_code=100000
# The URL for WeGO. Change this if you have forked the WeGO repo
export wegoURL="github.com/agorago/wego"
# The destination folder where the modules are generated. 
export dest_folder=/tmp
{% endhighlight %}
Tweak the file if required. The comments are self explanatory

## Generating the stringdemo Service
{% highlight bash %}
$ cd stringdemo
# we are expecting that we will have our stringdemo.go file, wego and wego-gen folders here
$ wego-gen/bin/gen.sh
<choose option 1 >
<Specify the stringdemo.go interface file that you have created.>
<Accept the suggested URLs for the API and the Service folder or specify alternate ones>
<Accept the start error code>
$ cd $dest_folder/stringdemoservice # dest_folder as specified in the setenv.sh that was discussed above
$ make run-main 
# The command above creates a bin folder and builds a main program there. 
# This command also creates a configs folder under stringdemo. 
# It executes the built main() program
{% endhighlight %}

## What Got Generated?
WeGO-Gen-gen.sh creates two WeGO modules - one for API and one for the service. It confirms the URLs for both the modules. The module structure of these modules is discussed [in this article](/stringdemo.html#apimodule). WeGO-Gen parses the GO file and creates a service that returns an empty response. The service file is created in _stringdemoservice_. The API module is copied to the _api_ package under _stringdemoapi_ module. 

Makefile, configs, error handling structures, environment files etc. are automatically generated. Test case, main file, swagger generation utility etc. are also created. In short, the entire boiler plate required for proper plumbing is done. 

## What do you do next?
{% highlight bash %}
$ cd $dest_dir/stringdemoservice
$ cd internal/service
$ vi service.go
#  Edit it to implement the interface. 
{% endhighlight %}






