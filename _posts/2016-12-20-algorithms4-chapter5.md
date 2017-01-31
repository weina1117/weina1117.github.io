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

{% endhighlight %}

**Proposition E**
To sort an array of N random strings, 3-way string quicksort uses $~ 2N ln N$ 
character compares, on the average. 

**The important characteristics of the string-sort algorithms summary**
![sample post]({{site.baseurl}}/images/algorithms4/stringsortperformance.png)

5.2 Tries
---------
Tries sorting is amazing that has high performance in typical applications, even for huge tables:
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

Wildcard match in a trie
{% highlight java %}  
public Iterable<String> keysThatMatch(String pat)
{
	Queue<String> q = new Queue<String>();
    collect(root, "", pat, q);
    return q;
}
public void collect(Node x, String pre, String pat, Queue<String> q) {
     int d = pre.length();
     if (x == null) return;
     if (d == pat.length() && x.val != null) q.enqueue(pre);
     if (d == pat.length()) return;
     char next = pat.charAt(d);
     for (char c = 0; c < R; c++)
        if (next == '.' || next == c)
           collect(x.next[c], pre + c, pat, q);
}
{% endhighlight %}

Matching the longest prefix of a given string
{% highlight java %}
public String longestPrefixOf(String s)
{
	int length = search(root, s, 0, 0);
    return s.substring(0, length);
}
private int search(Node x, String s, int d, int length) {
     if (x == null) return length;
     if (x.val != null) length = d;
     if (d == s.length()) return length;
     char c = s.charAt(d);
     return search(x.next[c], s, d+1, length);
}
{% endhighlight %}

**Ternary search tries (TSTs)**
Ternary search trie(TSTs) help us to avoid the excessive space cost associated with R-way tries. 
In a TST, each node has a character, three links, and a value. The three links correspond to 
keys whose current characters are less than, equal to, or greater than the node’s character. 

![sample post]({{site.baseurl}}/images/algorithms4/tst.png)

The search,insert and delete implementations:
* To search, compare the first character in the key with the character at the root. 
  If it is less, we take the left link; 
  if it is greater, we take the right link; and 
  if it is equal, we take the middle link and move to the next search key character. 
  In each case, we apply the algorithm recursively. We terminate with a search miss if we encounter 
  a null link or if the node where the search ends has a null value, and we terminate with a search 
  hit if the node where the search ends has a non-null value. 
* To insert a new key, we search, then add new nodes for the characters in the tail of the key, just 
  as we did for tries.
* To delete, in a TST, we have to use BST node deletion to remove the node corresponding to the character.

TST symbol table 
{% highlight java %}
public class TST<Value>
{
	 
	private Node root;          // root of trie
	private class Node   
	{
	   char c;                 // character
	   Node left, mid, right;  // left, middle, and right subtries 
	   Value val;              // value associated with string
	}
	public Value get(String key)  // same as for tries (See Trie symbol talbe).
	private Node get(Node x, String key, int d)
	{
	   if (x == null) return null;
	   char c = key.charAt(d);
	   if      (c < x.c) return get(x.left,  key, d);
	   else if (c > x.c) return get(x.right, key, d);
	   else if (d < key.length() - 1)
                         return get(x.middle, key, d + 1)
	   else return x;
	}
	public void put(String key, Value val)
	{  root = put(root, key, val, 0);  }
	private Node put(Node x, String key, Value val, int d)
	{
	   char c = key.charAt(d);
	   if (x == null) { x = new Node(); x.c = c; }
	   if      (c < x.c) x.left  = put(x.left,  key, val, d);
	   else if (c > x.c) x.right = put(x.right, key, val, d);
	   else if (d < key.length() - 1)
                         x.mid   = put(x.mid, key, val, d+1);
	   else x.val = val;
	return x;
	}
}
{% endhighlight %}

![sample post]({{site.baseurl}}/images/algorithms4/performanceofstringsearch.png)

5.3 Substring search
--------------------

**Brute Force**
Brute force is a straightforward approach to solving a problem, usually directly 
based on the problem statement and definitions of the concepts involved.

Brute-force substring search
{% highlight java %}
public static int search(String pat, String txt)
{
	int M = pat.length();
    int N = txt.length();
    for (int i = 0; i <= N - M; i++)
    {
    	int j;
        for (j = 0; j < M; j++)
			if (txt.charAt(i+j) != pat.charAt(j))
            break;
		if (j == M) return i;  // found
	}
    return N;                 // not found
}
{% endhighlight %}

