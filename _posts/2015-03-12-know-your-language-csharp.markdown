---
layout: post
title:  "Know your language: C# / CSharp (2.0 Edition)"
date:   2015-03-12 08:00:00
categories: csharp
---
In this series, I'll be covering some of the features introduced into the C# / CSharp language over time.

I won't be covering the basic stuff you see in most languages, also there are some things that are helpful additions but pretty basic so coverage of them may be brief.

## Features Overview

* 2.0
  * [Generics](https://msdn.microsoft.com/en-us/library/512aeb7t.aspx)
  * [Anonymous Methods](https://msdn.microsoft.com/en-us/library/0yw3tz5k.aspx)
  * [Nullable Types](https://msdn.microsoft.com/en-us/library/1t3y8s4s.aspx)
* 3.0
  * [Lambda Expressions](https://msdn.microsoft.com/en-us/library/bb397687.aspx)
  * [Extension Methods](https://msdn.microsoft.com/en-us/library/bb383977.aspx)
  * Expression Trees
  * Anonymous Types
  * LINQ
  * Implicit Typing
* 4.0
  * Late Binding (dynamic)
  * Named Arguments
  * Optional Parameters
* 5.0
  * Async features
  * Caller Information

## Code Examples

In this blog, I only go over the basics of a concept, but I have more exhaustive and interactive examples on my code solution:

[https://github.com/amalinich/csharpfeatures](https://github.com/amalinich/csharpfeatures)

Each of the feature folders will have a Driver.cs which has a suite of NUnit tests to drive the functionality.  This solution includes examples for all the noted features above.

## 2.0 Features

### Generics

Generics provide a way to define a data structure in generic terms so that you can reuse the structure, but enforce what goes into it.  Let's illustrate this buy creating a List that only allows integers.

{% highlight csharp %}
List<int> myIntList = new List<int>();
myIntList.Add(1);
myIntList.Add(2);
{% endhighlight %}

Now lets look at what something like this looks like internally, keep in mind this is a built in part of the framework. Don't focus too much on all the different interfaces that it implements.

{% highlight csharp %}
public class List<T> : IList<T>, ICollection<T>, IList, ICollection, IReadOnlyList<T>, IReadOnlyCollection<T>, IEnumerable<T>, IEnumerable
{
	...
	public void Add(T item);
	...
}

{% endhighlight %}

### Anonymous Methods

Anonymous methods provide the ability to define delegates inline, instead of them having to exist at the class level.  I see the main advantage being that you can be more concise with your code and limit the amount of distraction code you have lying around.  Lambdas expressions have since been introduced, and are typically more concise and preferred, but anonymous delegates provides a good stepping stone for understanding the transition from named delgates to lambdas.

#### Anonymous Method Example

{% highlight csharp %}
public class SomeClass
{
    // We still need a method signature at this time...
    private delegate string MyDelegate(int i);

    public void Run()
    {
        //Here we define the method inline, so long as it matches the appropriate signature
        this.MethodThatTakesADelegate(delegate(int i) { return i.ToString(); });
    }

    private void MethodThatTakesADelegate(MyDelegate someFunction)
    {
        //Here we can actually call the passed function.
        string result = someFunction(10);
        Console.WriteLine(result);
    }
}
{% endhighlight %}

#### Lambda Example

{% highlight csharp %}
public class SomeClass
{
    public void Run()
    {
        // note the lack of brackets, no return keyword, and no terminating semicolon after ToString()
        this.MethodThatTakesADelegate(i => i.ToString());
    }

    private void MethodThatTakesADelegate(Func<int, string> someFunction)
    {
        //Here we can actually call the passed function.
        string result = someFunction(10);
        Console.WriteLine(result);
    }
}
{% endhighlight %}

Compare this to something equivalent in JavaScript:

{% highlight javascript %}
var SomeObject = function() {
	this.run = function(){
		this.methodThatTakesACallback(function(input) {
			return input.toFixed(4);
		});
	}

	this.methodThatTakesACallback(callback) 
	{
		var result = callback(10);
		console.log(result);
	}
}

{% endhighlight %}


### Nullable Types

These are pretty simple, they are simply a struct wrapper for value types so that they can be expressed as null.

{% highlight csharp %}
int? myNullableInteger = null;
...same as...
Nullable<int> myNullableInteger = null;
{% endhighlight %}

## Learn More

For more in depth coverage of these topics, check out my code sample...

[https://github.com/amalinich/csharpfeatures](https://github.com/amalinich/csharpfeatures)

Also, I've linked to the MSDN documentation in the topics listing above.

-Aram
