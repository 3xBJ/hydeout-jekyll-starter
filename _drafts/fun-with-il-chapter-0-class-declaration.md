---
layout: post
title: Fun with IL - Chapter 0 - Class declaration
excerpt: Let's start our journey declaring a class and looking at the generated IL
  code
date: 2020-04-18 03:00:00 +0000
last_modified_at: 2020-04-18 03:00:00 +0000
categories: []
tags: []
comments: false

---
Chapter

dsadas

    public class Converter{   }

dsadsa

That's code translate in 

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
    

Yeah, a little overwhelming, but just a little.  

There is this class called <module> and after that we have our Converter class declaration. Despite we never declared a method we can find one in the IL code - that must be our default constructor. Good! We can undestand a lot of IL code from the start! 

So now let's look at the documentation and try to make sense of all this atributes and wtf is this <Module> thing.

#### <Module>

#### Special handling attributes