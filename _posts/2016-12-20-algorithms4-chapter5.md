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

**LSD string sort**
This algorithms is through key-index couting to sort strings, which from the right to the 
left of the string. Each characters as the key (similiar as the number of the group), we count
`W`(the length of the strings) times to sort them. This only could deal with the string we
have known the length of strings and all they have the same lenghtn, and it takes extra 
space for `count[]` and `aux[]`.

{% highlight java %}
public class LSD
{
	public static void sort(String[] a, int W)
 	{ // Sort a[] on leading W characters.
 		int N = a.length;
 		int R = 256;
 		String[] aux = new String[N];
 		for (int d = W-1; d >= 0; d--)
 		{ // Sort by key-indexed counting on dth char.
 			int[] count = new int[R+1]; // Compute frequency counts.
 			for (int i = 0; i < N; i++)
 				count[a[i].charAt(d) + 1]++;
 			for (int r = 0; r < R; r++) // Transform counts to indices.
 				count[r+1] += count[r];
 			for (int i = 0; i < N; i++) // Distribute.
 				aux[count[a[i].charAt(d)]++] = a[i];
 			for (int i = 0; i < N; i++) // Copy back.
 				a[i] = aux[i];
 		}
 	}
}

{% endhighlight %}

**Proposition B**
LSD string sort stably sorts fixed-length strings

**MSD string sort**
This is a general-purpose string sort, where strings are not necessarily all the same length, 
we consider the characters in left-to-right order. We know that strings that start with a 
should appear before strings that start with b, and so forth. The natural way to implement
this idea is a recursive method known as most-significant-digit-first (MSD) string sort. We 
use key-indexed counting to sort the strings according to their first character, then (recursively) 
sort the subarrays corresponding to each character.

We need to pay particular attention to reaching the ends of strings in MSD string sort. 
To convert from an indexed string character to an array index that returns -1 if the specified 
character position is past the end of the string. Then, we just add 1 to each returned value,this 
convention means that we have `R+1` different possible. Since we use key-index counting is still need 
an extra space, so `int count[] = new int[R + 2]`. We create a new count array after every recursive 
processes, so this methord will consum large space to deal with. We can chose insertion sort to handle
small arrarys to avoid the cost in examing all the characters that we have been examined before.
Space for the `count[]` array, on the other hand, can be an important issue (because it cannot 
be created outside the recursive `sort()` method) rather `an aux[]`.

the results of experiments where using a cutoff to insertion sort for subarrays of size 10 or 
less decreases the running time by a factor of 10 for a typical application.

A second pitfall for MSD string sort is that it can be relatively slow for subarrays containing large
numbers of equal keys. If a substring occurs sufficiently often that the cutoff for small subarrays 
does not apply, then a recursive call is needed for every character in all of the equal keys. The worst 
case for MSD string sorting is when all keys are equal. 

{% highlight java %}
public class MSD
{
	private static int R = 256; // radix
 	private static final int M = 15; // cutoff for small subarrays
 	private static String[] aux; // auxiliary array for distribution
 	private static int charAt(String s, int d)
 	{	if (d < s.length()) return s.charAt(d); else return -1; }
 	public static void sort(String[] a)
 	{
 		int N = a.length;
 		aux = new String[N];
 		sort(a, 0, N-1, 0);
 	}
 	private static void sort(String[] a, int lo, int hi, int d)
 	{ // Sort from a[lo] to a[hi], starting at the dth character.
 		if (hi <= lo + M)
 		{ Insertion.sort(a, lo, hi, d); return; }
 		int[] count = new int[R+2]; // Compute frequency counts.
 		for (int i = lo; i <= hi; i++)
 			count[charAt(a[i], d) + 2]++;
 		for (int r = 0; r < R+1; r++) // Transform counts to indices.
 			count[r+1] += count[r];
 		for (int i = lo; i <= hi; i++) // Distribute.
 			aux[count[charAt(a[i], d) + 1]++] = a[i];
 		for (int i = lo; i <= hi; i++) // Copy back.
 			a[i] = aux[i - lo];
 		// Recursively sort for each character value.
 		for (int r = 0; r < R; r++)
 			sort(a, lo + count[r], lo + count[r+1] - 1, d+1);
 	}
}
{% endhighlight %}

**Proposition C**
To sort N random strings from an R-character alphabet, MSD string sort 
examines about $N log_R N$ characters, on average.

