---
layout: post
title: Fun with IL - The Series
excerpt: Let's start our journey declaring a class and looking at the generated IL
  code
date: 2020-04-18 03:00:00 +0000
last_modified_at: 2020-04-18 03:00:00 +0000
categories: []
tags:
- Series
- C#
- IL
comments: false
published: false

---
### Fun with IL

We are trying to undestand how c# codes translates to IL  and how to undestand IL code. So we'll try to not get into many details when talking about CLI's specifics implementation, metadata, etc.

I couldn't find any good online source about IL (if you know one message me). So we are going to use the good and old [documentation](https://www.ecma-international.org/publications/standards/Ecma-335.htm "Ecma 335"). ECMA 335 defines the Common Language Infrastructure (CLI). There's a lot of cool things in this documentation but our focus will be about Partition III.

We are not going to go over the documentation and list all the instructions and detail how to use them. For that one can simple read the documentation. What we are going to do is to write some code, looks how it is writing in IL and than try to understand that code.

PS: To compile c# code to IL I'm using roselyn branch from [10 Mar 2020](https://github.com/dotnet/roslyn/commit/441c154b27eafc9ed04feda1786f0f07ce448f14 " Commit #42303").

#### The Series

Chapter 0 - Class declaration