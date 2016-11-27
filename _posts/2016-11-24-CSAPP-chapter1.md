---
layout: post
title: CSAPP Chapter 1
---

1.1 Information Is Bits + Content
---------------------------------

* hello program begins life as a source program, and this source program is 
  a text file. Each source file is a sequence of bits that included by 0
  and 1, it will present some characters by ASCII. That means each byte will
  be represent by an unique byte-size integer valuer.

* A set of bits could stands for any information in system. We can through
  the content to disdinguish the different bits.

1.2 Programs Are Translated by Other Programs into Different Forms
------------------------------------------------------------------

![sample post]({{site.baseurl}}/images/csappchp1/compilation-system.png)

Four phase:
* Preprocessor
  To revise C program according to the command start with \`#\`

* Compiler
  Translate `hello.i` to `hello.s`, which contains an
 ` assembly-language` prgram.

* Assembler
  Translate `hello.s` to computer commands, and package it to `hello.o`.  

* Linker
  Get the exacute `hello` file after merging two files.

1.3 It Pays to Understand How Compilation Systems Work
------------------------------------------------------

Important to understand how compilation system work for the programmers.
* Optimizing program performance
* Understanding link-time errors
* Avoiding security holes

1.4 Processors Read and Interpret Instructions Stored in Memory
---------------------------------------------------------------

##### 1.4.1 Hardware Organization of a System

![sample post]({{site.baseurl}}/images/csappchp1/1.4.png)

* Buses
  A collection of electrical conduits. It running throughout the system 
  to carry bytes of information.

* I/O Devices
  Four components: a keyboard, a mouse, a display and a disk drive.
  Through controllers or adapter to connecting with I/O buses.

* Main Memory
  temporay storage

* Processor 
  CPU: central processing unit
  ALU: arithmetic/logic unit
  PC: program counter

##### 1.4.2 Running the hello Program

* Read the hello command from keyboard: 
![sample post]({{site.baseurl}}/images/csappchp1/1.5.png)

* Shell get the command from the keyboard

* Code and data load to main memory from disk that process exacuted by shell
![sample post]({{site.baseurl}}/images/csappchp1/1.6.png)

* The output string will be write to the display from main memory,
  CPU will exacute this process.
![sample post]({{site.baseurl}}/images/csappchp1/1.7.png)

1.5 Cacher Matter
-----------------

Time-consuming:
looding program: information copied from disk to main memory
 runing program: information copied from main memory into the processor.

To deal with the processor-memory gap:
![sample post]({{site.baseurl}}/images/csappchp1/1.8.png)

1.6 Storage Devices Form a Hierarchy
------------------------------------

Here we can hit the poit to impove performance.
![sample post]({{site.baseurl}}/images/csappchp1/1.9.png)

1.7 The Operating System Manages the Hardware
---------------------------------------------

How does operating system looks like:
![sample post]({{site.baseurl}}/images/csappchp1/1.10.png)

![sample post]({{site.baseurl}}/images/csappchp1/1.11.png)

##### 1.7.1 Process

It's an abstraction of operating system running program.
![sample post]({{site.baseurl}}/images/csappchp1/1.12.png)

(need to go over P53 again)

##### 1.7.2 Threads

Multiple execution units within one process called threads.
Each thread is running in context switching.
They sharing the same code and globale data.(easier than process)

![sample post]({{site.baseurl}}/images/csappchp1/1.13.png)

##### 1.7.3 Virtual Memory

* program code and data
  starting at the same fixed address for all processes
 
* Heap
  At the begining, assigned the size to heap. 
  It expands and constacts dynamically at run time.

* shared libraries
  An arear sharing the code and data.

* stack
  Store the information when calls function.  

* Kernel virtual memory
  The top region of the address spaces is reserved for kernel.

##### 1.7.4 Files
 
A sequence of bytes. It provides applications with a uniform view of 
all the varied I/O device that might be contained in the system.

1.8 Systems Communicate with Other Systems Using Networks
---------------------------------------------------------

The network can be viewed as just another I/O device. The system can read
data sent from other machines and copy these data to its main memory over
a network.

![sample post]({{site.baseurl}}/images/csappchp1/1.14.png)
![sample post]({{site.baseurl}}/images/csappchp1/1.15.png)

1.9 Important Themes
--------------------

##### 1.9.1 Adahl's Law

Insightful obervation about the effectiveness of improving the performace

* The overall execution time would be: Tn = To[(1-a) + a/k

* Seedup S =  To/Tn as S = 1/[(1-a)+a/k]

Here: To is executing time of application
      a is some part of system requires a fraction a of this time
      k is improve its performance by a factor of k

##### 1.9.2 Concurrency and Parallelism

cocurrency: a system with multiple, simultaneous activities
parallelism: to make a system run faster by using cocurrency

* Thread-Level Concurrency
  One CPU, rapidly swith among its execution process.
  Multiprocessors have several CPU's, in order to improve system performance, 
we need multi-core processor and hyberthreading, that allows a gingle CPU to
execute multiple flows of control.

* Instruction-Level Parallelism
  The processor can execute multiple instructions at one time.
  Superscalar processors: the processor can sustain execution rates faster 
than 1 instruction per cycle.

* Single-Instruction, Multiple-Data (SIMD) Parallelism
  Many modern processors have special hardware that allows a single instruction
to cause multiple operations to be performed in parallel

##### 1.9.3 The Importance of Abstraction in Computer System

* files as an abstraction of I/O devices
* virtual memory as an abstraction of program memory
* prcesses as an abstraction of a running program
* virtual machine as an abstraction of the entire computer.

1.10 Summary
------------


