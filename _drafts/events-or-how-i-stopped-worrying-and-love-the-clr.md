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

Using SharpLab we can see that, this one line of code

![](assets/img/Chp2delegateDeclaration.png)

Turn into the following IL code