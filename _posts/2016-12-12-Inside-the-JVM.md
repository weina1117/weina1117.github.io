---
layout: post
title: chapter 5 of Inside the Java Virtual Machine
---
Copyright © 1996-2016 Artima, Inc. All Rights Reserved.http://www.artima.com/insidejvm/ed2/jvm.html

What is a Java Virtual Machine?
-------------------------------

A java virtual machine is the abstract specifications since we can not see this machine, it exist in the memory
of the computor, but it is so called a virtual computer platform, which consist of either all software or a 
combination of hardware and software. Each java application runs inside a runtime instance of some concrete
implementation of the abstract specification of the java virtual machine.

The Architecture of Java Virtual Machine
----------------------------------------

The outside it will be looks like three layer:
The top layer is a java virtual Machine, it talks with the second layer directly.
The second layer is our computer system (like windows), it talks with the first and the third layer deirectly.
THe third layer is our computer hardware system (like Intel), it talks with the computer system directly and 
java virtual machine indirectly.

The inside of a java virtual machine: what's the components and how does it works?

![sample post]({{site.baseurl}}/images/JVM/5.1.gif)

As shows in Figure 5-1, a java virtual machine has four parts:

* class loader subsystem: 
  It can loading the classes files if needed during the running time. A mechanism for loading types (classes and
interfaces) given fully qualified names. If classes files is the food, the class loader subsystem is our mouth, 
we eat the food when we are hungry. Accturally, 'the class loader subsystem is responsible for more than just locating 
and importing the binary data for classes. It must also verify the correctness of imported classes, allocate and 
initialize memory for class variables, and assist in the resolution of symbolic references. 
These activities are performed in a strict order:
1. Loading: finding and importing the binary data for a type
2. Linking: performing verification, preparation, and (optionally) resolution
	a. Verification: ensuring the correctness of the imported type
	b. Preparation: allocating memory for class variables and initializing the memory to default values
	c. Resolution: transforming symbolic references from the type into direct references.
3. Initialization: invoking Java code that initializes class variables to their proper starting values.'

* runtime data area: 
  It is the space to store all kinds of the data. When a Java virtual machine runs a program, it needs memory to store
many things, including bytecodes and other information it extracts from loaded class files, objects the program 
instantiates, parameters to methods, return values,local variables, and intermediate results of computations. The Java
virtual machine organizes  the memory it needs to execute a program into several runtime data areas.

* exacution engine: 
  This is a mechanism responsible for executing the instructions contained in the methods of loaded classes. It seems 
the stomach to digest the food. At the core of any Java virtual machine implementation is its execution engine. In the
Java virtual machine specification, the behavior of the execution engine is defined in terms of an instruction set. For
each instruction, the specification describes in detail what an implementation should do when it encounters the 
instruction as it executes bytecodes, but says very little about how. 

Each thread of a running Java application is a distinct instance of the virtual machine's execution engine. From the 
beginning of its lifetime to the end, a thread is either executing bytecodes or native methods. A thread may execute 
bytecodes directly, by interpreting or executing natively in silicon, or indirectly, by just-in-time compiling and 
executing the resulting native code. A Java virtual machine implementation may use other threads invisible to the running
application, such as a thread that performs garbage collection. Such threads need not be "instances" of the implementation's
execution engine. All threads that belong to the running application, however, are execution engines in action.

* native method interface:
The popurse of the native methord interface is to let java to comunicate wieh other language easily. It opens an area to 
deal with these native coding files. Specifically, it registered a native method first, and then loading the native libraies
when exacute engine working. To do useful work, a native method must be able to interact to some degree with the internal 
state of the Java virtual machine instance. For example, a native method interface may allow native methods to do some or 
all of the following:
	* Pass and return data
	* Access instance variables or invoke methods in objects on the garbage-collected heap
	* Access class variables or invoke class methods
	* Accessing arrays
	* Lock an object on the heap for exclusive use by the current thread
	* Create new objects on the garbage-collected heap
	* Load new classes
	* Throw new exceptions
	* Catch exceptions thrown by Java methods that the native method invoked
	* Catch asynchronous exceptions thrown by the virtual machine
	* Indicate to the garbage collector that it no longer needs to use a particular object

Memory Management of Java Virtual Machine
-----------------------------------------

##### Stack

##### Heap

##### Method Area

##### Object

##### PC counter

##### Exacution
