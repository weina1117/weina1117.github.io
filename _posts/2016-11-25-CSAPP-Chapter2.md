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

Rather than access individuel bits in a memory, many computer use blocks of eight bits,
or bytes, as the smallest addressable memory.

###### New to C? 

* Pointers are a central feature of C
* A pointer has two aspects: value(location) and type

###### 2.1.1 Hexadecimal Notation

A single byte consists of 8 bits. In binary notation, we use hexadecimal to 
express a byte in the computer.

To convert a binary number to a hex number:
  Splite the number into 4-bit groups and convert the groups one by one.


###### 2.1.2 Data Sizes

Word Size: Every computer has a word size, which indicates normal size of data and addresses.
exp: 32-bit word size =maximun 4GB virtual address
The virtual addresses range: from 0 to 2 \*\* [w]-1

###### 2.1.3 Addressing and Byte Ordering

For a object that span multiple bytes, we always focus these problems: 
What will be the address of the object? 
How will we order the bytes in the computer memory? 

The first problem: We always stored the object in a contiguous sequence.
For example, for a integer, the start of its memory is 0x01, then it may be span to 0x02, 0x03, 0x04…. 
The Second problem: Consider a w-bit question has a bit representation 
[x<sub>w−1</sub>x<sub>w−2</sub>...w<sub>1][xw-1xw-2...w1], in which xw-1 is the most significant bit. 
The former convention, where the least significant bit comes first is called little endian,
while now the most significant bit comes first is called big endian.

###### 2.1.4 Representing Strings



###### 2.1.5 Representing Code



###### 2.1.6 Introduction to Boolean Algebra



###### 2.1.7 Bit-level Operations in C



###### 2.1.8 Logical Operations in C



###### 2.1.9 Shift Operations in C



2.2 Integer Representations
---------------------------

Two’s-Complement Encodings
The most com-mon computer representation of signed numbers is known as two’s-complement form. 
This is defined by interpreting the most significant bit of the word to have negative weight. 
We express this interpretation as a function B2Tw(for “binary to two’s-complement” length w ):



2.3 Integer Arithmetic
----------------------




2.4 Floating Point
------------------




2.5 Summary
-----------