Alternate implementation of brute-force substring search (explicit backup)

{% highlight java %}
public static int search(String pat, String txt)
{
	int j, M = pat.length();
    int i, N = txt.length();
    for (i = 0, j = 0; i < N && j < M; i++)
    {
    	if (txt.charAt(i) == pat.charAt(j)) j++;
        else { i -= j; j = 0;  }
     }
     if (j == M) return i - M;  // found
     else            return N;  // not found
}
{% endhighlight %}

**Knuth-Morris-Pratt substring search**
The basic idea is whenever we detect a mismatch, we already know some of the 
characters in the text (since they matched the pattern charac- ters prior to 
the mismatch). We can take advantage of this information to avoid backing up 
the text pointer over all those known characters.

The insight of the KMP algorithm is that we can decide ahead of time exactly
 how to restart the search, because that decision depends only on the pattern.

Knuth-Morris-Pratt substring search
{% highlight java %}
public class KMP
{
	private String pat;
    private int[][] dfa;
    public KMP(String pat)
    {  // Build DFA from pattern.
        this.pat = pat;
        int M = pat.length();
        int R = 256;
        dfa = new int[R][M];
        dfa[pat.charAt(0)][0] = 1;
        for (int X = 0, j = 1; j < M; j++)
        {  // Compute dfa[][j].
           for (int c = 0; c < R; c++)
              dfa[c][j] = dfa[c][X];                   // Copy mismatch cases.
           dfa[pat.charAt(j)][j] = j+1;                // Set match case.
           X = dfa[pat.charAt(j)][X];                  // Update restart state.
		}
	}
    public int search(String txt)
    {  // Simulate operation of DFA on txt.
        int i, j, N = txt.length(), M = pat.length();
        for (i = 0, j = 0; i < N && j < M; i++)
           j = dfa[txt.charAt(i)][j];
        if (j == M) return i - M;  // found (hit end of pattern)
        else        return N;      // not found (hit end of text)
	}

    public static void main(String[] args)
	{
     String pat = args[0];
     String txt = args[1];
     KMP kmp = new KMP(pat);
     StdOut.println("text:    " + txt);
     int offset = kmp.search(txt);
     StdOut.print("pattern: ");
     for (int i = 0; i < offset; i++)
        StdOut.print(" ");
     StdOut.println(pat);
	}
}
{% endhighlight %}

**PropositionN**
Knuth-Morris-Pratt substring search accesses no more than $M+N$ characters to search
for a pattern of length $M$ in a text of length $N$.

The KMP algorithm provided the linear-time worst-case guarantee. But when backup is easy, 
we can do significantly better than KMP.

**Boyer-Moore substring search**
When backup in the text string is not a problem, here is a faster substring-searching method
by scanning the pattern from right to left when trying to match it against the text.

