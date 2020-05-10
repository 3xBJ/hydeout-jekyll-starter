---
layout: post
title: Delegates - How to use
excerpt: ''
date: 
last_modified_at: 
categories: []
tags: []
comments: false

---
Delegates are callbacks, a pointer to a - or a collention - of function(s). So you can pass a function as a argument to another or just storage a lot of them in one to call them _simultaneously_ at our will.

There are two wrappers to use delegate in C#

    Action<type a1, ..., type aN> MyAction;

Action<> has N arguments and accept any function that has the same arguments. Doesn't return anything - so we must pass a void function.

    Func<type a, type b, ... , type return> MyFunc;

Func<> has N arguments and one return type.

#### Passing a function as a parameter

Invoking

# passar o que eu já vi para ca. dividir em how to use and Varianca and Covariante

If we just whant to go around using delegates, that it. With this  we can pass a function as a parameter 