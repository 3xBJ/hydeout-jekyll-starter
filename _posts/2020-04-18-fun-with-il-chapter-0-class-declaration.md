---
layout: post
title: 'Fun with IL: Class declaration'
excerpt: 'Let''s start our journey declaring a class and looking at the generated
  IL code<br><br>We are already familiar with C# class declaration:'
date: 2020-04-18T03:00:00.000+00:00
last_modified_at: 2020-04-18T03:00:00.000+00:00
categories: []
tags:
- IL
- C#
- CLR
comments: false

---
Let's start our journey declaring a class and looking at the generated IL code.

We are already familiar with C# class declaration:

{% highlight c# %}

    public class Converter {}

{% endhighlight %}

Now, let's take a look at how this same declaration looks like in IL:

{% highlight c# %}

    .class private auto ansi '<Module>'
    {
    } // end of class <Module>
    
    .class public auto ansi beforefieldinit Converter
        extends [System.Private.CoreLib]System.Object
    {
        // Methods
        .method public hidebysig specialname rtspecialname 
            instance void .ctor () cil managed 
        {
            // Method begins at RVA 0x2050
            // Code size 7 (0x7)
            .maxstack 8
    
            IL_0000: ldarg.0
            IL_0001: call instance void [System.Private.CoreLib]System.Object::.ctor()
            IL_0006: ret
        } // end of method Converter::.ctor
    
    } // end of class Converter

{% endhighlight %}

Yeah, a little overwhelming, but just a little.

There is this class called <module> and after that we have our Converter class declaration. Despite we never declared a method, we can find one in the IL code - that must be our default constructor. Good! It's not hard to grasp what the IL code is doing!

So now let's look at the documentation and try to make sense of all this atributes and wtf is this <Module> class.

#### Module

It's a way to create _global variables._ I didn't find any detailed explanation about it :'(

#### Type layout attributes

The documentation tell us

> The type layout specifies how the fields of an instance of a type are arranged.

But what thats means? And why one should care about that?

* **auto**: The layout shall be done by the CLI, with no user-supplied constraints;
* **explicit**: The layout of the fields is explicitly provided;
* **sequential**: The CLI shall lay out the fields in sequential order, based on the order of the fields in the logical metadata table.

We can also set this property when declaring a C# class

{% highlight c# %}

    [StructLayout(LayoutKind.Explicit, Size=16, CharSet=CharSet.Ansi)]
    public class Converter {}

{% endhighlight %}

#### Interoperation attributes

When dealing with .dll from another language, a call by another thread - or any interoperation in the unmenaged realm - we need to serialize our data structure to send as a message to whoever is calling. This serialization process is called **marshalling**.

These attributes specify the default behavior to be used when calling a method  on the class, that has an argument or return type of System.String and does not itself specify marshalling behavior.

* **ansi** specifies that marshalling shall be to and from ANSI strings.
* **autochar** specifies marshalling behavior (either ANSI or Unicode), depending on the platform on which the CLI is running.
* **unicode** specifies that marshalling shall be to and from Unicode strings.

#### Special handling attributes

The documentation states

> These attributes can be combined in any way.
>
> * **beforefieldinit** instructs the CLI that it need not initialize the type before a static method is called;
> * **rtspecialname** indicates that the name of this item has special significance to the CLI. There are no currently defined special type names; this is for future use. Any item marked rtspecialname shall also be marked specialname;
> * **serializable** Reserved for future use, to indicate that the fields of the type are to be serialized into a data stream (should such support be provided by the implementation);
> * **specialname** indicates that the name of this item can have special significance to tools other than the CLI.

If **beforefieldinit** confuses you (as confused me) look at John seek's anwser on [stackoverflow ](https://stackoverflow.com/questions/610818/what-does-beforefieldinit-flag-do "What does beforefieldinit flag do?"), his [article](https://csharpindepth.com/Articles/BeforeFieldInit "C# and beforefieldinit") and his commnets on [C#4 changes](https://codeblog.jonskeet.uk/2010/01/26/type-initialization-changes-in-net-4-0/ "TYPE INITIALIZATION CHANGES IN .NET 4.0")

**_serializable_** is a well-know friend of c# developers. It apears when we add the serializeble attribute to our class.

{% highlight c# %}

    [Serializable]
    public class Converter {}

{% endhighlight %}

The documentation tell us that **_rtspecialname_** and **_specialname_** for future use, but the documentation is from 2012! So there is a chance that this must be already in use today (2020). Maybe in the future we are going to talk more about this two.

#### Inehrence

To specify inehrence we just need to use extends. As all c# types inheret from System.Object our class couldn't be different.

#### Inheritance attributes

These attributes can be used together.

* **abstract** specifies that this type shall not be instantiated;
* **sealed** specifies that a type shall not have derived classes.

A type that is both abstract and sealed should have only static members, and serves as what some languages call a “namespace” or “static class”

#### Static Class

{% highlight c# %}

    .class private auto ansi '<Module>'
    {
    } // end of class <Module>
    
    .class public auto ansi abstract sealed beforefieldinit Converter
        extends [System.Private.CoreLib]System.Object
    {
    } // end of class Converter

{% endhighlight %}