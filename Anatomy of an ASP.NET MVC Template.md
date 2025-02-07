---
title: Anatomy of an ASP.NET MVC Template
date: 2025-02-06
summary: "What are these folders and files?"
description: "What are these folders and files?"
toc: true
readTime: true
autonumber: false
math: true
tags: [".NET", "csharp", "mvc", "beginners"]
showTags: true
hideBackToTop: false
draft: true
dev: false
---

Templates for common projects can really speed up the beginning of a project. The .NET SDK has templates for the most common types of projects, allowing you to bypass the tedious boilerplate setup and skip straight to a working app that you can quickly add your own code to. 

While this saves quite a lot of time, if you haven't used ASP.NET before, or used the MVC pattern, these templates can be intimidating. There are so many files and folders, where do you begin? 

I'll go through each of the major folders that you'll see if you create an mvc template using Visual Studio or the .NET CLI and talk a little about what it does and why it's there. By the end, you'll be able to start adding your own code into the template with confidence!

So, without further adieu, open up a terminal, run `dotnet new mvc` and follow along!

## Describing your project

Before we dive into all of the different folders that are available in this template that you're looking at, the first thing we should talk about are files that are included with every project type: the `.sln` and `.csproj` files. Without these files, your project isn't going to run. Both of these define important pieces of your project.

### Solutions are made up of projects

If you're newer to ASP.NET, the distinction between Solutions and Projects may just seem like a weird semantic difference. The first time that I created an app from a template in Visual Studio, I wasn't sure either.

Well, let there be no more confusion. A solution is a set of projects that work together and interdepend on one another. Not every project in a soltion will be referenced in every other, but in general different projects like class libraries, console apps and MVC applications can work together to create an entire solution.

### The .sln file defines your projects

You won't do much in this file, or even open it really if you're using an IDE. This file has a good amount of very difficult-to-read configuration. It's not really important that you know how to manipulate this as long as you're using something like Visual Studio or Jetbrains Rider. The main function of this file is to talk with your IDE and link all of your projects to a startup project.

### The .csproj file gives your project dependencies

The `.csproj` file, on the other hand, defines the dependencies of an individual project. Does it need a class library? How about a NuGet package? All of that good stuff can be defined right here. The syntax is somewhat similar to HTML, using matching opening and closing tags to surrond the information.

## The heart of your project

All of that is interesting and important to know, but it's not where the fun of the application lives. ASP.NET is object-oriented, meaning that its essentially made up of a bunch of classes with properties and methods. With software designed this way, we need an entry-point.

The `Program.cs` file will serve as this entry point for us. If you're familiar with C#, you will be familiar with the `Main([])` method. This is the method that will be called to start your program. This method sets up your application, server, adds middleware, and other important configurations that let all of your classes work as an application! 

### Startup.cs is where you add middleware

Most of these configurations are actually set in a different file though. The `Startup.cs` file actaully contains most of the explicit configurations for an application. The .NET runtime will call `.ConfigureServices()` which lives inside the `Startup.cs` file. 

I'm going to be honest, I still pick the wrong one about 50% of the time. The best way to remember it, though, is the the program lives in `Program.cs` and it learns about its startup configuration in `Startup.cs`. 

### You might not have a Startup.cs file

If you ran the command at the beginning of this article though, you might be looking at your project and thinking that you're missing something. Depending on the framework version that you chose when you created this template, you may only have a `Project.cs` file. And it might not even have a `Main()` method in it!

This is because of the new templates that the sdk uses. They take advantage of something called "Top-Level Statements." These allow the project to run on a much more minimal syntax, implying much of the boilerplate code for these projects. 

While it might be a little easier to point out the important bits with this syntax, when you're creating a large-scale application with 30 or more middleware components and configuration mounts, then you're going to want to separate these again. 

This use of top-level statements is also very new, so if you're working in a codebase that wasn't created yesterday, odds are, you're going to see a `Program.cs` and a `Startup.cs` file.

## Give the people what they want

wwwroot

### Access static files using their pathname


## Your Code Goes Here

### Models are just C# classes

### Views are written in HTML... sort of

### Setup a layout for your application