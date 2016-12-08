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
two elementary sorting methods: selection sort and insertion sort
a variation of one of them: shellsort

{% highlight java %}

private static boolean less(Comparable v, Comparable w) {
   return (v.compareTo(w) < 0);
}

private static void exch(Comparable[] a, int i, int j) {
   Comparable swap = a[i];
   a[i] = a[j];
   a[j] = swap;
} 

{% endhighlight %}

##### Selection Sort

* First, find the smallest item in the array, and exchange it with the first entry.
* Then, find the next smallest item and exchange it with the second entry.
* Continue in this way until the entire array is sorted.

##### Insertion Sort

##### Visualizing sorting algorithms

##### Shell Sort

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


