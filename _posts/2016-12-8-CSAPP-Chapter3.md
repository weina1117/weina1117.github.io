---
layout: post
title: CSAPP Chapter 3
---
Machine-Level Representation of Programs
----------------------------------------

Computer execute machine code. The GCC C compiler output assembly code that
is the text form to represent the machine code command one by one. And then
GCC invokes both an assembler and a linker to generate the exacutable machine
code from the assembly code.

This chapter will cover these twelves main topics:

- [3.1 A Historical Perspective](#31-a-historical-perspective)
- [3.2 Program Encodings](#32-program-encodings)
- [3.3 Data Formats](#33-data-formats)
- [3.4 Accessing Information](#34-accessing-information)
- [3.5 Arithmetic and Logical Operations](#35-arithmetic-and-logical-operations)
- [3.6 Control](#36-control)
- [3.7 Procedures](#37-procedures)
- [3.8 Array Allocation and Access](#38-array-allocation-and-access)
- [3.9 Heterogeneous Data Structures](#39-heterogeneous-data-structures)
- [3.10 Combining Control and Data in Machine-Level Programs](#310-combining-control-and-data-in-machine-Level-programs)
- [3.11 Floating-Point Code](#311-floating-point)
- [3.12 Summary](#312-summary)

3.1 A Historical Perspective
----------------------------

The Intel processor referred to as x86, has followed a long evolutionary development.

3.2 Program Encodings
---------------------

In practice, option -O2 level optimization is considered a better choice in terms of 
resulting program performance.

How does gcc command turn the source code into exacutable code?
* first, the C preprocessor expands the source code and macros
* second, the compiler generates assembly code files, names p1.s and p2.s
* next, the compiler converts the assembly coded into binary files that we cann't viewed directly,
        names p1.o and p2.o this is just object-code files that are not yet filled in the 
        addresses of glabla values
* finally, merge these above two files with code implementing library functions and generates the
           final fle p that is exacutable

##### Machine-Level Code

Two important abstraction:
* Instruction set architecture, ISA
  Defining the processor state
           the format of the instructions 
       and the effect each of these instructions will have on the state

* Virtual addresses
  Make the memory model looks like a very large byte array

A single machine instruction performs only a very elementary operation. The compiler must deal with
the sequences of these instructions to implement program constructs such as arithmetic expression 
evaluation, loops, or procedure calls and returns.

##### Notes on Formatting

All of the lines beginning with '.' are directives to guide the assembler and linker, we can 
ignore these.

3.3 Data Formats
----------------

Term "word": 16-bit data type 
Term "double words": 32-bit data quantities
Term "quad words": 64-bit quantities

3.4 Accessing Information
-------------------------

An x86-64 CPU contains a set of 16 general-purpose registers storing 64-bit values. These registers are
used to store integer data as well as pointers, starting with %r, but following different naming conventions.

##### Operand Specifiers

Most instructions have one or more operands, which shows the source value caused by the operand or tell us
the destination location of the result.

Three main operands:
* immediate, constant values.
* register, denote the contents of a register.
* memory reference (effective address), in which we access the memory location according to a computed address.

##### Data Movement Instructions

Mostly used movement is copy data from one location to another.Operand notation makes this process much easier.





3.5 Arithmetic and Logical Operations
--------------------------------------



3.6 Control
----------



3.7 Procedure
------------



3.8 Array Allocation and Access
-------------------------------



3.9 Heterogeneous Data Structures
---------------------------------



3.10 Combining Control and Dl Programs
---------------------------------------



3.11 Floating-Point Code
------------------------



3.12 Summary
------------
