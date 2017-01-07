---
layout: post
title: Algorithms 4th Chapter 5
---

Chapter 5 Strings
----------------------

This chapter will cover these four main topics:

- [5.1 String Sorts](#51-string-sorts)
- [5.2 Tries](#52-tries)
- [5.3 Substring search](#53-substring-search)
- [5.4 Regular Expressions](#54-regular-expressions)
- [5.5 Data Compression](#55-data-compression)

**Review the most important characteristics of Strings**

* Characters. 
A String is a sequence of characters. Characters are of type char and can have 
one of $2^16$ possible values. 

* Immutability. 
String objects are immutable, so that we can use them in assignment statements 
and as arguments and return values from methods without having to worry about 
their values changing. 

* Indexing. 
Java Srting class provides the `charAt()` method that is extract a specified character
from a string. We expect `charAt()` to complete its work in constant time, as if the 
string were stored in a `char[]` array. 
* Length. 
In Java, the find the length of a string operation is implemented in the `length()` 
method in String. We expect `length()` to complete its work in constant time as well.

* Substring. 
Java’s `substring()` method implements the extract a specified substring operation.
We expect a constant-time implementation of this method, again.

* Concatenation. 
In Java, the create a new string formed by appending one string to another operation 
is a built-in operation (using the `+` operator) that takes time proportional to the 
length of the result. 

* Character arrays. 
An array of char values are prefered in many of the algorithms, because it consumes 
less space and takes less time. 

Two ways to represent strings in Java

|          operation        |      array of characters    |        Java string      |
|:--------------------------|:---------------------------:|:-----------------------:|
|        declare            |          char[] a           |         String s        | 
|  indexed character access |          a[i]               |       s.charAt(i)       |
|        length             |          a.length           |         s.length()      |
|        convert            |       a = s.toCharArray();  |     s = new String(a);  |

5.1 String Sorts
----------------

Here covers two methods:
* The first approach examines the characters in the keys in a right-to-left order. Such
methods are generally referred to as least-significant-digit (LSD) string sorts.

* The second approach examines the characters in the keys in a left-to-right order, working 
with the most significant character first, most-significant-digit (MSD) string sorts.
MSD string sorts are attractive because they can get a sorting job done without necessarily 
examining all of the input characters. MSD string sorts are similar to quicksort. The 
difference is that MSD string sorts use just the first character of the sort key to do the 
partitioning, while quicksort uses comparisons that could involve examining the whole key.

**Key-indexed counting**

Key-indexed counting is an extremely effective and often overlooked sorting method for 
applications where keys are small integers. There are four steps:

* Compute frequency counts. 
The first step is to count the frequency of occurrence of each key value, using an int 
array `count[]`.  For each item, we use the key to access an entry in `count[]` and 
increment that entry. If the key value is `r`, we increment `count[r+1]`. (Why `+1`? The reason 
for that will become clear in the next step.)

* Transform counts to indices. 
Next, we use `count[]` to compute, for each key value, the starting index positions in 
the sorted order of items with that key. For each key value `r`, the sum of the counts for key
 values less than `r+1` is equal to the sum of the counts for key values less than `r` plus `count[r]`, 
so it is easy to proceed from left to right to transform `count[]` into an index table that we 
can use to sort the data. 

* Distribute the data. 
With the `count[]` array transformed into an index table, we accomplish the actual sort 
by moving the items to an auxiliary array aux[].We move each item to the position in `aux[]` 
indicated by the `count[]` entry corresponding to its key, and then increment that entry to 
maintain the following invariant for `count[]`: for each key value `r`, `count[r]` is the 
index of the position in `aux[]` where the next item with key value `r` (if any) should be placed. 

* Copy back. 
The last step is to copy the sorted result back to the original array. 

**Proposition A**
Key-indexed counting uses $8N+3R+1$ array accesses to stably sort $N$ items whose keys are 
integers between $0$ and $R-1$.

Proposition A implies that key-indexed counting breaks through the $NlogN$ lower. Because 
(when data is accessed only through `compareTo()`) in key-indexed counting does no compares
(it accesses data only through `key()`) When `R` is within a constant factor of `N`, we have
a linear-time sort.

Key-indexed counting (a[].key is an int in [0, R)
{% highlight java %}
int N = a.length;

String[] aux = new String[N];
int[] count = new int[R+1];

// Compute frequency counts.
for (int i = 0; i < N; i++)
	count[a[i].key() + 1]++;

// Transform counts to indices.
for (int r = 0; r < R; r++)
	count[r+1] += count[r];

// Distribute the records.
for (int i = 0; i < N; i++)
	aux[count[a[i].key()]++] = a[i];

// Copy back.
for (int i = 0; i < N; i++)
	a[i] = aux[i];
{% endhighlight %}

**Proposition B**
LSD string sort stably sorts fixed-length strings

5.2 Trses
---------



5.3 Substring search
--------------------



5.4 Regular Expressions
-----------------------



5.5 Data Compression
--------------------



