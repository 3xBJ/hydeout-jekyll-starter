---
layout: post
title: 'Chapter 2: How the compiler deal withs delegates'
excerpt: ''
date: 
last_modified_at: 
categories: []
tags: []
comments: false

---
On the surface, delegates seem easy to use: 

* define one
* pass a function to them
* when whanted call them. 

However, what's going on is a bit more complex than that - the complexity must leve in somewhere. As we will see, the compiler and the CLR do the hard work for us.

Understanding how the compiler and the CLR deal with delegates is a good wya to learn how to use them efficiently and effectively.

### Delegate declaration

With [SharpLab](https://sharplab.io/#v2:EYLgtghgzgLgpgJwDQxASwDYB8ACBmAAhwCYCBZATwGENooCBYAKAG9mCOCAHBNANwjwCAEzgY4Ac0FwiAFnIUAImMnSAFGgB2MAgIwBXOAEoA3MwC+QA=== "SharpLob - Delegate") we can see that, this one line of code

![](assets/img/Chp2delegateDeclaration.png)Generate the following IL code

![](assets/img/chp2ILdelegateDeclaration.png)

Yeah, a lot of code right? The important thing here is that the compiler create 4 objects for us.

* MyDelegate(Object @object, IntPtr method)
* Invoke(int value)
* BeginInvoke(...)
* EndInvoke(...)

BeginInvoke and EndInvoke are legacy code from the _.Net Framework Asynchronous -_ at the present time (2020) _Taks_ are the cool thing that we should be playng arround. So we'll focus in the constructor and invoke methods.

#### Constructor

One curious thing is that our constructor derivate from _System.MulticastDelegate_, and if we look around we'll find out that there is a _System.Delegate_ class. There are two classes for historical reasons and unfortunely, besides every delegate has as a base class _MulticastDelegate_, when manipulating delegates we are going to use methods defined by the _Delegate_ class.