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
exp: 32-bit word size =maximum 4GB virtual address
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
Assume we have an integer data type of w bits. We write a bit vector, or as 
[xw−1xw−2..x1][x_{w-1}x_{w-2}..x_1] to denote its individual bits. When we refer to 
such a unsigned number, the value of the number is 

B2Ux=∑i=0w−1xi∗2iB2U_x = \sum\_{i =0}^{w-1}{x_i\*2^i} 

Then in this case, the max number of a w-bit number is [11111..11], while the min number
is [000…00]. However, for many applications, we wish to represent negative values as well.
The most common computer representation is called two’s complement form. This defined by 
interpretation the most significant bit of the word to served as the sign bit. 

The function is here below: 

B2Tw(x)=−xw−12w−1+∑i=0w−2xi2iB2T_w(x) =-x\_{w-1}2^{w-1}+\sum\_{i =0}^{w-2}x_i2^i 

We set the sign bit to 1 when the value is negative, while set to 0 when the value is positive. 
Similarly, we have another two methods to represent an integer as follow.
Which is called one’s complement and sign-magnitude. 

B2Ow(x)=−xw−1(2w−1−1)+∑i=0w−2xi2iB2O_w(x) =-x\_{w-1}(2^{w-1}-1)+\sum\_{i =0}^{w-2}x_i2^i 

B2Sw(x)=(−1)xw−1+∑i=0w−2xi2iB2S_w(x) =(-1)^{x_w-1}+\sum\_{i=0}^{w-2}{x_i2^i} 

###### Expanding the bit representation of a number: 

In some cases, such as convert a data type to a larger one, we should expand the bit 
representation of a number. 

**Unsigned:** Zero Extension 

**Signed:** Sign Bit Extension 

###### Truncating Numbers: 

When truncating a w-bit number to a k-bit one, we just drop the high-order w-k bits. 
Truncating a number can alter its value, which is another type of overflow!

2.3 Integer Arithmetic
----------------------

Unsigned arithmetic can be viewed as a form of modular arithmetic. 

x+y=(x+y)%2wx+y =(x+y)\%2^w 

Modular addition forms Abelian group, which has the identity element 0. Two’s-Complement Arithmetic 
as we shown above, the Two’s-Complement Arithmetic may cause positive overflow or negative overflow, 
which will alter the value of the number! 

Multiply or divide by powers of 2: We can use left or right shift to accomplish this.

2.4 Floating Point
------------------

Floating Point representation encodes rational numbers of the form V=x∗2yV =x\*2^y, which is useful
for performing computations involving very largr numbers. 

1985-IEEE754 Standard, under the sponsorship with the design of 8087.

**IEEE Floating-Point Representation:**

The sign s determines whether the number is negative or positive. 
The significant M is a fractional binary number that ranges ethier between 1 and 2-e or between 0 and 1-e. 
The exponent E weights the value by power of 2. 
The value encoded by a given bit representation can be divided into 3 cases, depending on the value of exp.

2.5 Summary
-----------