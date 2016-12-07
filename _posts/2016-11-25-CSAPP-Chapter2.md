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

![sample post]({{site.baseurl}}/images/csappchp2/2.2.jpg)

A single byte consists of 8 bits. In binary notation, we use hexadecimal to 
express a byte in the computer.

To convert a binary number to a hex number:
Splite the number into 4-bit groups and convert the groups one by one.


###### 2.1.2 Data Sizes

![sample post]({{site.baseurl}}/images/csappchp2/2.3.jpg)

A　“long” integer uses the full word size of the machine. The “long long” integer　data type, 
introduced in ISO C99, allows the full range of 64-bit integers. For 32-bit　machines, the compiler
must compile operations for this data type by generating　code that performs sequences of 32-bit operations.

Word Size: Every computer has a word size, which indicates normal size of data and addresses.
exp: 32-bit word size =maximum 4GB virtual address
The virtual addresses range: from 0 to 2^w -1

###### 2.1.3 Addressing and Byte Ordering

![sample post]({{site.baseurl}}/images/csappchp2/2.4.jpg)

For a object that span multiple bytes, we always focus these problems: 
What will be the address of the object? 
How will we order the bytes in the computer memory? 

The first problem: We always stored the object in a contiguous sequence.
For example, for a integer, the start of its memory is 0x01, then it may be span to 0x02, 0x03, 0x04…. 
The Second problem: Consider a w-bit question has a bit representation 
$x\_(w−1),x\_(w−2),...x_1,x_0$, in which $x\_(w-1)$ is the most significant bit. 
The former convention, where the least significant bit comes first is called little endian,
while now the most significant bit comes first is called big endian.

###### 2.1.4 Representing Strings

A string in C is encoded by an arrary of characters, which ended with the null(value is 0). 
Each characters is represented by some standard encoding, with the most common standard encoding
is ASCII code. Since we will get the same result by using ASCII code, independent of the byte
ordering and word size conventions. That's why we say text data are more platform independent than
binary data.

###### 2.1.5 Representing Code

Difference machine types use different and incompatible instructions and encodings.
Binary code is seldom portable across different combinations of machine and operating system.

###### 2.1.6 Introduction to Boolean Algebra

Boolean Algebra

~                &    0   1              |      0    1            ^     0   1
:---:           :----------:            :-------------:          :------------:
0  1             0    0   0              0      0    1            0     0   1
1  0             1    0   1              1      1    1            1     1   0

###### 2.1.7 Bit-level Operations in C

C supports bitwise Boolean operations. The best way to show the bit-level result is to convert 
hexadecimal to binary to do the operations and then convert back to hexadecimal.

Considering this example:
As an application of the property that a ^ a = 0 for any bit vector, we have the following program:

```C
void inplace_swap(int *x, int *y) {
	*y = *x ^ *y; /*Step 1*/
	*x = *x ^ *y; /*Step 2*/
	*y = *x ^ *y; /*Step 3*/
}
``` 

Step                    \*x            \*y
:-----------------------------------------:
Initially                a               b
Step 1                   a              a^b
Step 2          a^(a^b)=(a^a)^b=b       a^b
Step 3                   b          b^(a^b)=(b^b)^a=a

Here we do not need a third location to temporarily store one value while we are moving the other.
This procedure is to swap the values stored at the locations denoted by pointer variables x and y.
(classic application)

###### 2.1.8 Logical Operations in C

Logical Operators(||,&&,!) We should convert the expressions to be true or false before we involve
the logical operators, and then to calculate the logical operation.

Distinction between the logical operators versus their bit = level counterparts is that the logical
operators do no evaluate their second arguent if the result of the expression can be determined
by evaluating the first argument.

###### 2.1.9 Shift Operations in C



2.2 Integer Representations
---------------------------

##### 2.2.1 Integral Data Types

![sample post]({{site.baseurl}}/images/csappchp2/2.10.jpg)

##### 2.2.2 Unsigned Encodings

Assume we have an integer data type of w bits. We write a bit vector, or as 
$[x\_{w-1},x\_{w-2},...,x_0]$ to denote its individual bits. When we refer to 
such a unsigned number, the value of the number is 

$B2U\_w (\vec{x}) = sum\_{i=0}^{w-1} {x_i \*2^i}$ 

