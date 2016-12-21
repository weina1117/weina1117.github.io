---
layout: post
title: Algorithms 4th Chapter 2
---

Chapter 2 Sorting
----------------

This chapter will cover these five main topics:

- [2.1 Elementary Sorts](#21-elementary-sorts)
- [2.2 Mergesort](#22-mergesort)
- [2.3 Quicksort](#23-quicksort) 
- [2.4 Priority Queues](#24-priority-queues)
- [2.5 Applications ](#25-applications)

2.1 Elementary Sorts
---------------------

Rules of the game: 
Our primary concern is algorithms for rearranging arrays of items, and each item 
contains a key. The purpose of sorting is to rearrange the items through their keys
 are in certain order. In Java, the abstract notion of a key is captured in a 
built-in mechanism—the `Comparable` interface. With but a few exceptions, our sort code 
refers to the data only through two operations: the method `less()` that compares 
objects and the method `exch()` that exchanges them.

Template for sort classes:

{% highlight java %}

public class Example
{
	public static void sort(Comparable[] a)
	{ /* See Algorithms 2.1, 2.2, 2.3, 2.4, 2.5, or 2.7. */ }
	
	private static boolean less(Comparable v, Comparable w)
	{ return v.compareTo(w) < 0; }
	 
	private static void exch(Comparable[] a, int i, int j)
	{ Comparable t = a[i]; a[i] = a[j]; a[j] = t; }
	
	private static void show(Comparable[] a)
	{ // Print the array, on a single line.
		for (int i = 0; i < a.length; i++)
			StdOut.print(a[i] + " ");
		StdOut.println();
	}

	public static boolean isSorted(Comparable[] a)
	{ // Test whether the array entries are in order.
		for (int i = 1; i < a.length; i++)
			if (less(a[i], a[i-1])) return false;
		return true;
	}

	public static void main(String[] args)
	{ // Read strings from standard input, sort them, and print.
		String[] a = In.readStrings();
		sort(a);
	 	assert isSorted(a);
		show(a);
	}
}

{% endhighlight %}

Sorting cost model.
When studying sorting algorithms, we count compares and exchanges. 
For algorithms that do not use exchanges, we count array accesses.

two elementary sorting methods: selection sort and insertion sort
a variation of one of them: shellsort

##### Selection Sort

Steps:
 
* First, find the smallest item in the array, and exchange it with the first entry.
* Then, find the next smallest item and exchange it with the second entry.
* Continue in this way until the entire array is sorted.

Two signature properties:
* Running time is insensitive to input
* Data movement is minimal

**Proposition A.**
If the length of an array is N, we uses $N^2/2$ compares and $N$ exchanges to.

{% highlight java %}

public class Selection
{
	public static void sort(Comparable[] a)
 	{ // Sort a[] into increasing order.
 		int N = a.length; // array length
 		for (int i = 0; i < N; i++)
 		{ // Exchange a[i] with smallest entry in a[i+1...N).
 			int min = i; // index of minimal entr.
 			for (int j = i+1; j < N; j++)
 				if (less(a[j], a[min])) min = j;
 			exch(a, i, min);
 		}
 	}
 	// See page 245 for less(), exch(), isSorted(), and main().
}

{% endhighlight %}

##### Insertion Sort

Unlike that of selection sort, the running time of insertion sort depends on the initial
order of the items in the input. Before inserting the current item into the vacated 
position, we need to make space for the current item by moving larger items one position
to the right. 

{% highlight java %}

public class Insertion
{
	public static void sort(Comparable[] a)
	{ // Sort a[] into increasing order.
		int N = a.length;
		for (int i = 1; i < N; i++)
		{ // Insert a[i] among a[i-1], a[i-2], a[i-3]... ..
			for (int j = i; j > 0 && less(a[j], a[j-1]); j--)
				exch(a, j, j-1);
		}
	}
	// See page 245 for less(), exch(), isSorted(), and main().
}

{% endhighlight %}

**Proposition B.**
On the average, insertion sort uses $N^2/4$ compares and $N^2/4$ exchanges to sort a randomly
ordered array of length N with distinct keys. The worst case is $N^2/2$ compares and $N^2/2$ 
exchanges and the best case is $N-1$ compares and $0$ exchanges.

Typical examples of partially sorted arrays are the following:
* An array where each entry is not far from its final position
* A small array appended to a large sorted array
* An array with only a few entries that are not in place
Insertion sort is an efficient method for such arrays; selection sort is not. 

**Proposition C.**
The number of exchanges used by insertion sort is equal to the number of inversions in the array,
and the number of compares is at least equal to the number of inversions and at most equal to 
the number of inversions plus the array size minus 1.

It is not difficult to speed up insertion sort substantially, by shortening its inner loop to
move the larger entries to the right one position rather than doing full exchanges (thus
cutting the number of array accesses in half). (Need to Practice)

##### Visualizing sorting algorithms

![sample post]({{site.baseurl}}/images/algorithms4/visualtraces.jpg)

##### Comparing to sorting algorithms

**Property D.** 
The running times of insertion sort and selection sort are quadratic
and within a small constant factor of one another for randomly ordered arrays of
distinct values.

{% highlight java %}

public class SortCompare
{
	public static double time(String alg, Double[] a)
	{ /\*See text\*/ }
	
	public static double timeRandomInput(String alg, int N, int T)
 	{ // Use alg to sort T random arrays of length N.
 		double total = 0.0;
 		Double[] a = new Double[N];
 		for (int t = 0; t < T; t++)
 		{ // Perform one experiment (generate and sort an array).
 			for (int i = 0; i < N; i++)
 				a[i] = StdRandom.uniform();
 			total += time(alg, a);
 		}
 		return total;
 	}

 	public static void main(String[] args)
 	{
 		String alg1 = args[0];
 		String alg2 = args[1];
 		int N = Integer.parseInt(args[2]);
 		int T = Integer.parseInt(args[3]);
 		double t1 = timeRandomInput(alg1, N, T); // total for alg1
 		double t2 = timeRandomInput(alg2, N, T); // total for alg2
 		StdOut.printf("For %d random Doubles\n %s is", N, alg1);
 		StdOut.printf(" %.1f times faster than %s\n", t2/t1, alg2);
 	}
}

{% endhighlight  %}

##### Shell Sort

Shellsort is a simple extension of insertion sort that in order to gains the speed by 
allowing exchanges of array entries that are far apart, to produce partially sorted arrays 
that can be efficiently sorted, eventually by insertion sort.

The idea is to rearrange the array to give it the property that taking every \*hth entry
(starting anywhere) yields a sorted subsequence. To implement shellsort: for each h, to use 
insertion sort independently on each of the h subsequences.This methord reduces the shellsort 
implementation to an insertion-sort-like pass through the array for each increment.  

{% highlight java %}

public class Shell
{
	public static void sort(Comparable[] a)
 	{ // Sort a[] into increasing order.
 		int N = a.length;
 		int h = 1;
 		while (h < N/3) h = 3\*h + 1; // 1, 4, 13, 40, 121, 364, 1093, ...
 		while (h >= 1)
 		{ // h-sort the array.
 			for (int i = h; i < N; i++)
 			{ // Insert a[i] among a[i-h], a[i-2\*h], a[i-3\*h]... .
 				for (int j = i; j >= h && less(a[j], a[j-h]); j -= h)
 					exch(a, j, j-h);
 			}
 			h = h/3;
 		}
 	}
 	// See page 245 for less(), exch(), isSorted(), and main().
}

{% endhighlight %}

**Property E.**
The number of compares used by shellsort with the increments $1, 4,13, 40, 121, 364, . . .$ is 
bounded by a small multiple of N times the number of increments used.

2.2 Mergesort
-------------

The simple idea is: to sort an array, divide it into small groups, sort the small groups 
(recursively) first, and then merge the results.

Pros: to sort an array of N items in time proportional to N log N, no matter what the input
Cons: it uses extra space proportional to N.

![sample post]({{site.baseurl}}/images/algorithms4/mergesort-overview.png)

##### Abstract in-place merge

The core idea is to make sure that the number in the pointer left side is the lower value elements in the two
subsequences, respectively. We exchange the part of the array to reorder it. Easier to understand as follow:

Here Figure a shows the two subsequences, and we start in-place merge from Figure b. 
After Figure b, we can see the value in the left side of pointer `i` is the lower value elements in the two subsequences.
Figure c, since $20,25 < 30$, pointer `j` keep right movement until find the value is greater than $30$. 

![sample post]({{site.baseurl}}/images/algorithms4/a.jpg)

After Figure c, we learned that all the value in $\[index, j)$ are less then the part of $\[i,index)$, in order to
make sure the left side of pointer `i` is the lower value elements, we change this two part, the step length equals 
the numbers of the $\[index,j)$, here is $2$. Then we get Figure e.
![sample post]({{site.baseurl}}/images/algorithms4/e.png)

Figure e, pointer `i` moves 2(we just mentioned above) to the right, that is $i += (j - index)$
Figure f, repeat the process in Figure b.

![sample post]({{site.baseurl}}/images/algorithms4/f.png)
 
{% highlight java  %}

// Abstract In-place Merge

public static void merge(Comparable[] a, int lo, int mid, int hi)
{ // Merge a[lo..mid] with a[mid+1..hi].

	int i = lo, j = mid+1;

	for (int k = lo; k <= hi; k++)           // Copy a[lo..hi] to aux[lo..hi].
		aux[k] = a[k];
	for (int k = lo; k <= hi; k++)           // Merge back to a[lo..hi].
		if (i > mid) a[k] = aux[j++];
		else if (j > hi ) a[k] = aux[i++];
		else if (less(aux[j], aux[i])) a[k] = aux[j++];
		else a[k] = aux[i++];
}

{% endhighlight %}

For the second for loop, there are four conditions: 
* if the left half exhausted, we take from the right
* if the right half exhausted, we take from the left 
* if the current key on right less than current key on left, we take from the right 
* if the current key on right greater than or equal to current key on left, we take from the left.

##### Top-down mergesort

This is a recursive mergesort implementation based on the abstract in-place merge. It is one of the 
best-known examples of the divide-and-conquer utility fot the algorithm design.

{% highlight java %}

public class Merge
{
	private static Comparable[] aux;        // auxiliary array for merges
 	public static void sort(Comparable[] a)
 	{
 		aux = new Comparable[a.length];     // Allocate space just once.
 		sort(a, 0, a.length - 1);
 	}
 	private static void sort(Comparable[] a, int lo, int hi)
 	{ // Sort a[lo..hi].
 		if (hi <= lo) return;
 		int mid = lo + (hi - lo)/2;
 		sort(a, lo, mid);                   // Sort left half.
 		sort(a, mid+1, hi);                 // Sort right half.
 		merge(a, lo, mid, hi);              // Merge results (code on Abstract In-place Merge).
 	}
}

{% endhighlight %}

To sort a subarray a[lo..hi], first, we divide it into two parts: a[lo..mid] and a[mid+1..hi], and 
then to sort them independently (via recursive calls), finally, to merge the resulting ordered 
subarrays and produce the result.

**Proposition F**
Top-down mergesort uses between $1/2 N lg N$  and $N lg N$ compares to sort any array of length $N$.

**Proposition G** 
Top-down mergesort uses at most $6N lg N$ array accesses to sort an array of length $N$. 
Since $2N$ for the copy, $2N$ for the move back, and at most $2N$ for compares.

Also the mergesort has its cons. It requires extra space proportional to $N$ for the auxiliary 
array for merging. So we can carefully to consider that: if space is at a premium, can we have another
method? Or can we cut the running time of mergesort? Is there anything else we can improve? Such as: 
* Use insertion sort for small subarrays. 
* Test whether the array is already in order. 
* Eliminate the copy to the auxiliary array. 

##### Bottom-up mergesort

{% highlight java %}

public class MergeBU
{
	private static Comparable[] aux; // auxiliary array for merges
 	// See merge() code on Abstract In-place Merge.
 	public static void sort(Comparable[] a)
 	{ // Do lg N passes of pairwise merges.
 		int N = a.length;
 		aux = new Comparable[N];
 		for (int sz = 1; sz < N; sz = sz+sz) // sz: subarray size
 			for (int lo = 0; lo < N-sz; lo += sz+sz) // lo: subarray index
 				merge(a, lo, lo+sz-1, Math.min(lo+sz+sz-1, N-1));
 	}
}

{% endhighlight %}

**Proposition H**
Bottom-up mergesort uses between $1/2 N lg N$ and $N lg N$ compares and at most $6N lg N$ 
array accesses to sort an array of length $N$.

A version of bottom-up mergesort is the method of choice for sorting data organized in a 
linked list. Consider the list to be sorted sublists of size 1, then pass through to make
sorted subarrays of size 2 linked together, then size 4, and so forth. This method rearranges
the links to sort the list in place (without creating any new list nodes).

When the array length is a power of 2, top-down and bottom-up mergesort perform precisely 
the same compares and array accesses, just in a different order. 

##### The complexing of sorting

**Proposition I** 

No compare-based sorting algorithm can guarantee to sort $N$ items with fewer than $lg(N !) ~ N lg N$ compares.

**Proposition J**
Mergesort is an asymptotically optimal compare-based sorting algorithm.

Why we considering the upper bounds and lower bounds in algorithms?
* First, good upper bounds allow software engineers to provide performance guarantees; there 
  are many documented instances where poor performance has been traced to someone using a 
  quadratic sort instead of a linearithmic one.
* Second, good lower bounds spare us the effort of searching for performance improvements 
  that are not attainable. 

2.3 Quicksort
-------------

Quicksor is probably used more widely than any others. The reason is simple, because it works well 
for a variety of different kinds of input data, and is substantially faster than any other sorting 
method in typical applications. The advantages are that it is in-place (uses only a small auxiliary
 stack) and that it requires time proportional to $N log N$ on the average to sort an array of 
length $N$.

##### The Idea of Quicksort

Quicksort is complementary to mergesort: for mergesort, we break the array into small subarrays 
to be sorted and then combine the ordered subarrays to make the whole ordered array; for quicksort,
we rearrange the array during the procession that we divide the whole ararry into small subarrays.
Once the small subarrays are sorted, the whole array is ordered. 

Rearranges the array to make the following three conditions hold:
The entry a[j] is in its final place in the array, for some j.
No entry in a[lo] through a[j-1] is greater than a[j].
No entry in a[j+1] through a[hi] is less than a[j].

![sample post]({{site.baseurl}}/images/algorithms4/partitioning-overview.png)

##### The partitioning method Steps

1. Choose a mid-item to be the partitioning item, whicn will go into its final position. Usually we
   choose the left-hand item or right-hand item.
2. Get the index of each side of the mid-tiem except the mid-item. 
3. Iterating from the two sides to the middle, during every iteration we need to compare and sort 
   them, respectively.
4. Repeat step 3 until the two index of the two side meet each other. Here, the left side is the 
   items that are less than the mid-item, and the right side is the items are greater than mid-item.
5. Repeat the whole steps until the subarrays can not divide at all.

{% highlight java %}

private static int partition(Comparable[] a, int lo, int hi)
{ // Partition into a[lo..i-1], a[i], a[i+1..hi].
	int i = lo, j = hi+1; // left and right scan indices
 	Comparable v = a[lo]; // partitioning item
 	while (true)
 	{ // Scan right, scan left, check for scan complete, and exchange.
		while (less(a[++i], v)) if (i == hi) break;
 		while (less(v, a[--j])) if (j == lo) break;
 		if (i >= j) break;
 		exch(a, i, j);
 	}
 	exch(a, lo, j); // Put v = a[j] into position
 	return j; // with a[lo..j-1] <= a[j] <= a[j+1..hi].
}

{% endhighlight %}

##### Implementation details

* Partitioning inplace. 
  If we use an extra array, partitioning is easy to implement, but not so much easier that
  it is worth the extra cost of copying the partitioned version back into the original.

* Staying in bounds. 
  If the smallest item or the largest item in the array is the partitioning item, we have to
  take care that the pointers do not run off the left or right ends of the array, respectively.

* Preserving randomness. 
  The random shuffle puts the array in random order.

* Terminating the loop. 
  Properly testing whether the pointers have crossed is a bit trickier than it might seem at 
  first glance. A common error is to fail to take into account that the array might contain 
  other keys with the same value as the partitioning item.

* Handling items with keys equal to the partitioning item's key. 
  It is best to stop the left scan for items with keys greater than or equal to the partitioning 
  item's key and the right scan for items less than or equal to the partitioning item's key. Even
  though this policy might seem to create unnecessary exchanges involving items with keys equal to
  the partitioning item's key, it is crucial to avoiding quadratic running time in certain typical
  applications.

* Terminating the recursion. 
  A common mistake is we can not ensuring the partitioning item is always put into position, then 
  falling into an infinite recursive loop when the partitioning item happens to be the largest or 
  smallest item in the array.

##### Algorithmic improvements

Quicksort was invented in 1960 by C. A. R. Hoare. Almost from this moment, people began trying to
find ways to improve this algorithm. Not all of these ideas are fully successful, because this 
algorithm is so well-balanced that the effects of improvements can be more than offset by
unexpected side effects, but a few of them, which we now consider, are quite effective.

* The first improvement is based on Quicksort is slower then insertion sort for tiny subarrays, so
  once dealing with tiny subarrarys we could considering insertion sort.

* In the real cases, most arrays have large number of duplicate keys, we could consider to divide
  the arrays into three subarrays, one part keys are less than the middle items value, second
  part keys are equals the middle items value, and the rest are the third part, its keys are greater
  than the middle items value. This method we called 3-way partitioning:
   
{% highlight java %}

public class Quick3way
{
	private static void sort(Comparable[] a, int lo, int hi)
 	{ // See Quicksort code for public sort() that calls this method.
 		if (hi <= lo) return;
 		int lt = lo, i = lo+1, gt = hi;
 		Comparable v = a[lo];
 		while (i <= gt)
 		{
 			int cmp = a[i].compareTo(v);
 			if (cmp < 0) exch(a, lt++, i++);
 			else if (cmp > 0) exch(a, i, gt--);
 			else i++;
 		} // Now a[lo..lt-1] < v = a[lt..gt] < a[gt+1..hi].
 		sort(a, lo, lt - 1);
 		sort(a, gt + 1, hi);
 	}
}

{% endhighlight %}

As with standard quicksort, the running time tends to the average as the array size grows, and large 
deviations from the average are extremely unlikely, so we use 3-way quicksort since it reduces the 
time of the sort from linearithmic to linear for arrays with large numbers of duplicate keys. And this
is independent of the element sorting. So 3-way quicksort turns out to be best in that situation. And the
improved Quicksort is widely as well.

To sorting the tens of millions of arrays, let's see the runnint time: 
`Quick` > `Merge` > `Shell` > `Insertion` > `Selection`

2.4 Priority Queues
-------------------

For example, our cellphones are likely to process an incoming call with higher priority than a game 
application. To support this function need to ordering the appropriate data type through these two 
operations: remove the maximum and insert. Such a data type is called a priority queue. 

Using priority queues is similar to using queues (remove the oldest) and stacks (remove the newest),
but every element in the queue has its own priority, once we dealing with them, we operate the highest 
priority element at first, if there are two equally higher priority elements, implementing them by their 
input order, respectively. We could implemente the priority queues by linked-list, arrays, heap and 
other data structures. 

##### Array representation

**Costs of finding the largest M in a stream of N items**

|client                                      | order of growth            |
|                                            | time       |     space     |
|:-------------------------------------------|:----------:|:-------------:|
|sort client                                 | N log N    | N             |
|PQ client using elementary implementation   | NM         | M             |
|PQ client using heap-based implementation   | N log M    | M             |

* Array representation (unordered). Perhaps the simplest priority queue implementation is based on our 
code for pushdown stacks. The code for insert in the priority queue is the same as for push in the stack.
To implement remove the maximum, we can add code like the inner loop of selection sort to exchange the 
maximum item with the item at the end and then delete that one, as we did with pop() for stacks.

* Array representation (ordered). Another approach is to add code for insert to move larger entries one 
position to the right, thus keeping the entries in the array in order (as in insertion sort). Thus the 
largest item is always at the end, and the code for remove the maximum in the priority queue is the same 
as for pop in the stack. 

* Linked-list representations (unordered and reverse-ordered). Similarly, we can start with our linked-list
code for pushdown stacks, either modifying the code for pop() to find and return the maximum or the code for
push() to keep items in reverse order and the code for pop() to unlink and return the first (maximum) item 
on the list.

**Proposition O**
The largest key in a heap-ordered binary tree is found at the root.

##### Binary representation

**Heap:**
The binary heap is a data structure that can efficiently support the basic priority-queue operations. In a 
binary heap, the items are stored in an array such that each key is guaranteed to be larger than (or equal to)
the keys at two other specific positions. In turn, each of those keys must be larger than two more keys, and 
so forth. A binary tree is heap-ordered if the key in each node is larger than (or equal to) the keys in that 
nodes two children (if any).

In a heap, the parent of the node in position $k$ is in position $k/2$; and, conversely, the two children of 
the node in position k are in positions $2k$ and $2k + 1$. We can travel up and down by doing simple arithmetic
on array indices: to move up the tree from $a[k]$ we set $k$ to $k/2$; to move down the tree we set $k$ to 
$2\*k$ or $2\*k+1$.

**Proposition P**
The height of a complete binary tree of size $N$ is $lg N$.

* Bottom-up reheapify (swim)
  If the heap order is violated because a node's key becomes larger than that node's parents key, then we can make
  progress toward fixing the violation by exchanging the node with its parent. After the exchange, the node is 
  larger than both its children (one is the old parent, and the other is smaller than the old parent because it was
  a child of that node) but the node may still be larger than its parent. We can fix that violation in the same way,
  and so forth, moving up the heap until we reach a node with a larger key, or the root.

{% highlight java %}

private void swim(int k) {
   while (k > 1 && less(k/2, k)) {
      exch(k, k/2);
      k = k/2;
   }
}

{% endhighlight %}

* Top-down heapify (sink)
  If the heap order is violated because a node's key becomes smaller than one or both of that node's children's keys,
  then we can make progress toward fixing the violation by exchanging the node with the larger of its two children.
  This switch may cause a violation at the child; we fix that violation in the same way, and so forth, moving down the
  heap until we reach a node with both children smaller, or the bottom.

{% highlight java %}

private void sink(int k) {
   while (2\*k <= N) {
      int j = 2*k;
      if (j < N && less(j, j+1)) j++;
      if (!less(k, j)) break;
      exch(k, j);
      k = j;
   }
}

{% endhighlight %}

**Heap-based priority queue**

* insert.
  We add the new item at the end of the array, increment the size of the heap, and then swim up through the heap
  with that item to restore the heap condition.
* Remove the maximum.
  We take the largest item off the top, put the item from the end of the heap at the top, decrement the size of 
  the heap, and then sink down through the heap with that item to restore the heap condition.

Heap priority queue
{% highlight java %}
public class MaxPQ<Key extends Comparable<Key>>
{
	private Key[] pq;          // heap-ordered complete binary tree
 	private int N = 0;         // in `pq[1..N]` with `pq[0]` unused
 	public MaxPQ(int maxN)
 	{ pq = (Key[]) new Comparable[maxN+1]; }
 	public boolean isEmpty()
 	{ return N == 0; }
 	public int size()
 	{ return N; }
 	public void insert(Key v)
 	{
 	pq[++N] = v;
 	swim(N);
 	}
 	public Key delMax()
 	{
 		Key max = pq[1];       // Retrieve max key from top.
 		exch(1, N--);          // Exchange with last item.
 		pq[N+1] = null;        // Avoid loitering.
 		sink(1);               // Restore heap property.
 		return max;
 	}
 	// See pages 145-147 for implementations of these helper methods.
 	private boolean less(int i, int j)
 	private void exch(int i, int j)
 	private void swim(int k)
 	private void sink(int k)
}

{% endhighlight %}

**Proposition Q**
In an $N$-key priority queue, the heap algorithms require no more than $1 + lg N$ compares for insert
and no more than $2 lg N$ compares for remove the maximum.

**Practical considerations**
We conclude our study of the heap priority queue API with a few practical considerations.
* Multiway heaps. 
  It is not difficult to modify our code to build heaps based on an array representation of complete heap-ordered
  ternary or d-ary trees. There is a tradeoff between the lower cost from the reduced tree height and the higher 
  cost of finding the largest of the three or d children at each node.
* Array resizing.
  We can add a no-argument constructor, code for array doubling in `insert()`, and code for array halving in 
  `delMax()`. The logarithmic time bounds are amortized when the size of the priority queue is arbitrary and
   the arrays are resized.
* Immutability of keys. 
  The priority queue contains objects that are created by clients but assumes that the client code does not change
  the keys (which might invalidate the heap invariants).
* Index priority queue.
  In many applications, it makes sense to allow clients to refer to items that are already on the priority queue. 
  One easy way to do so is to associate a unique integer index with each item.

**Heapsort**

* Heap construction. 
  We can accomplish this task in time proportional to $N lg N$, by proceeding from left to right through the array,
  using `swim()` to ensure that the entries to the left of the scanning pointer make up a heap-ordered complete tree,
  like successive priority queue insertions. A clever method that is much more efficient is to proceed from right to
  left, using `sink()` to make subheaps as we go. Every position in the array is the root of a small subheap; `sink()`
  works or such subheaps, as well. If the two children of a node are heaps, then calling `sink()` on that node makes
  the subtree rooted there a heap.

* Sortdown. 
  Most of the work during heapsort is done during the second phase, where we remove the largest remaining items from 
  the heap and put it into the array position vacated as the heap shrinks.

{% highlight java %}
public static void sort(Comparable[] a)
{
	int N = a.length;
	for (int k = N/2; k >= 1; k--)
		sink(a, k, N);
	while (N > 1)
	{
		exch(a, 1, N--);
		sink(a, 1, N);
	}
}

{% endhighlight %}

2.5 Applications
----------------


