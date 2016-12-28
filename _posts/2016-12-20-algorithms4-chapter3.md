---
layout: post
title: Algorithms 4th Chapter 3
---

Chapter 3 Searching
----------------------

This chapter will cover these five main topics:

- [3.1 Symbol Tables](#31-symbol-tables)
- [3.2 Binary Search Trees](#32-binary-search-trees)
- [3.3 Balanced Search Trees](#33-balanced-search-trees)
- [3.4 Hash Tables](#34-hash-tables)
- [3.5 Applications](#35-applications)

3.1 Symbol Tables
-----------------

**Symbol table**
The symbol table is data structure and its purpose is to associate a value with a key.

It supports two operations: insert (put) a new pair into the table 
                            search for (get) the value associated with a given key.

Several design choices for the implementations:
* Generics. 
  We consider the methods without specifying the types of keys and values being
   processed, using generics.
* Duplicate keys.
  No duplicate keys in a table since the new value will replaces the old one. Only one 
  value is associated with each key. 
* Null keys. 
  Keys must not be `null`. As with many mechanisms in Java, use of a `null` key results in
  an exception at runtime.
* Null values. 
  No key can be associated with the value `null`. That means `get()` should return `null`
  for keys not in the table. Two (intended) consequences: First, we can test whether or not 
  the symbol table defines a value associated with a given key by testing whether `get()` 
  returns `null`. Second, we can use the operation of calling `put()`with null as its second 
  (value) argument to implement deletion.
* Deletion. 
  Two types of deletion: lazy deletion, at first we associate keys in the table with null, 
  then perhaps remove all such keys at some later time, the code `put(key, null)` is an 
  easy (lazy) implementation of `delete(key)`. Eager deletion, where we remove 
  the key from the table immediately and the implementation of `delete()`, it is intended
  to replace this default.
* Iterators.
  The `keys()` method returns an Iterable<Key> object for clients to use to iterate through the keys.
* Key equality.
  Java requires that all objects implement an `equals()` method and provides implementations both for
   standard types such as `Integer`, `Double`, and `String` and for more complicated types such as `Date`, 
   `File` and `URL`. For applications involving these types of data, you can just use the built-in 
   implementation. 

**Ordered symbol tables** 
  In typical applications, keys are `Comparable` objects, so the option exists of using the code
  `a.compareTo(b)` to compare two keys `a` and `b`. Several symbol-table implementations take advantage
   of order among the keys that is implied by `Comparable` to provide efficient implementations of the 
  `put()` and `get()` operations.

**Searching cost model**
  That is we count compares (equality tests or key comparisons). In (rare) cases where compares are not 
  in the inner loop, we count array accesses. 

A symbol-table client

{% highlight java %}
public class FrequencyCounter
{
	public static void main(String[] args)
 	{
 		int minlen = Integer.parseInt(args[0]); // key-length cutoff
 		ST<String, Integer> st = new ST<String, Integer>();
 		while (!StdIn.isEmpty())
 		{ // Build symbol table and count frequencies.
 			String word = StdIn.readString();
 			if (word.length() < minlen) continue; // Ignore short keys.
 			if (!st.contains(word)) st.put(word, 1);
 			else st.put(word, st.get(word) + 1);
 		}
 		// Find a key with the highest frequency count.
 		String max = "";
 		st.put(max, 0);
 		for (String word : st.keys())
 			if (st.get(word) > st.get(max))
 				max = word;
 		StdOut.println(max + " " + st.get(max));
 	}
}

{% endhighlight %}

FrequencyCounter is surrogate for a very common situation. Specifically, it has 
the following characteristics, which are shared by many other symbol-table clients:
* Search and insert operations are intermixed.
* The number of distinct keys is not small.
* Substantially more searches than inserts are likely.
* Search and insert patterns, though unpredictable, are not random

Sequential search (in an unordered linked list)

{% highlight java %}
public class SequentialSearchST<Key, Value>
{
	private Node first; // first node in the linked list
 	private class Node
 	{ // linked-list node
 		Key key;
 		Value val;
 		Node next;
 		public Node(Key key, Value val, Node next)
 		{
 			this.key = key;
 			this.val = val;
 			this.next = next;
 		}
 	}

 	public Value get(Key key)
 	{ // Search for key, return associated value.
 		for (Node x = first; x != null; x = x.next)
 			if (key.equals(x.key))
 				return x.val; // search hit
 		return null; // search miss
 	}
 	
	public void put(Key key, Value val)
 	{ // Search for key. Update value if found; grow table if new.
 		for (Node x = first; x != null; x = x.next)
 			if (key.equals(x.key))
 			{ x.val = val; return; } // Search hit: update val.
 		first = new Node(key, val, first); // Search miss: add new node.
 	}
}

{% endhighlight %}

**Proposition A**
Unsuccessful search and insert in an unordered linked-list symbol table both use $N$ compares, 
and successful search uses $N$ compares in the worst case. In particular, inserting $N$ keys 
into an initially empty linked-list symbol table uses $~N^2/2$ compares.

**Binary search in an ordered array**
The heart of the implementation is the `rank()` method, which returns the number of keys smaller
than a given key. For `get()`, the rank tells us precisely where the key is to be found if it is
in the table (and, if it is not there, that it is not in the table). For `put()`, the rank tells
us precisely where to update the value when the key is in the table, and precisely where to put 
the key when the key is not in the table. We move all larger keys over one position to make room
(working from back to front) and insert the given key and value into the proper positions in 
their respective arrays.

**Proposition B**
Binary search in an ordered array with $N$ keys uses no more than $lg N + 1$ compares for a search
(successful or unsuccessful) in the worst case.

**Proposition C** 
Inserting a new key into an ordered array uses $~ 2N$ array accesses in the worst case, so inserting
$N$ keys into an initially empty table uses $~ N^2$ array accesses in the worst case.

3.2 Binary Search Trees
------------------------

Binary Search Tree (BST) is a binary tree where each node has a Comparable key (and an associated 
value) and satisfies the restriction that the key in any node is larger than the keys in all nodes in
that node's left subtree and smaller than the keys in all nodes in that node's right subtree. Each node
contains a key, a value, a left link, a right link, and a node count. The left link points to a BST for
items with smaller keys, and the right link points to a BST for items with larger keys. The instance 
variable $N$ gives the node count in the subtree rooted at the node. The running times of algorithms 
on binary search trees depend on the shapes of the trees, which, in turn, depends on the order in 
which keys are inserted.

* Search. 
A recursive algorithm to search for a key in a BST follows immediately from the recursive structure: If
the tree is empty, we have a search miss; if the search key is equal to the key at the root, we have a 
search hit. Otherwise, we search (recursively) in the appropriate subtree. The recursive `get()` method
implements this algorithm directly. It takes a node (root of a subtree) as first argument and a key as 
second argument, starting with the root of the tree and the search key.

* Insert. 
Insert is not much more difficult to implement than search. Indeed, a search for a key not in the tree 
ends at a null link, and all that we need to do is replace that link with a new node containing the key. 
The recursive `put()` method accomplishes this task using logic similar to that we used for the recursive
search: If the tree is empty, we return a new node containing the key and value; if the search key is 
less than the key at the root, we set the left link to the result of inserting the key into the left 
subtree; otherwise, we set the right link to the result of inserting the key into the right subtree.

Binary search tree symbol table

{% highlight java %}
public class BST<Key extends Comparable<Key>, Value>
{
	private Node root; // root of BST
 	
	private class Node
 	{
 		private Key key; // key
 		private Value val; // associated value
 		private Node left, right; // links to subtrees
 		private int N; // # nodes in subtree rooted here
 	
		public Node(Key key, Value val, int N)
 		{ this.key = key; this.val = val; this.N = N; }
 	}
 	public int size()
 	{ return size(root); }
 	
	private int size(Node x)
 	{
 		if (x == null) return 0;
 		else return x.N;
 	}
 	
	// See page 399 for public Value get(Key key)
	// See page 399 for public void put(Key key, Value val)
	public Value get(Key key)
	{ return get(root, key); }
	private Value get(Node x, Key key)
	{ // Return value associated with key in the subtree rooted at x;
	 // return null if key not present in subtree rooted at x.
		if (x == null) return null;
		int cmp = key.compareTo(x.key);
		if (cmp < 0) return get(x.left, key);
		else if (cmp > 0) return get(x.right, key);
		else return x.val;
	}
	public void put(Key key, Value val)
	{ // Search for key. Update value if found; grow table if new.
		root = put(root, key, val);
	}
	private Node put(Node x, Key key, Value val)
	{
	// Change key’s value to val if key in subtree rooted at x.
	// Otherwise, add new node to subtree associating key with val.
		if (x == null) return new Node(key, val, 1);
		int cmp = key.compareTo(x.key);
		if (cmp < 0) x.left = put(x.left, key, val);
		else if (cmp > 0) x.right = put(x.right, key, val);
		else x.val = val;
		x.N = size(x.left) + size(x.right) + 1;
		return x;
	}

 	// See page 407 for min(), max(), floor(), and ceiling().
	public Key min()
	{
		return min(root).key;
	}
	private Node min(Node x)
	{
		if (x.left == null) return x;
		return min(x.left);
	}
	public Key floor(Key key)
	{
		Node x = floor(root, key);
	 	if (x == null) return null;
	 	return x.key;
	}
	private Node floor(Node x, Key key)
	{
		if (x == null) return null;
	 	int cmp = key.compareTo(x.key);
	 	if (cmp == 0) return x;
	 	if (cmp < 0) return floor(x.left, key);
	 	Node t = floor(x.right, key);
	 	if (t != null) return t;
	 	else return x;
	}

	// See page 409 for select() and rank().
	public Key select(int k)
	{
		return select(root, k).key;
	}
	private Node select(Node x, int k)
	{	// Return Node containing key of rank k.
		if (x == null) return null;
	 	int t = size(x.left);
	 	if (t > k) return select(x.left, k);
	 	else if (t < k) return select(x.right, k-t-1);
	 	else return x;
	}
	public int rank(Key key)
	{ return rank(key, root); }
	private int rank(Key key, Node x)
	{	// Return number of keys less than x.key in the subtree rooted at x.
		if (x == null) return 0;
	 	int cmp = key.compareTo(x.key);
	 	if (cmp < 0) return rank(key, x.left);
	 	else if (cmp > 0) return 1 + size(x.left) + rank(key, x.right);
	 	else return size(x.left);
	}
	

 	// See page 411 for delete(), deleteMin(), and deleteMax().
	public void deleteMin()
	{
		root = deleteMin(root);
	}
	private Node deleteMin(Node x)
	{
	 	if (x.left == null) return x.right;
		x.left = deleteMin(x.left);
	 	x.N = size(x.left) + size(x.right) + 1;
		return x;
	}
	public void delete(Key key)
	{	root = delete(root, key); }
	private Node delete(Node x, Key key)
	{
		if (x == null) return null;
	 	int cmp = key.compareTo(x.key);
	 	if (cmp < 0) x.left = delete(x.left, key);
	 	else if (cmp > 0) x.right = delete(x.right, key);
	 	else
	 	{
	 		if (x.right == null) return x.left;
	 		if (x.left == null) return x.right;
	 		Node t = x;
	 		x = min(t.right); // See page 407.
	 		x.right = deleteMin(t.right);
	 		x.left = t.left;
	 	}
	 	x.N = size(x.left) + size(x.right) + 1;
	 	return x;
	}

 	// See page 413 for keys().
	public Iterable<Key> keys()
	{	return keys(min(), max()); }
	public Iterable<Key> keys(Key lo, Key hi)
	{
	 	Queue<Key> queue = new Queue<Key>();
	 	keys(root, queue, lo, hi);
	 	return queue;
	}
	private void keys(Node x, Queue<Key> queue, Key lo, Key hi)
	{
		if (x == null) return;
	 	int cmplo = lo.compareTo(x.key);
	 	int cmphi = hi.compareTo(x.key);
	 	if (cmplo < 0) keys(x.left, queue, lo, hi);
	 	if (cmplo <= 0 && cmphi >= 0) queue.enqueue(x.key);
	 	if (cmphi > 0) keys(x.right, queue, lo, hi);
	}
}

{% endhighlight %}

**Proposition C**
Search hits in a BST built from $N$ random keys requires $~ 2 ln N$ (about $1.39 lg N$) compares on
 the average.

**Proposition D**
Insertion and search misses in a BST built from $N$ random keys requires $~ 2 ln N$ (about $1.39 lg N$) 
compares on the average.roposition.

**Order-based methods and deletion**
An important reason that BSTs are widely used is that they allow us to keep the keys in order. 

* Minimum and maximum.
If the left link of the root is null, the smallest key in a BST is the key at the root; 
if the left link is not null, the smallest key in the BST is the smallest key in the subtree rooted at the 
node referenced by the left link. Finding the maximum key is similar, moving to the right instead of to the left.

* Floor and ceiling. 
If a given key key is less than the key at the root of a BST, then the floor of key (the largest key in 
the BST less than or equal to key) must be in the left subtree. If key is greater than the key at the root, 
then the floor of key could be in the right subtree, but only if there is a key smaller than or equal to key
in the right subtree; if not (or if key is equal to the key at the root) then the key at the root is the floor
of key. Finding the ceiling is similar, interchanging right and left.

* Selection. 
Suppose that we seek the key of rank $k$ (the key such that precisely $k$ other keys in the BST are smaller). If the 
number of keys $t$ in the left subtree is larger than $k$, we look (recursively) for the key of rank $k$ in the left 
subtree; if $t$ is equal to $k$, we return the key at the root; and if $t$ is smaller than $k$, we look (recursively)
 for the key of rank $k - t - 1$ in the right subtree.

* Rank. 
If the given key is equal to the key at the root, we return the number of keys $t$ in the left subtree; if the given
key is less than the key at the root, we return the rank of the key in the left subtree; and if the given key is 
larger than the key at the root, we return $t + 1$ (to count the key at the root) plus the rank of the key in the 
right subtree.

* Delete the minimum and maximum. 
For delete the minimum, we go left until finding a node that that has a null left link and then replace the link to
 that node by its right link. The symmetric method works for delete the maximum.

* Delete. what can we do to delete a node that has two two links?
The steps as follow:
	* Save a link to the node to be deleted in `t`.
	* Set `x` to point to its successor `min(t.right)`.
	* Set the right link of `x` (which is supposed to point to the BST containing all the keys larger than `x.key`)
      to deleteMin(`t.right`), the link to the BST containing all the keys that are larger than `x.key` after the deletion.
	* Set the left link of `x` (which was null) to `t.left` (all the keys that are less than both the deleted key 
      and its successor).

* Range search. 
To implement the `keys()` method that returns the keys in a given range, we begin with a basic 
recursive BST traversal method, known as inorder traversal. To illustrate the method, we consider 
the task of printing all the keys in a BST in order. To do so, print all the keys in the left 
subtree (which are less than the key at the root by definition of BSTs), then print the key at 
the root, then print all the keys in the right subtree, (which are greater than the key at the 
root by definition of BSTs).

**Proposition E**
In a BST, all operations take time proportional to the height of the tree, in the worst case.

3.3 Balanced Search Trees
-------------------------

Balanced Search Trees is a type of binary search tree where costs are guaranteed to be logarithmic. 
These trees have near-perfect balance, where the height is guaranteed to be no larger than 2 lg N.

**2-3 search trees**
To guarantee balance in search trees is to allow the nodes to hold more than one key. This means that 
a 2-3 search tree is a tree that either is empty or:
* A 2-node, with one key and two links, a left link to a 2-3 search tree with smaller keys, 
  and a right link to a 2-3 search tree with larger keys
* A 3-node, with two keys and three links, a left link to a 2-3 search tree with smaller keys, 
  a middle link to a 2-3 search tree with keys between the node's keys and a right link to a 2-3 search tree
  with larger keys.
A perfectly balanced 2-3 search tree (or 2-3 tree for short) is one whose null links are all the 
same distance from the root.

![sample post]({{site.baseurl}}/images/algorithms4/23tree-anatomy.png)

* Search

![sample post]({{site.baseurl}}/images/algorithms4/23tree-1.png)

![sample post]({{site.baseurl}}/images/algorithms4/23tree-2.png)

* Insert into a 2-node

![sample post]({{site.baseurl}}/images/algorithms4/23tree-3.png)

* Insert into a tree consisting of a single 3-node

![sample post]({{site.baseurl}}/images/algorithms4/23tree-4.png)

* Insert into a 3-node whose parent is a 2-node

![sample post]({{site.baseurl}}/images/algorithms4/23tree-5.png)

* Insert into a 3-node whose parent is a 3-node

![sample post]({{site.baseurl}}/images/algorithms4/23tree-6.png)

* Splitting the root

![sample post]({{site.baseurl}}/images/algorithms4/23tree-7.png)

* Local transformations
  The basis of the 2-3 tree insertion is that all of these transformations are purely local: 
  No part of the 2-3 tree needs to be examined or modified other than the specified nodes and links.
  The number of links changed for each transformation is bounded by a small constant. Each of the 
  transformations passes up one of the keys from a 4-node to that nodes parent in the tree, and then
  restructures links accordingly, without touching any other part of the tree.

* Global properties
  These local transformations preserve the global properties that the tree is ordered and balanced: 
  the number of links on the path from the root to any null link is the same.

**Proposition**
Search and insert operations in a $2-3$ tree with $N$ keys are guaranteed to visit at most $lg N$ nodes.

![sample post]({{site.baseurl}}/images/algorithms4/23tree-8.png)

**Red-black BSTs**
A simple representation known as a red-black BST.

* Encoding 3-nodes
To adding extra information to encode 3-nodes by starting with standard BSTs, whick are made up of 
2-nodes, there are two different types: red links, which bind together two 2-nodes to represent 3-nodes,
and black links, which bind together the 2-3 tree. Specifically, we represent 3-nodes as two 2-nodes 
connected by a single red link that leans left. We refer to BSTs that represent 2-3 trees in this way 
as red-black BSTs. One advantage of using such a representation is that it allows us to use our `get()` 
code for standard BST search without modification.

![sample post]({{site.baseurl}}/images/algorithms4/redblack-1.png)

A given any 2-3 tree, we can immediately derive a corresponding red-black BST, just by converting 
each node as specified. Conversely, if we draw the red links horizontally in a red-black BST, all 
of the null links are the same distance from the root, and if we then collapse together the nodes 
connected by red links, the result is a 2-3 tree.

![sample post]({{site.baseurl}}/images/algorithms4/redblack-2.png)

* Color representation
Since each node is pointed to by one link from its parent, we encode the color of links in nodes,
by adding a boolean instance variable color to our node, which is true if the link from the parent 
is red and false if it is black. By convention, null links are black.

![sample post]({{site.baseurl}}/images/algorithms4/redblack-3.png)

* Rotations
This operation called rotation that switches orientation of red links. First, suppose that we have 
a right-leaning red link that needs to be rotated to lean to the left. This operation is called a 
left rotation. Implementing a right rotation that converts a left-leaning red link to a right-leaning
one amounts to the same code, with left and right interchanged.

![sample post]({{site.baseurl}}/images/algorithms4/redblack-4.png)

![sample post]({{site.baseurl}}/images/algorithms4/redblack-5.png)

* Flipping colors
The color flip operation flips the colors of the the two red children to black and the color of 
the black parent to red.

![sample post]({{site.baseurl}}/images/algorithms4/redblack-9.png)

![sample post]({{site.baseurl}}/images/algorithms4/redblack-6.png)

![sample post]({{site.baseurl}}/images/algorithms4/redblack-7.png)

**Proposition**
The height of a red-blackBST with $N$ nodes is no more than $2 lg N$.

**Proposition**
In a red-black BST, the following operations take logarithmic time in the worst case: search, 
insertion, finding the minimum, finding the maximum, floor, ceiling, rank, select, delete the 
minimum, delete the maximum, delete, and range count.

The average length of a path from the root to a node in a red-black BST with $N$ nodes is $~1.00 lg N$.

3.4 Hash Tables
---------------

We reference key-value pairs using arrays by doing arithmetic operations to transform keys into array indices.
There are two steps throught hashing to do search algorithmes. The first step is to compute a hash function 
that transforms the search key into an array index. Ideally, different keys would map to different indices. 
But in the real case, we have to face the possibility that two or more different keys may hash to the same 
array index. Thus, the second part of a hashing search is a collision-resolution process that deals with this 
situation.

![sample post]({{site.baseurl}}/images/algorithms4/hashing1.png)

**Hash functions**
If we have an array that can hold $M$ key-value pairs, then we need a function that can transform any given key
into an index into that array: an integer in the range $[0, M-1]$. We seek a hash function that is both easy 
to compute and uniformly distributes the keys.

* Positive integers 
The most commonly used method for hashing integers is called modular hashing: we choose the array size $M$ 
to be prime, and, for any positive integer key $k$, compute the remainder when dividing $k$ by $M$. This 
function is very easy to compute ($k % M$, in Java), and is effective in dispersing the keys evenly 
between $0$ and $M-1$.

* Floating-point numbers
If the keys are real numbers between $0$ and $1$, we might just multiply by $M$ and round off to the nearest 
integer to get an index between $0$ and $M-1$. Although it is intuitive, this approach is defective because 
it gives more weight to the most significant bits of the keys; the least significant bits play no role. One
way to address this situation is to use modular hashing on the binary representation of the key 
(this is what Java does).

* Strings
Modular hashing works for long keys such as strings, too: we simply treat them as huge integers. For example, 
the code below computes a modular hash function for a String s, where R is a small prime integer (Java uses 31).

{% highlight java %}
int hash = 0;
for (int i = 0; i < s.length(); i++)
    hash = (R * hash + s.charAt(i)) % M;
{% endhighlight %}

* Compound keys
If the key type has multiple integer fields, we can typically mix them together in the way just described 
for String values. For example, suppose that search keys are of type `USPhoneNumber.java`, which has three 
integer fields area (3-digit area code), exch (3-digit exchange), and ext (4-digit extension). In this case,
 we can compute the number

{% highlight java %}

int hash = (((area * R + exch) % M) * R + ext) % M; 

{% endhighlight %}

* Java conventions
In Java every type of data needs a hash function by requiring that every data type must implement a 
method called `hashCode()` (which returns a 32-bit integer). The implementation of `hashCode()` for 
an object must be consistent with equals. That is, if `a.equals(b)` is true, then `a.hashCode()` must
have the same numerical value as `b.hashCode()`. If the `hashCode()` values are the same, the objects 
may or may not be equal, and we must use `equals()` to decide which condition holds.

* Converting a `hashCode()` to an array index
Since our goal is an array index, not a $32-bit$ integer, we combine `hashCode()` with modular hashing
 in our implementations to produce integers between $0$ and $M-1$ as follows:

{% highlight java %}
private int hash(Key key) {
   return (key.hashCode() & 0x7fffffff) % M;
}
{% endhighlight %}

The code masks off the sign bit (to turn the 32-bit integer into a 31-bit nonnegative integer) and then 
computing the remainder when dividing by $M$, as in modular hashing.

* User-defined `hashCode()`. 
For any object `x`, we can write `x.hashCode()` and, in principle, expect to get any one of the $2^32$ 
possible 32-bit values with equal likelihood. Java provides `hashCode()` implementations that aspire to 
this functionality for many common types (including `String, Integer, Double, Date, and URL`), but for 
our own type, we have to try to do it on our own.
 
We have three primary requirements in implementing a good hash function for a given data type:
* It should be deterministic—equal keys must produce the same hash value.
* It should be efficient to compute.
* It should uniformly distribute the keys.

**Assumption J (uniform hashing assumption)** 
The hash function that we use uniformly distributes keys among the integer values between $0$ and $M-1$.

![sample post]({{site.baseurl}}/images/algorithms4/hashing2.png)

**Proposition K** 
In a separate-chaining hash table with $M$ lists and $N$ keys, the probability (under Assumption J) that 
the number of keys in a list is within a small constant factor of $N/M$ is extremely close to $1$. of 
$N/M$ is extremely close to $1$. (Assumes an idealistic hash function.) This classical mathematical result 
is compelling, but it completely depends on Assumption J. Still, in practice, the same behavior occurs.

**Property L**
In a separate-chaining hash table with $M$ lists and $N$ keys, the number of compares (equality tests) for
search and insert is proportional to $N/M$.

**Hashing with linear probing**
Another approach to implementing hashing is to store $N$ key-value pairs in a hash table of size $M > N$, 
relying on empty entries in the table to help with with collision resolution. Such methods are called 
open-addressing hashing methods. The simplest open-addressing method is called linear probing: when there 
is a collision (when we hash to a table index that is already occupied with a key different from the search 
key), then we just check the next entry in the table (by incrementing the index). There are three possible 
outcomes:

* key equal to search key: search hit
* empty position (null key at indexed position): search miss
* key not equal to search key: try next entry

![sample post]({{site.baseurl}}/images/algorithms4/hashing3.png)

**Proposition M** 
In a linear-probing has table of size $M$ with $N = α M$ keys, the average number of probes (under Assumption J) 
is $~ 1/2 (1 + 1 / (1 - α))$ for search hits and $~ 1/2 (1 + 1 / (1 - α)^2)$ for search misses or inserts.

3.5 Applications
----------------
