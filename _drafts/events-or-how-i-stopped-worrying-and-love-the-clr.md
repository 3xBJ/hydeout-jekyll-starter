---
layout: post
title: 'Delegates: or how i stopped worrying and love the CLR'
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

Using [SharpLab](https://sharplab.io/#v2:EYLgtghgzgLgpgJwDQxASwDYB8ACBmAAhwCYCBZATwGENooCBYAKAG9mCOCAHBNANwjwCAEzgY4Ac0FwiAFnIUAImMnSAFGgB2MAgIwBXOAEoA3MwC+QA=== "SharpLob - Delegate") we can see that, this one line of code

![](assets/img/Chp2delegateDeclaration.png)Generate the following IL code

![](assets/img/chp2ILdelegateDeclaration.png)

Yeah, a lot of code right? The important thing here is that the compiler create 4 objects for us.

* Constructor(Object @object, IntPtr method)
* Invoke(int value)
* BeginInvoke(...)
* EndInvoke(...)

BeginInvoke and EndInvoke are legacy code from the _.Net Framework Asynchronous -_ at the present time (2020) _Taks_ are the cool thing that we should be playng arround.