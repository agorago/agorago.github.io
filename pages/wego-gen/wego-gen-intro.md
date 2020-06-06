---
title: WeGO Code Generator
keywords: wego-gen code generation
sidebar: wego_gen_sidebar
toc: false
permalink: wego_gen_landing_page.html
folder: wego-gen
summary: An introduction to WeGO-Gen -  the code generator for WeGO
---

## What is WeGO-Gen
WeGO-Gen is a framework that is used to generate a WeGO module. WeGO supports multiple types of modules:
1. Service Modules - both API and service which are used to expose a service via HTTP
2. Workflow modules - which support a workflow oriented service via HTTP
3. Deploy modules - that support  a deployment. The service modules deploy one service whilst the deploy modules are capable of deploying multiple services.

WeGO-Gen generates these modules automatically with hooks to WeGO and compliant to WeGO best practices. 

## Why WeGO-Gen
It is highly recommended that developers must use WeGO-Gen to generate WeGO modules due to the following reasons:
1. WeGO makes extensive recommendations around the structure of modules. WeGo-Gen automatically generates the modules in compliance to those recommendations.
2. WeGO-Gen evolves as the recommendations evolve. Hence by using WeGO-Gen, you are guaranteed to remain up to date in your adherence to WeGO recommendations.
3. By generating Makefile, BDD, etc. WeGO-Gen ensures that you leverage WeGO  features fully.
4. Boiler plate code is automatically minimized.

## How to use WeGO-GEN?
WeGO-Gen provides a wrapper program _gen.sh_ that can be used to generate the boiler plate code. 
It has straight forward options to choose between the different types of modules, provide an interface file or workflow file and then choose the URLS. We  will discuss these in more detail in the respective module  sections.