---
layout: post
title: chapter 5 of Inside the Java Virtual Machine
---
Copyright © 1996-2016 Artima, Inc. All Rights Reserved.http://www.artima.com/insidejvm/ed2/jvm.html

What is a Java Virtual Machine?
-------------------------------

A java virtual machine is the abstract specifications since we can not see this machine, it exist in the memory
of the computer, but it is so called a virtual computer platform, which consist of either all software or a 
combination of hardware and software. Each java application runs inside a runtime instance of some concrete
implementation of the abstract specification of the java virtual machine.

The Architecture of Java Virtual Machine
----------------------------------------

The outside it will be looks like three layer:
The top layer is a java virtual Machine, it talks with the second layer directly.
The second layer is our computer system (like windows), it talks with the first and the third layer directly.
The third layer is our computer hardware system (like Intel), it talks with the computer system directly and 
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

This is an executive area of a thread, it holds the state of current method in discrete frame.
And in this stack, each method has its frame. The frame is a unit of the stack. The Java virtual 
machine only performs two operations directly on Java Stacks: it pushes and pops frames. When a 
thread invokes a Java method, the virtual machine creates and pushes a new frame onto the thread's
Java stack. This new frame then becomes the current frame. As the method executes, it uses the 
frame to store parameters, local variables, intermediate computations, and other data. When a method
completes, whether normally or abruptly, the Java virtual machine pops and discards the method's 
stack frame. The frame for the previous method then becomes the current frame.

##### Heap

One java virtual machine instance has only one heap, the size of the memory for heap could adjust. 
The Objects are stored in heap, the object cannot assigned in the stack. One java virtual machine has 
one heap. But all threads shared the data or the objects of this heap. The virtual machine has its own
way to deciding whether and when to free memory occupied by objects that are no longer referenced by 
the running application. Usually, a Java virtual machine implementation uses a garbage collector to 
manage the heap automatically. So the Garbage Collector is the most important management area is heap 
in java virtual machine.

##### Method Area

Inside a Java virtual machine instance, information about loaded types is stored in a logical area of
memory called the method area. If curren thread exacuting the local C or C++ data, then the method will
exacute in the local menthod stack instead of java stack. Java stack exacutes only java method. 

Method area shared by all the threads, the virtual machine extracts information about the type from the 
binary data and stores the information in the method area. Memory for class (static) variables declared
in the class is also taken from the method area. The virtual machine will search through and use the 
type information stored in the method area as it executes the application it is hosting. Method area
protects all the informations.

##### Program counter

Each thread has its own pc register, or program counter, which is created when the thread is started. 
The pc register is one word in size, so it can hold both a native pointer and a returnAddress. As a 
thread executes a Java method, the pc register contains the current address that  being executed by 
the thread. An "address" can be a native pointer or an offset from the beginning of a method's bytecodes.
If a thread is executing a native method, the value of the pc register is undefined.