Boyer-Moore substring search (mismatched character heuristic)
{% highlight java %}
public class BoyerMoore
{
   private int[] right;
   private String pat;
   BoyerMoore(String pat)
   {	// Compute skip table.
      this.pat = pat;
      int M = pat.length();
      int R = 256;
      right = new int[R];
      for (int c = 0; c < R; c++)
         right[c] = -1;                   // -1 for chars not in pattern
      for (int j = 0; j < M; j++)         // rightmost position for
         right[pat.charAt(j)] = j;        //   chars in pattern
	}
	public int search(String txt)
	{	// Search for pattern in txt.
		int N = txt.length();
		int M = pat.length();
		int skip;
		for (int i = 0; i <= N-M; i += skip)
		{	// Does the pattern match the text at position i ?
			skip = 0;
		   	for (int j = M-1; j >= 0; j--)
		    	if (pat.charAt(j) != txt.charAt(i+j))
		    	{
		        	skip = j - right[txt.charAt(i+j)];
		        	if (skip < 1) skip = 1;
		        	break;
				}
			if (skip == 0) return i;         // found.   
		}
		return N;                            // not found.
	public static void main(String[] args)
	{
		String pat = args[0];
  	   	String txt = args[1];
  	   	KMP kmp = new KMP(pat);
  	   	StdOut.println("text:    " + txt);
  	   	int offset = kmp.search(txt);
  	   	StdOut.print("pattern: ");
  	   	for (int i = 0; i < offset; i++)
  	   	   StdOut.print(" ");
  	   	StdOut.println(pat);
  	}
}
{% endhighlight %}

In this substring search algorithm builds a table giving the rightmost occurrence in the 
pattern of each possible character. The search method scans from right to left in the pattern, 
skip- ping to align any character causing a mismatch with its rightmost occurrence in the pattern.


**PropertyO** 
On typical inputs, substring search with the Boyer-Moore mismatched character heuristic
uses $~N/M$ character compares to search for a pattern of length $M$ in a text of length $N$.

**Rabin-Karp fingerprint search** 
This method is a completely different approach to substring search that is based on hashing. 
We compute a hash function for the pattern and then look for a match by using the same hash 
function for each possible M-character substring of the text. If we find a text substring 
with the same hash value as the pattern, we can check for a match. 

The basic of this method implementation would be much slower than a brute-force search,
since the process is equivalent to storing the pattern in a hash table, then doing a search for each
substring of the text, but we do not need to reserve the memory for the hash table because it 
would have just one entry. Why is slower? Since computing a hash function that involves every character
is much more expensive than just comparing characters. But if we can do some preprocessing at first, that 
will be easy to compute hash functions for M-character substrings in constant time.

Basick plan
![sample post]({{site.baseurl}}/images/algorithms4/rabinkarp1.png)

Horner’s method, applied to modular hashing
{% highlight java %}
private long hash(String key, int M)
{	// Compute hash for key[0..M-1].
    long h = 0;
    for (int j = 0; j < M; j++)
    	h = (R * h + key.charAt(j)) % Q;
    return h;
}
{% endhighlight %}

Key idea.
The Rabin-Karp method is based on efficiently computing the hash function for position $i+1$ in the text,
given its value for position $i$. It follows directly from a simple mathematical formulation. The result 
is that we can effectively move right one position in the text in constant time, whether $M$ is 5 or 100
or 1,000.

![sample post]({{site.baseurl}}/images/algorithms4/rabinkarp2.png)
![sample post]({{site.baseurl}}/images/algorithms4/rabinkarp3.png)

Monte Carlo correctness.
Monte Carlo algorithm that has a guaranteed completion time but fails to output a correct answer with a 
small probability. The alternative method of checking for a match could be slow (it might amount to the 
brute-force algorithm, with a very small probability) but is guaranteed correct. Such an algorithm is 
known as a Las Vegas algorithm.

![sample post]({{site.baseurl}}/images/algorithms4/rabinkarp4.png)

Rabin-Karp fingerprint substring search 
{% highlight java %}
public class RabinKarp
{
	private String pat;            // pattern (only needed for Las Vegas)
	private long patHash;          // pattern hash value
	private int M;                 // pattern length
	private long Q;                // a large prime
	private int R = 256;           // alphabet size
	private long RM;               // R^(M-1) % Q
	public RabinKarp(String pat)
	{
		this.pat = pat;                // save pattern (only needed for Las Vegas)
		this.M = pat.length();
		Q = longRandomPrime();
		RM = 1;                        // See Exercise 5.3.33.
		for (int i = 1; i <= M-1; i++) 
	    	RM = (R * RM) % Q;         // Compute R^(M-1) % Q for use
		patHash = hash(pat, M);        //   in removing leading digit
	}
	public boolean check(int i)        // Monte Carlo (See text.)
	{	return true;  }                // For Las Vegas, check pat vs txt(i..i-M+1).

	private long hash(String key, int M)
	{	// Compute hash for key[0..M-1].
	    long h = 0;
	    for (int j = 0; j < M; j++)
	        h = (R * h + key.charAt(j)) % Q;
		return h;
	}	