**Proposition D**
MSD string sort uses between $8N+3R$ and $~7wN+3WR$ array accesses to sort N strings 
taken from an R-character alphabet, where w is the average
string length.

**Proposition D (continued)**
To sort N strings taken from an R-character alphabet,the amount of space needed 
by MSD string sort is proportional to R times the length of the longest 
string (plus N ), in the worst case.

**Three-way string quicksort**
We can also adapt quicksort to MSD string sorting by using 3-way partitioning on the 
leading character of the keys, moving to the next character on only the middle
subarray (keys with leading character equal to the partitioning character). 

3-way string quicksort adapts well to handling equal keys, keys with long common
prefixes, keys that fall into a small range, and small arrays—all situations where 
MSD string sort runs slowly. Of particular importance is that the partitioning adapts
to different kinds of structure in different parts of the key. Also, like quicksort, 
3-way string quicksort does not use extra space (other than the implicit stack
to support recursion), which is an important advantage over MSD string sort, which
requires space for both frequency counts and an auxiliary array. 


Three-way string quicksort
{% highlight java %}
public class Quick3string
{
	private static int charAt(String s, int d)
 	{ if (d < s.length()) return s.charAt(d); else return -1; }
 	public static void sort(String[] a)
 	{ sort(a, 0, a.length - 1, 0); }
 	private static void sort(String[] a, int lo, int hi, int d)
 	{
 		if (hi <= lo) return;
 		int lt = lo, gt = hi;
 		int v = charAt(a[lo], d);
 		int i = lo + 1;
 		while (i <= gt)
 		{
 			int t = charAt(a[i], d);
 			if (t < v) exch(a, lt++, i++);
 			else if (t > v) exch(a, i, gt--);
 			else i++;
 		}
 		// a[lo..lt-1] < v = a[lt..gt] < a[gt+1..hi]
 		sort(a, lo, lt-1, d);
 		if (v >= 0) sort(a, lt, gt, d+1);
 		sort(a, gt+1, hi, d);
 	}
}

{% endhighlignt %}

**Proposition E**
To sort an array of N random strings, 3-way string quicksort uses $~ 2N ln N$ 
character compares, on the average. 

**The important characteristics of the string-sort algorithms summary**
![sample post]({{site.baseurl}}/images/algorithms4/stringsortperformance.png)

5.2 Trses
---------
Trses sorting is amazing that has high performance in typical applications, even for huge tables:
* Search hits take time proportional to the length of the search key.
* Search misses involve examining only a few characters.

![sample post]({{site.baseurl}}/images/algorithms4/anatomyofatrie.png)

**Basicproperties**

* There is no characters for the root. The other nodes have one characters for each node.
* Pathing from the root to one node will get the strings.
* All the sub-nodes of each node has their different strings. 

![sample post]({{site.baseurl}}/images/algorithms4/triesearch.png)

![sample post]({{site.baseurl}}/images/algorithms4/triesearchexamples.png)

The insert, delete and find implementations are very easy by one level loop, that is for `i`th 
times loop can find the sub-trie through the characters before `i`. And then to deal with
such implementations, respectively.

![sample post]({{site.baseurl}}/images/algorithms4/trieconstructiontrace.png)

Trie symbol table
{% highlight java %}
public class TrieST<Value>
{
	private static int R = 256; // radix
	private Node root;          // root of trie
	private static class Node
	{
		private Object val;
        private Node[] next = new Node[R];
    }
	public Value get(String key)
	{
        Node x = get(root, key, 0);
        if (x == null) return null;
        return (Value) x.val;
	}
    private Node get(Node x, String key, int d)
    {  // Return value associated with key in the subtrie rooted at x.
        if (x == null) return null;
        if (d == key.length()) return x;
        char c = key.charAt(d); // Use dth key char to identify subtrie.
        return get(x.next[c], key, d+1);
	}
    public void put(String key, Value val)
    {  root = put(root, key, val, 0);  }
    private Node put(Node x, String key, Value val, int d)
    {  // Change value associated with key if in subtrie rooted at x.
        if (x == null) x = new Node();
        if (d == key.length()) {  x.val = val; return x; }
        char c = key.charAt(d); // Use dth key char to identify subtrie.
        x.next[c] = put(x.next[c], key, val, d+1);
        return x;
	} 
}
{% endhighlight %}


5.3 Substring search
--------------------



5.4 Regular Expressions
-----------------------



5.5 Data Compression
--------------------



