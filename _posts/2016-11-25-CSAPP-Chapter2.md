---
layout: post
title: CSAPP Chapter 2
---

Representation and Manipulation Information
-------------------------------------------

Why computers store and pprcess information represented as two-valued signals?
Two-valued signals can readily be represented, stored, and transmitted.

Three most important representations of numbers:

* Unsigned encodings
  traditional binary notation, representing numbers >= 0

* Two's-complement encodings
  representing signed integer numbers, possitive or negative

* Floating-point encodings
  a base-2 version of scientific notation for representing real numbers

Becareful with overflow since computer representation use a limited number of 
bits to encode a number, some operation can overflow when the results are too
large to be represented.

The different mathematical properties of integer versus floating-point arithmetic:

* Integer
  can encode a comparatively small range of values, but do so precisely

* Floating-point
  can encode a wide range of values, but only approximately

2.1 Information Storage
-----------------------

8 bits or bytes, as the smallest addressable unit of memory. 
Virtual address space is a very large arrary of bytes.

###### New to C? 

* Pointers are a central feature of C
* A pointer has two aspects: value(location) and type

###### 2.1.1 Hexadecimal Notation

A common task is to manually convert between decimal, binary, and hexadecimal

###### 2.1.2 Data Sizes

The virtual addresses range: from 0 to 2 \*\* [w]-1

###### 2.1.3 Addressing and Byte Ordering

Two conventions:
* what the address of the object will be
* how will order the bytes in memory

Note: be careful with `little endian` and `big endian`

###### 2.1.4 Representing Strings



###### 2.1.5 Representing Code



###### 2.1.6 Introduction to Boolean Algebra



###### 2.1.7 Bit-level Operations in C



###### 2.1.8 Logical Operations in C



###### 2.1.9 Shift Operations in C



2.2 Integer Representations
---------------------------




2.3 Integer Arithmetic
----------------------




2.4 Floating Point
------------------




2.5 Summary
-----------