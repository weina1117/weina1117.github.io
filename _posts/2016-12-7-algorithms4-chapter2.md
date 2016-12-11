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

##### Abstract in-place merge

##### Top-down mergesort

##### Bottom-up mergesort

##### The complexing of sorting

2.3 Quicksort
-------------



2.4 Priority Queues
-------------------



2.5 Applications
----------------


