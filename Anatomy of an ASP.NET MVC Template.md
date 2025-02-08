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

Using a template for a common technology can speed you along the setup process of building an application. It allows you to bypass the tedious and error-prone boilerplate setup and gives you a working app that is easy to extend! If you're unfamiliar with the technology or the structure, seeing the number of files and directories that are generated for you can be daunting.

I am going to create an ASP.NET MVC app today. Well, actually just the template. But, after one command in your terminal, you will have a working application. Today, I'd like to talk about how all of these files work together to create this MVC application. We'll cover files that are specific to this template, and others that are common to all .NET applications.

So, without further adieu, open up a terminal, run `dotnet new mvc` and follow along!

## Describing your project

Before we dive into the specifics of this template, we should first talk about the files included with *every* project template: the `.sln` and `.csproj` files. Without these files, your project isn't going to run. Suffice it to say... they're important.

### Solutions are made up of projects

If you're new to .NET, you might be asking "What the heck is a solution?" I will admit that the terminology threw me a little when I started as well.

A solution is a container for a project or set of projects that work together and depend on one another. In general different projects like class libraries, console apps and MVC applications can work together to create an entire solution.

### The `.sln` file defines your projects

You probably won't look at this file or do much with it, but that doesn't mean it isn't important. It contains vital configurations for your project. Most of the manipulation of this file will be done by your IDE, not you. Just know that you cannot get rid of this file!

### The `.csproj` file gives your project dependencies

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

A very important file in this template is the `wwwroot` folder. This is where files that are delivered directly to the client are stored. Everything else in your project will live on your server while the application is running, but as soon as someone requests the app from their computer, these files will be delivered.

This files are called *static* files. They include CSS stylesheets, Javascript, and you can even add images to be referenced from in here. Libraries for Bootstrap and jQuery are also stored in here. 
### Access static files using their pathname

Since these files are delivered to the client, you can access these files in the browser while the app is running. To make sure this works, ensure that in `Program.cs`, before your `app.Run()` call, you have mounted the middleware to use static files. Add a line that says `app.UseStaticFiles()`. 

In your wwwroot folder, create a directory named images and put a picture that you have stored on your computer there. Your directory structure should look like this.

```plaintext
wwwroot/
├── css/
├── images/
│   ├── testimage.png
├── js/
└── lib/
```

Now run your application and visit `https://localhost:{port}/images/testimage.png`. Using your folder structure as a pathname, as you might in your computer's own file system, you can access these files!

## Your Code Goes Here

Now it's time to talk about the fun stuff. Where the C# code that you write goes! In the MVC template, there are three folders generated: `Models`, `Views`, `Controllers`. These contain... well exactly what they say. I won't dig too far into exactly what the MVC pattern is here, but for a good resource check out [this article from Codecademy](https://www.codecademy.com/article/mvc). 

### Models are just C# classes

```
Models/
└── ErrorViewModel.cs
```

Starting with the Models folder, you can see that the template doesn't give us a lot. Just a file called `ErrorViewModel.cs`. If you click on it you will find a very simple C# class with properties for `RequestID` and a Boolean `ShowRequestId`.

This class is used by the simple application that is provided for you when you generate the template. The thing to notice here, is that it is a vanilla C# class. There's nothing special added here. This is just a class that can be instantiated later.

Lets say that we were creating [a movie application](https://learn.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/start-mvc?view=aspnetcore-9.0&tabs=visual-studio). This is the folder where you would create a class for a movie that stores all of its related information: title, director, runtime, etc. This model will then be referenced by all of the places in the application that a movie object is needed.

### Views are written in HTML... sort of

```plaintext
Views/
├── Home/
│   ├── Index.cshtml
│   ├── Privacy.cshtml
├── Shared/
│   ├── _Layout.cshtml
│   ├── _ValidationScriptsPartial.cshtml
│   └── Error.cshtml
├── _ViewImports.cshtml
└── _ViewStart.cshtml
```

Like I mentioned above, the template that is generated is a working application. If you launch it, you will see that you can navigate between the home pages (called Index) and a Privacy page. You may notice that there are two files in our `Views/Home/` directory that represent these pages! Let's take a look at our Index file. (Formatted for easier reading.)

```html
@{
    ViewData["Title"] = "Home Page";
}

<div class="text-center">
    <h1 class="display-4">@ViewData["Title"]</h1>
    <p>
	    Learn about 
		    <a href="https://learn.microsoft.com/aspnet/core">
			    building Web apps with ASP.NET Core
			</a>
		.
	</p>
</div>
```

It's just HTML! It has a few additions, but very easy to understand. The `.cshtml` files that you see are View files, html displays that have access to run C# code between the `@` symbols. 
### Setup a layout for your application

The use of `.cshtml` files also allows for us to keep from repeating code. Load up the mvc template and you will see that a significant amount of the interface doesn't change when you go from one page to another, this is because these views are rendered inside of a layout. 

Two files to point out here are the `_ViewStart.cshtml` and `_Layout.cshtml`. The `_ViewStart.cshtml` file...

```cs
@{
	Layout = "_Layout";
}
```

Sets the Layout property for all of the view pages. All views *start* here! Then `_Layout.cshtml` contains the actual layout that we're assigning here!

Here is where `<meta>` and `<title>` tags appear, as well as the doctype declarations. When you open this, you'll see that there's quite a bit of HTML here, and it would be pretty annoying to have to put this on every one of your view pages.

One thing to point out about the `_Layout.cshtml` file is the `@RenderBody()` method. In the starting template for an MVC application, it's called on line 36. This is the method that allows all of the other views to be displayed. Without this method, the application doesn't work! Wherever you put this in the layout is where the view that you're setting will be rendered.

### Controllers decide what views get displayed

```
Controllers/
└── HomeController.cs
```

The last part of the mvc model is the controller, the brains of the operation! If you open the `Controllers` directory, you will see that they supply us with a `HomeController`. In this file, you will see a `HomeController` class that inherits from the `Controller` class.

This `HomeController` class has a constructor, and three methods, `Index()`, `Privacy()`, and `Error()`. You may notice that these correspond directly to the views that we saw in the previous section. These methods handle what happens when a use goes to a certain route on the app. 

Go ahead and launch the application and navigate to `https://localhost:{port}/Home/Privacy`. You will get the `Privacy` View within the `HomeController`!

The slug for the URL is decided first by the name of the controller and then the name of the method that returns an `IActionResult`. 

So if you had the following:

```csharp
public class MovieController : Controller
{
	// props and ctor

	public IActionResult Directors()
	{
		return View();
	}
}
```

You could visit `https://localhost:{port}/Movie/Directors` and you would be returned a view, so long as you have a corresponding view in the Views directory!

## Scratching the surface

This post is by no means an in depth look at the MVC model within the ASP.NET space, we've only scratched the surface of what is possible here. But, armed with this knowledge, you should know enough to be able to create an application template, and know how you can begin to manipulate it to create something simple of your own!

---

If you're getting value out of these posts, consider subscribing using the link below to receive these posts straight to your inbox! 

[Click here to subscribe!](https://d782b8fa.sibforms.com/serve/MUIFAK2keDpq4jw-krst9Ki0T2Asllq4pHVH7YEaci2JN2o3H1rLOXm-4H3G3lc31swK7WFMNYjoSJqaBleHxcV0vc8EEBLLxb3HK0U59_fRRDFUaj96lZyvOSE2NiYQSi1jC_0L0Tq8wj2_OcG8PFuNsL5SH65CQh_GpSOXqV3FqTJosq6tSRV2e2mw9MSXcAx7-2c_3fY-abRi)