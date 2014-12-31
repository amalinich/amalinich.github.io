---
layout: post
title:  "Up and Running with ASP.NET on OSX"
date:   2014-12-31
categories: aspnet csharp
---

A few months ago, [Microsoft announced they were open sourcing .NET](http://www.hanselman.com/blog/AnnouncingNET2015NETasOpenSourceNETonMacandLinuxandVisualStudioCommunity.aspx), which in my mind is huge and awesome.  Do I expect that I'll be able to start working in OSX on existing ASP.NET projects tomorrow with no problems? No.  I do however expect that in time (and perhaps fairly quickly) more and more tooling will become available to replicate much of what would traditionally be done in Visual Studio.

## Why I Care:

Up until a little over a year ago, I'd pretty much developed exclusively on the .NET / C# stack using Visual Studio and Windows.  Since then I've migrated to OSX and have had the opportunity to work on projects using Ruby on Rails and Node.  There are advantages to all these different technology stacks and operating systems, but when it comes down to it, I simply enjoy working on OSX more...which is mostly due to the terminal workflow and the build quality of the hardware.  The recent work I've done using the .NET stack has been relegated to a virtual machine, which I would have avoided if possible.

To be completely honest though, the Mono Project and Xamarin have already done a ton of work on this front by reverse engineering large parts of the .NET API and providing tooling all of which I'm not super familiar with because working using that tool set simply wouldn't have been viable for the stuff I was working on with teams highly entrenched in the traditional .NET stack.  With Microsoft going to open source route though, I think those wanting to develop .NET apps on platforms other than Windows will become more of a first class citizen rather than a difficult workaround.

So let's take a look at the bare minimum of things you need to run an asp.net application on OSX...

## Must haves:

* Create ready to roll template solutions
* Manage External Dependencies
* Run Code
* Edit Code...Run some more

### Yeoman ASP.NET Generator

In Visual Studio, you can create a new application / solution from any number of predefined templates.  On OSX, the easiest way to this is use [Yeoman](http://yeoman.io) and the [asp.net generator](https://www.npmjs.com/package/generator-aspnet).

Yeoman is a scaffolding engine to pump out template code and you use specific generators to specify the templates, giving you the same end result as creating a new solution in Visual Studio.

#### Installation

Node.js: Both Yeoman and it's generators rely on Node.js being installed, so [download it](http://nodejs.org/download/) if you don't already have it. Then you can install Yeoman and the asp.net generator...

{% highlight bash %}
npm install -g yo
npm install -g generator-aspnet
{% endhighlight %}

#### Running the generator

The generator will create a subdirectory based upon the name you provide it and your current location in the terminal. Follow the prompts that the generator provides. For this example, I chose the MVC Application.

{% highlight bash %}
yo aspnet
{% endhighlight %}

At this point, you'll have a viable code template, but haven't pulled in any external dependencies or even have a platform to run it on yet.

### K Runtime Environment (KRE / KRuntime)

The KRuntime will take care of loading nuget packages, compiling, and running web server.  

#### Installation 

First you'll need [Homebrew](http://brew.sh), follow their documentation if you don't already have it installed.  Otherwise, I still suggest your run `brew update` from the terminal to make sure everything is up to date. Once that is all in order, proceed...

{% highlight bash %}
# tap asp.net vNext related repos
# to remove a previous version, run 'brew untap aspnet/k' first
brew tap aspnet/k   
# install KVM
brew install kvm
# this sets up some commands you'll need 
# you'll probably want to put this in your ~/.bash_profile 
# so it runs every time you start the terminal
source kvm.sh   	
{% endhighlight %}

#### Running stuff...

Now you can update packages, build, and run a web server...

{% highlight bash %}
# start in the root directory of your application...
# update packages...
kpm restore
# run it...
k kestrel
{% endhighlight %}

Now you should be able to hit `http://localhost:5004` and see the basic website

### Sublime Text 3

I pretty much use Sublime Text for all of my other development on OSX, so it will be ideal once the tooling for .NET is a bit more established.

[OmniSharp](http://www.omnisharp.net) is the umbrella project for getting C# support up and running for the popular editors in OSX / Linux, including Sublime Text.  I'm not going to get into the instructions here, but if you follow their instructions you should be able to get syntax highlighting, (some) intellisense, code snippets, and also built in support for the KRuntime commands.

## Gotchas / Needs improvement:

* Debugging - not quite ready, but there is some support in Xamarin supposedly.
* MS SQL Server - if you depend on this to run a local full stack, this could be a major barrier


This is really a work in progress, with existing functionality being cobbled together at best.  I will say that there is a lot of promise and I'm really looking forward to when things mature a bit more.

### Resources

* [Microsoft on Github](http://microsoft.github.io)
