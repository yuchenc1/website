---
title: JVM Memory Space
date: 2019-09-11 15:36:06
tags:
- Engineering
- Java
categories:
- Engineering
---

I start to write this post because I have been assigned a ticket at work. My mentor wanted me to collect some heap stats when the JVM old-gen memory usage is beyond threshold%. When I first lookat at this ticket, what is old-gen? So I began searching for more info about it and write a post to help me understand and review in the future.

JVM memory space is divided into two parts: HEAP and non-HEAP.

HEAP space could be divided into three parts: Eden Space, Survivor Space, Old-gen.

Non-HEAP space could be divided into Code Cache, Perm Gen, JVM Stack, Local Method Stack.

Eden Space: As the meaning of the name, when an object is created, it is placed into this space at first. After GC(garbage collection), objects which cannot be reclaimed would be thrown into Survivor Space.

Survivor Space: As we can see from Eden Space, objects which cannot be reclaimed in Eden Space during GC would be put in this space. There are two Survivor Space: To Survivor and From Survivor, with the same size. To Survivor would always be empty. Objects cannot be reclaimed during GC would be thrown into To Survivor. These objects came from not only Eden Space, but also From Survivor. You may be curious that I have mentioned To Survivor would always be empty. But we just throw some objects into it. This is because after each iteration, To Survivor and From Survivor would exchange to make sure To Survivor would always be empty.

Eden Space and Survivor Space are both classified into New-gen. GC in New-gen is called Minor GC or Young GC. The age of objects would be recorded.

Old-gen is to store objects that are still alive after many GC. Also large objects that cannot be allocated would enter Old-gen directly. Once Olg-gen is full, virtual machine would do a GC called Major GC. It is also called Full GC because it will scan the whole heap and reclaim.

The size of HEAP = the size of Old-gen + the size of New-gen. 

Code cache is used to store the code compiled by JIT. Its size is set as 32M by default in client mode, 48M in server mode. It could be changed by changing JVM parameter. Code cache may be full and if it was full, there would be a warning message.

Perm Gen is to store the information about Class and Meta. Class would be put into this space when loaded. It would be never be reclaimed during GC.


