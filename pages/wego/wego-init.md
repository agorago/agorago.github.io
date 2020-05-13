---
title: "Initialization & Contexts in WeGO"
keywords: WeGO initialization commandcatalog
sidebar: wego_sidebar
toc: true
permalink: wego_init.html
folder: wego
summary: How is a WeGO service initialized? How is DI handled?
---

## The Approach

Initialization always happens during start up. All relevant strategies, commands etc. must be instantiated as singletons when the program starts up.

WeGO has a unique approach to initialization. WeGo modules must support dependency injection and 100% testabiity using any mocking strategy. 

WeGO does not prescribe any DI framework or container such as wire, dig etc. It instead uses a command catalog to maintain a catalog of initialized commands. This serves as a sort of application context.

The following guidelines are applicable for all WeGO modules: 

1. **No init() methods anywhere:** Very few exceptions apply to this rule. WeGO should not rely on initialization using init() methods. init() methods often get inextricably tied to global variables which considerably hinder testability. There are rare exceptions such as the _i18n_ and _config_ modules in WeGO. But these should be the exception rather than the rule.
2. **Make methods:** All WeGO services must be abstracted by interfaces. They all must support a make method that allows the creation of the service. The make method must return the interface and not the service type. *Make methods* also accept dependent interfaces. For example if a __UserService__ depends
on the __UserRepo__ object, then the make method must accept the UserRepo object.
3. Initializer : Initializer is an interface defined in WeGO. Its definition is as follows:

```
type Initializer interface{
	Initialize(fw.CommandCatalog)(fw.CommandCatalog,error)
}
```
Implementation of _Initializer_ is optional but strongly encouraged. It is typically present in the root package of the module. Look at the initializer in the WeGO for an example. It must be possible to initialize a module by calling its initializer. Initializers abstract the complexity of instantiating multiple commands and injecting one command into another. The initializer takes away the complexity that might otherwise be forced to exist in a main() method. If a main needs to initialize multiple modules, the recommended approach is to use the __fw.MakeInitializedCommandCatalog__ method with a bunch of initializers in the proper order. This enables the initialization of all the modules in the correct sequence. 

##  Command Catalog
Command Catalog represents the "application context" which is a construct in other programming languages such as Java. However, the WeGO command catalog is extremely light. It serves as a container for commands.  

The purpose of __initializer__ is to enhance the command catalog. Initializers accept a command catalog and send out a mutated version of the command catalog (with the new commands created by the initializer) .Not more than one initializer is encouraged per module.

## Go files in the root module
Every root WeGO module is encouraged to have two GO files. One is modulename-init.go and the other one is called initializers.go

### modulename-init.go
The module Initializer resides in modulename-init.go. It instantiates all the command exposed by the module. It also exposes the modules using convenience methods. Example WeGO initializer creates a ProxyService which is exposed using a convenience method like:
```
    wego.GetProxyService(commandCatalog fw.CommandCatalog)(wegohttp.ProxyService,error)
```

## initializers.go
This contains a slice of all the initializers that need to be called before the module could be initialized. There is a MakeInitializedCommandCatalog() method in WeGO framework that accepts an array of initializers and instantiates and enhances the Command catalog.