Why we can use binary represent decimal numbers?
The unsigned binary representation has the important property that every number between 0 and $2^w −1$
has a unique encoding as a w -bit value. For example, there is only one representation of decimal
value 11 as an unsigned, 4-bit number, namely [1011]. This property is captured in mathematical terms
by stating that **function B2Uw is a bijection** —it associates a unique value to each bit vector of length
w ; conversely, each integer between 0 and $2^w −1$ has a unique binary representation as a bit vector of
length w.

##### 2.2.3 Two's-Complement Encodings

Then in the above case, the max number of a w-bit number is [11111..11], while the min number
is [000…00]. However, for many applications, we wish to represent negative values as well.
The most common computer representation is called two’s complement form. This defined by 
interpretation the most significant bit of the word to served as the sign bit. 

The function is here below: 

![sample post]({{site.baseurl}}/images/csappchp2/2's-1.jpg)
![sample post]({{site.baseurl}}/images/csappchp2/2's-2.jpg)
![sample post]({{site.baseurl}}/images/csappchp2/2's-3.jpg)
![sample post]({{site.baseurl}}/images/csappchp2/2.14.jpg)

The C standards do not require signed integers to be represented in two’s-complement 
form, but nearly all machines do so.

![sample post]({{site.baseurl}}/images/csappchp2/2.A.jpg)

We set the sign bit to 1 when the value is negative, while set to 0 when the value is positive. 
Similarly, we have another two methods to represent an integer as follow.
Which is called one’s complement and sign-magnitude. 

##### 2.2.4 Conversions between Signed and Unsigned
Casting from shortint to unsignedshortchanged the numeric value, but not the bit representation. 
In casting from unsignedintto int , the underlying bit representa-tion stays the same.

![sample post]({{site.baseurl}}/images/csappchp2/2.5.jpg)
![sample post]({{site.baseurl}}/images/csappchp2/2.6.jpg)
![sample post]({{site.baseurl}}/images/csappchp2/2.15.jpg)
![sample post]({{site.baseurl}}/images/csappchp2/2.17.jpg)

##### 2.2.5 Signed versus Unsigned in C

Although the C standard does not specify a particu-lar representation of signed numbers, 
almost all machines use two’s complement. Generally, most numbers are signed by default.

C allows conversion between unsigned and signed. The rule is that the under-lying bit 
representation is not changed.

###### 2.2.6 Expanding the bit representation of a number: 

In some cases, such as convert a data type to a larger one, we should expand the bit 
representation of a number. 

**Unsigned:** Zero Extension 

**Signed:** Sign Bit Extension 

###### 2.2.7 Truncating Numbers: 

When truncating a w-bit number to a k-bit one, we just drop the high-order w-k bits. 
Truncating a number can alter its value, which is another type of overflow!

![sample post]({{site.baseurl}}/images/csappchp2/2.22.jpg)

2.3 Integer Arithmetic
----------------------

##### 2.3.1 Unsigned Addition

Unsigned arithmetic can be viewed as a form of modular arithmetic. 

\begin{eqnarray\*}
x+^{\upsilon}\_{\omega}y & = & \ x+y , x+y<2^{\omega} Normal\\
& = & \ x+y-2^{\omega}, 2^{\omega}\le(x+y)\le2^(\{omega+1}) Overlow
\end{eqnarray\*}

Modular addition forms Abelian group, which has the identity element 0. Two’s-Complement Arithmetic 
as we shown above, the Two’s-Complement Arithmetic may cause positive overflow or negative overflow, 
which will alter the value of the number! 

Multiply or divide by powers of 2: We can use left or right shift to accomplish this.

![sample post]({{site.baseurl}}/images/csappchp2/2.22.jpg)
![sample post]({{site.baseurl}}/images/csappchp2/2.23.jpg)

##### 2.3.3 Two's-Complement Addition

![sample post]({{site.baseurl}}/images/csappchp2/2.24.jpg)
![sample post]({{site.baseurl}}/images/csappchp2/2.25.jpg)

2.4 Floating Point
------------------

Floating Point representation encodes rational numbers of the form $V =x\*2^y$, which is useful
for performing computations involving very largr numbers. 

1985-IEEE754 Standard, under the sponsorship with the design of 8087.

**IEEE Floating-Point Representation:**

The sign s determines whether the number is negative or positive. 
The significant M is a fractional binary number that ranges ethier between 1 and 2-e or between 0 and 1-e. 
The exponent E weights the value by power of 2. 
The value encoded by a given bit representation can be divided into 3 cases, depending on the value of exp.

2.5 Summary
-----------