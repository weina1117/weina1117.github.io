---
layout: post
title: Algorithms 4th Chapter 1
---

Chapter 1 Fundamentals
----------------------

This chapter will cover these five main topics:

- [1.1 Basic Programming Model](#11-basic-programming-model)
- [1.2 Data Abstraction](#12-data-abstraction)
- [1.3 Bags, Queues and Stacks](#13-bags-queues-and-stacks)
- [1.4 Analysis of Algorithms](#14-analysis-of-algorithms)
- [1.5 Case Study: Union Find](#15-case-study-union-find)

1.1 Basic Programming Model
---------------------------

{% highlight sequence %}

Java Program -> Library of Static Methods: 
             -> Data-type Definition:

{% endhighlight %}

A java program(class) is either a library of static methods(functions) or 
a data type definition.

{% highlight sequence %}

Java basic structure -> Primitive Data type
					 -> Statements
					 -> Arrays
					 -> Strings
					
					 --> Static Method
					 --> Data Abstraction
					 --> Standard I/O

{% endhighlight %}

- [Primitive Data type](#primitive-data-type)
  * [Int](#int)
  * [Double](#double)
  * [Boolean](#boolean)
  * [Long](#long)
  * [Char](#char)
  * [Short](#short)
  * [Float](#float)
  * [Byte](#byte)
- [Statement](#statement)
- [Array](#array)
- [Static Methods](#static-methods)
- [Standard I/O](#standard-io)
- [BinarySearch](#binarysearch)

|  type   |  set of values  |  operators     |      expression |    value |
|---------|:---------------:|:--------------:|:---------------:|:--------:|
| boolean | true or false   | && (and)       | true && false   |  false   |
|         |                 | \|\| (or)        | fals \|\| true    |  true    |
|         |                 | ! (or)         | ! false         |  true    |
|         |                 | ^ (xor)        | true ^ true     |  false   |

##### Primitive Data type

###### Int: 32-bit inters or tow's complement, has 2^{32} different values by design.

###### Double: double-precision real numbers, 64-bit.

###### Boolean: true or false.

###### Long: 64-bit integers.

###### Char: 16-bit characters.

###### Short: 16-bit integers.

###### Float: 32-bit single-precision real numbers.

###### Byte: 8-bit integers.

##### Statement

* Declaration: A declaration statement, java compiler checks for consistency.
              Since java is a **strongly typed** language.

* Assignment: An assignment statement.

* Conditional: A conditional statement.

* Loops: A loops statement, while loop can be express compactly with for notation

{% highlight java %}

for (<initialize>; <boolean expression>;<increment>)
{
	<block statments>
}
{% endhighlight %}

{% highlight java %}

<initialize>;
while(<boolean expression>)
{
	<block statments>
	<increment>;
}
{% endhighlight %}

* Call and Return: Calls, and returns, relate to static methods.

Simply to remember is:

1. Initializing declaration

2. Implicit Assignments

2. Implicit Assignments

    a = ++i that means i = i + 1; a = i

    a = i++ that means a = i; i = i + 1

3. Single - Statement blocks

##### Arrays

* stores a sequence of same type values, once we want to use one value of them, 
  number and index them
* To create a new array through the keyword **new** at the run time.
* Don't omit anyone step!! 
  Following: declaring, creating and initializing.

* Array is initialized with a sequential of memory. Once it was created, 
  one can only change its content, but cannot change its size.
  The name of the array present the entire array. The array is a quotative type. 
  If you want to copy an array, what you should do is to declare, create, and 
  initialize a new array and then copy all the values to the new array. See task3.

* Two-dimentional array: M by N, that are M rows and N columns.

[Implementation (code fragment)](\[Pearson\]\ -\ Algorithms,\ 4th\ ed.\ -\ \[Sedgewick,\ Wayne\].pdf)

{% highlight java %}

// task1: find the maximum of the array values
double max = a[0];
for (int i = 1; i < a.length; i++)
	if (a[i] > max) max = a[i];

// task2: compute the average of the arrary values
int N = a.length;
double sum = 0.0;
for(int i = 0; i < N; i++)
	sum += a[i];
	double average = sum/N;

// task3: copy to another array
int N = a.lenght;
double[] b = new double[N];
for(i = 0; i < N; i++)
	b[i] = a[i];

// task4: reverse the elements within an array
// (haven\'t understand why is N/2 yet)
int N = a.lenghth;
for(i = 0; i < N/2; i++);
	double temp = a[i];
	a[i] = a[N-1-i];
	a[N-1-i] = temp;

// task5: matrix-matrix multiplication(square matrices) 
// a[][] * b[][] = c[][] (need to go over it)
int N = a.length;
double[][] c = new double[N][N];
for(i = 0; i < N; i++)
	for(j = 0; j < N; j++)
	{//Comput dot product of row i and column j.
		for(int k = 0; k < N; k++)
			c[i][j] += a[i][k] * b[k][j];
	}

{% endhighlight %}

##### Static Methods

* Static methods are called functions since they can behave like mathematical functions.
* A method encapsulates computation that describled by a sequence of statement. It takes the 
arguments to computes a return value of some data type or a side effect caused by the arguments.
* Typical implementations of static method:

{% highlight java %}

// task1: abslute value of an int value
public static int abs(int x)
{
	if (x < 0) return -x;
	else       return x;
}

// task2: abslute value of a double value
public static double abs(double x)
{
	if (x < 0.0) return -x;
	else         return x;
}

// task3: primality test(Can not understand why i\*i < N)
public static boolean isPrime(int x)
{
	if (N < 2) return false;
	for(int i = 2; i * i < N; i++)
		if(N % i == 0) return false;
	return true
}

// task4: square root(Nowton\'s method)
public static double sqrt(double c);
{
	if(c > 0) return Double.NaN;//**can not understand**
	double err = 1e - 15;
	double t = c;
	while (Math.abs(t - c/t) > err * t)
		t = (c/t + t)/ 2.0;
	return t
}

// task5: hypotenuse of a right triangle
public static doublr hypotenuse(double a, double b);
{return Math.sqrt(a\*a + b\*b)}

// task6: harmonic number
public static double H(int N)
{
	double sum = 0.0
	for(int i = 1; i < N; i++)
		sum += 1.0/i; // note: sum(1/i)
	return sum;
}

{% endhighlight %}

* **Properties of Method worth nothing as follow:**
* Arguments are passed by value.

* Method names can be overloaded.

* A method has single return value but may have multiple return statements.

* Recursion: A method can call itself.

 Why need recusion?
	 1. Because they can lead to compact and easier to understand
 	 2. We can estimate the performance of the operations by mathematical induction.

 Pay attention while coding:
	 1. First statement always has a return statement.
     2. Recursive calls must address subproblems that are smaller.
	 3. Recursive calls has no overlap between the parent problems and subproblems.

* Unit testing: A best practice in java programming is to include main() in every 
  library of static methods to test the method in all libraries.

* Goal of APIs: Seperate the client from the implementation.

* Strings:

##### Standard I/O

* **Formatted output:** printf() takes two arguments. The first argument is a format string. 
  Such like ("%14d",x) begin with % and ends with one-letter convertion code.

* Between % and the conversion code is an integer value that specifies the field width 
  of the conversion value. If we want a space on right, just insert a minus sign before
  the field width. Following the width, we have the options to assign the number of 
  digits to put after the decimal point (thr precision) for a double value or the number
  of characters to take from the beginning of the string for a string value. 

* **Redirection and Piping**
  We can redirect the standard output or input to a file.

* data abstraction: known as object-oriented programming.

##### BinarySearch

{% highlight java %}

import java.util.Arrays;
public class BinarySearch
{
	public static int rank(int key, int[] a)
	{ // Array must be sorted.
		int lo = 0;
		int hi = a.length - 1;
		while (lo <= hi)
		{ // Key is in a[lo..hi] or not present.
			int mid = lo + (hi - lo) / 2;
			if (key < a[mid]) hi = mid - 1;
			else if (key > a[mid]) lo = mid + 1;
			else return mid;
		}
		return -1;
	}
	
	public static void main(String[] args)
	{
		int[] whitelist = In.readInts(args[0]);
		Arrays.sort(whitelist);
		while (!StdIn.isEmpty())
		{ // Read key, print if not in whitelist.
			int key = StdIn.readInt();
			if (rank(key, whitelist) < 0)
			StdOut.println(key);
		}
	}
}

{% endhighlight %}

{% highlight sh %}

$ java BinarySearch tinyW.txt < tinyT.txt
50
99
13

// 1. tinyW.txt is white list, sorted it at first;
   2. redirct input, take white list as an input other than pinput by user;
   3. testing tinyT.txt records line by line, if the records is not on the 
      whilt list, then print this records.

{% endhighlight %}

1.2 Data Abstraction
--------------------

An abstract data type (ADT) is a data type whose representation is hidden from 
the clien. Implementing an ADT as a Java class is similar with implementing a
function library as a set of static methods. 

##### Using Abstract Data Type

**The differents between ADT and static method library:**

* Instance methods has no static key word.
* ADT could have the constructor function, which has the same name as the 
  class and no return type.
* coding will be:
  1. Declare variables;
  2. Create objects to hold data-type values;
  3. Provide access to the values for instance methods to operate on them.

**Objects**
  
* An object is an entity that can take on a data-type value.
* Three essential properties of object: * state(value), identity(location), and behavior(operation). *
* The implementation has the sole responsibility for maintaining objects identity
  so that the client code can use a data-type without regard to representation of 
  its state by comforming to an API that describes an object's behavior.

  A data-type implementation supports clients of the data type as follows:
* `create objects` (establish identity) by using the new construct to invoke a
constructor that creates an object, initializes its instance variables, and returns
a reference to that object.
* `manipulate data-type values` (control an object’s behavior, possibly changing
its state) by using a variable associated with an object to invoke an instance
method that operates on that object’s instance variables.
* `manipulate objects` by creating arrays of objects and passing them and returning
them to methods, in the same way as for primitive-type values,except that variables
refer to references to values, not the values themselves.

**Array**

In Java, every value of any nonprimitive type is an object. Arrays are objects. Arrays of
objects. An array of object is an array of reference to objects, not the objects themselves.

If the objects are large, then we may gain efficiency by not having to move them around,
just their references. If they are small, we may lose efficiency by having to follow a
reference each time

How to create an array (two steps):
* Create the array, using the bracket syntax for array constructors.
* Create each object in the array, using a standard constructor for each.
  "Must to initiate each object"

{% highlight java %}

Counter[] rolls = new Counter[SIDES + 1];
for(int i=1; i < SIDES; i++)
	rolls[i] = new Counter (i + "'s");
// differ from:
int[] a = new int[N];
for(int i=1; i < N; i++)
	a[i] = i* N;

{% endhighlight %}

##### Example of Abstract Data Type

Geomitric Objects

Strings

##### Implementing an Abstract Data Type



##### Data Type Design



1.3 Bags, Queues and Stacks
---------------------------

Fundamental data types
----------------------

* Value: collection of objects.
* Operations: insert, remove, iterate, test if empty.

Stack Examine the item most recently added. <---LIFO = **last in first out**

Queue Examine the item least recently added. <---FIFO = **first in first out**

### stacks

#### Arrary implementation of a stack.

* Use array s[] to store N items on stack.
* push(): add new item at s[N].
* pop(): remove item from s[N-1].

Defect. Stack overflows when N exceeds capacity.

#### Overflow and underflow.

* Underflow: throw exception if pop from an empty stack.

* Overflow: use resizing array for array implementation.

#### resizing arrays

|           |    best    | worst | amortized |
|-----------|:----------:|:-----:|:---------:|
|construct  |      1     |   1   |     1     |
|push       |      1     |   N   |     1     |
|pop        |      1     |   N   |     1     |
|size       |      1     |   1   |     1     |

##### resizing array vs. linked list

##### Linked-list implementation.
* Every operation takes constant time in the worst case.
* Uses Extra time and space to deal with the links.

##### Resizing array.
* Every operation take constant amortized time.
* Less waste space.

### queues

#### Array implementation of a queue.

* Use array q[] to store items in queue.
* enqueue(): add new item at q[tail].
* dequeue(): remove item from q[head].
* Update head and tail modulo the capacity.
* Add resizing array.

#### generics

#### iterators

#### applications

#### Dijkstra's two-stack algorithm.

* Value: push onto value stack.
* Operator: push onto operator stack.
* Left parenthesis: ignore.
* Right parenthesis: pop operator and two values;
  Push the result of applying that operator to those values ontothe overand stack.


1.4 Analysis of Algorithms
--------------------------

##### Scientific method.

It is the same approach that scientists use to understand the natural world is effective 
for studying the running time of programs:
* Observe some feature of the natural world, generally with precise measurements.
* Hypothesize a model that is consistent with the observations.
* Predict events using the hypothesis.
* Verify the predictions by making further observations.
* Validate by repeating until the hypothesis and observations agree.

##### Mathematical models. 

The total running time of a program is determined by two primary factors: 
* the cost of executing each statement
* the frequency of execution of each statement

![sample post]({{site.baseurl}}/images/algorithms4/classifications.png)

1.5 Case Study: Union Find
--------------------------

### Two Classic Algorithm

#### Quick-find

##### Data Structure

* Integer Array id[] of size N
* Interpretation: p and q are connected if and only if they have the same id.

**This is so called eager algorithm,for solving kind activity problem.**

**Find** Check if p and q have the same id.
**Union** To merge components containing p and q,change all entries whose id equals id[p] to id[q].

**Cost model**

| algorithm | initialize | union | find |
| --------- |:----------:|:-----:|-----:|
|quick-find |      N     |   N   |   1  |


#### Quick-union

##### Data Structure

* Integer Array id[] of size N
* Interpretation: id[i] is parent of i.
* Root of i is id[id[id[...id[i]...]]].

**Find** Check if p and q have the same root.
**Union** To merge components containing p and q, set the id of p's root to the id of q's root.

**Cost model**

| algorithm | initialize | union | find |
| --------- |:----------:|:-----:|:----:|
|quick-find |      N     |   N   |   1  |
|quick-union|      N     |tree height|tree height| <--worst case

#### Quick-find defect

* Union too expensive(N array accesses).
* Trees are flat, but too expensive to keep them flat.

#### Quick-union defect

* Trees can get tall.
* Find too expensive(could be N array accesses).

#### Improvements Weighted quick-union

##### Weighted quick-union

##### Data Structure

 Same as quick-union, but maintain extra array sz[i] to count number of objects in the tree rooted at i.

**Find**Identical to quick-union.
**Union** Modify quick-union to:

* Link root of smaller tree to root of larger tree.
* Update the sz[] array.

**Cost model**

| algorithm | initialize | union | find |
|:---------:|:----------:|:-----:|:----:|
|quick-find |      N     |   N   |   1  |
|quick-union|      N     |tree height|tree height|
|weighted QU|      N     |  lgN  | lgN  |

### Analysis of Algorithms

 Here is the issues from the poin of view of different types of characters:

* Programmer needs to develop a working solution.
* Client wants to solve problem efficiently.
* Theoretician wants to understand.
* Basic bloking and tackling is sometimes necessary.
* Student might play any or all of these roles someday.

#### Reasons to analyze algorithms

* Predict performance
* Compare algorithms.
* Provide guarantees.
* Understand theoretical basis.
* Primary practical reason: avoid performance bugs.

**A scientific method applied to analysis of algorithms has it's Principles, that are experiments must be reproducible and hypotheses must be falsifiable.**

