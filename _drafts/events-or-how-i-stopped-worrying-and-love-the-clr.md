---
layout: post
title: Chapter 2 - How the compiler deal withs delegates
excerpt: ''
date: 
last_modified_at: 
categories: []
tags: []
comments: false

---
With [SharpLab](https://sharplab.io/#v2:EYLgtghgzgLgpgJwDQxASwDYB8ACBmAAhwCYCBZATwGENooCBYAKAG9mCOCAHBNANwjwCAEzgY4Ac0FwiAFnIUAImMnSAFGgB2MAgIwBXOAEoA3MwC+QA=== "SharpLob - Delegate") we can see that, this one line of code

![](assets/img/Chp2delegateDeclaration.png)

Generates the following IL code

![](assets/img/chp2ILdelegateDeclaration.png)

Yeah, a lot of code, right? The important thing here is that the compiler create 4 objects for us.

* MyDelegate(Object @object, IntPtr method)
* Invoke(int value)
* BeginInvoke(...)
* EndInvoke(...)

**BeginInvoke** and **EndInvoke** are legacy code from the _.Net Framework Asynchronous -_ at the present time (2020) _Taks_ are the cool thing that we should be playng arround. So we'll focus in the **constructor** and **invoke** methods. We can see a little about them in the [runtime source code](https://github.com/dotnet/runtime/blob/master/src/coreclr/src/System.Private.CoreLib/src/System/Delegate.CoreCLR.cs "Github/dotnet").

#### Constructor

One curious thing is that our constructor derivate from _System.MulticastDelegate_, and if we look around we'll find out that there is a _System.Delegate_ class. There are two classes for historical reasons and _MulticastDelegate_ inherites from _Delegate._

There are 3 significant non-public fields that our class inherits from MulticastDelegate.

##### object _target

The object on wich the delegate invokes the instance method. If the delegate invokes one or mode instance methods, this property returns the target of the last instance method in the invocation list.

A static method is a method that is associated with the class itself, som for static methods _target is null.

Can be acessesed by the Target property.

##### IntPtr _methodPtr

Is a pointer to the method we'll invoke. 

##### object _invocationList

##### Runtime Menaged

if we take a look at the IL code generated we'll see that, after each one of the

this is a ECMA CLI spipulation.

#### Invoke