	private int search(String txt)
	{	// Search for hash match in text.
	    int N = txt.length();
	    long txtHash = hash(txt, M);
	    if (patHash == txtHash) return 0;   // Match at beginning.
	    for (int i = M; i < N; i++)
	    {	// Remove leading digit, add trailing digit, check for match.
	    	txtHash = (txtHash + Q - RM*txt.charAt(i-M) % Q) % Q;
	    	txtHash = (txtHash*R + txt.charAt(i)) % Q;
	    	if (patHash == txtHash)
	        	if (check(i - M + 1)) return i - M + 1; // match
	    }
	    return N;                                    // no match found
	}
}
{% endhighlight %}

Cost summary for substring search implementations

![sample post]({{site.baseurl}}/images/algorithms4/rabinkarp5.png)

5.4 Regular Expressions
-----------------------

Examples of regular expressions
|          RE           |            matches          |       does not match      |
|:---------------------:|:---------------------------:|:-------------------------:|
| (A|B)(C|D)            |  AC AD BC BD                |  everyotherstring         |
| A(B|C)\*D             |  AD ABD ACD ABCCBD          |  BCD ADD ABCBC            |
|A\* | (A\*BA\*BA\*)\*  |  AAA BBAABB BABAAA          |  ABA BBB BABBAAA          |

Definition. A regular expression (RE) is either
* Empty
* A single character
* A regular expression enclosed in parentheses
* Two or more concatenated regular expressions
* Two or more regular expressions separated by the or operator (|)
* A regular expression followed by the closure operator (\*)

Definition (continued). Each RE represents a set of strings, defined as follows:
* The empty RE represents the empty set of strings, with 0 elements.
* A character represents the set of strings with one element, itself.
* An RE enclosed in parentheses represents the same set of strings as the RE
  without the parentheses.
* The RE consisting of two concatenated REs represents the cross product of
  the sets of strings represented by the individual components (all possible strings 
  that can be formed by taking one string from each and concatenating them, in the 
  same order as the REs).
* The RE consisting of the or of two REs represents the union of the sets 
  represented by the individual components.
* The RE consisting of the closure of an RE represents (the empty string) or the union
  of the sets represented by the concatenation of any number of copies of the RE.

Closure
The closure of a pattern is the language of strings formed by concatenating the pattern
with itself any number of times (including zero). We denote closure by placing a * after
the pattern to be repeated. Closure has higher precedence than concatenation.

Set-of-characters descriptors

|        name        |             notation          |            example            |
|:------------------:|:-----------------------------:|:-----------------------------:|
| wildcard           | .                             | A.B                           |
| specied set        | enclosed in []                | [AEIOU]\*                     |
| range              | enclosed in [] separated by - | [A-Z] [0-9]                   |
| complement         | enclosed in [] preceded by ^  | [^AEIOU]\*                    |
 
Closure shortcuts (for specifying the number of copies of the operand)

|   option  |   notation  | example shortcut for |   in language  |  not in language  |
|:---------:|:-----------:|:--------------------:|:--------------:|:-----------------:|
| atleast1  |  +          | (AB)+                |  AB ABABAB     | $\epsilon$ BBBAAA |
| 0 or 1    |  ?          | (AB)?                | $\epsilon$  AB | any other string  |
| specic    | count in {} | (AB){3}              | ABABAB         | any other string  |
| range     | rangein{}   | (AB){1-2}            | AB ABAB        | any other string  |

Typical regular expressions in applications (simplified versionscal)

|       context       |              regular            |    expression matches  |
|:-------------------:|:-------------------------------:|:----------------------:|
| substring search    | .\*NEEDLE.\*                    | A HAYSTACK NEEDLE IN   |
| phone number        | \([0-9]{3}\)\ [0-9]{3}-[0-9]{4} | (800) 867-5309         |
| Java identier       | [$\_A-Za-z][$_A-Za-z0-9]\*      | Pattern_Matcher        |
| genome marker       | gcg(cgg|agg)\*ctg               | gcgaggaggcggcggctg     |
| email address       | [a-z]+@([a-z]+\.)+(edu|com)     | rs@cs.princeton.edu    |

**Nondeterministic Finite-state Automaton (NFA)**
To handle regular expressions, we need a more powerful abstract machine. Because of 
the or operation, the automaton cannot determine whether or not the pattern could occur 
at a given point by examining just one character; indeed, because of closure, it cannot
even determine how many characters might need to be examined before a mismatch is 
discovered. The overview of our RE pattern matching algorithm is the nearly the same as
for KMP:
* Build the NFA corresponding to the given RE.
* Simulate the operation of that NFA on the given text.
Kleene’s Theorem, a fundamental result of theoretical computer science, asserts 
that there is an NFA corresponding to any given RE (and vice versa). 






5.5 Data Compression
--------------------



