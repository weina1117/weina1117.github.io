---
layout: post
title: Algorithms 4th Chapter 1
---

Chapter 1 Fundamentals
----------------------

This chapter will cover these five main topics:

### 1.1 Basic Programming Model

### 1.2 Data Abstraction

### 1.3 Bags, Queues and Stacks

### 1.4 Analysis of Algorithms

### 1.5 Case Study: Union Find

1.1 Basic Programming Model
---------------------------

```sequence

Java Program -> Library of Static Methods: 
             -> Data-type Definition:

```

A java program(class) is either a library of static methods(functions) or 
a data type definition.

```sequence
Java basic structure -> Primitive Data type
					 -> Statements
					 -> Arrays
					 -> Strings
					
					 --> Static Method
					 --> Data Abstraction
					 --> Standard I/O
```

##### Primitive Data type

![int]() 

![double]() 

![boolean]()

![long]()

![char]()

![short]()

![float]()

![byte]()

##### Statement

* Declaration: A declaration statement, java compiler checks for consistency.
              Since java is a *strongly typed* language.

* Assignment: An assignment statement.

* Conditional: A conditional statement.

* Loops: A loops statement, while loop can be express compactly with for notation

```java

for (<initialize>; <boolean expression>;<increment>)
{
	<block statments>
}
```

``` java

<initialize>;
while(<boolean expression>)
{
	<block statments>
	<increment>;
}
```


* Call and Return: Calls, and returns, relate to static methods.

Simply to remember is:

1. Initializing declaration

2. ![Implicit Assignments]()

3. Single - Statement blocks

##### Arrays

* stores a sequence of same type values, once we want to use one value of them, 
  number and index them
* To create a new array through the keyword * new * at the run time.
* Don't omit anyone step!! 
  Following: declaring, creating and initializing.

* Array is initialized with a sequential of memory. Once it was created, 
  one can only change its content, but cannot change its size.
  The name of the array present the entire array. The array is a quotative type. 
  If you want to copy an array, what you should do is to declare, create, and 
  initialize a new array and then copy all the values to the new array. See task3.

* Two-dimentional array: M by N, that are M rows and N columns.

[Implementation (code fragment)](\[Pearson\]\ -\ Algorithms,\ 4th\ ed.\ -\ \[Sedgewick,\ Wayne\].pdf)

```java

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
```

##### Static Methods

* Static methods are called functions since they can behave like mathematical functions.
* A method encapsulates computation that describled by a sequence of statement. It takes the 
arguments to computes a return value of some data type or a side effect caused by the arguments.
* Typical implementations of static method:

```java

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

// task3: primality test(Can not understand why i*i < N)
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
{return Math.sqrt(a*a + b*b)}

// task6: harmonic number
public static double H(int N)
{
	double sum = 0.0
	for(int i = 1; i < N; i++)
		sum += 1.0/i; // note: sum(1/i)
	return sum;
}

```
* Properties of Method worth nothing as follow: *
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

* goal of APIs: Seperate the client from the implementation.

* Strings:

##### Standard I/O

* ** Formatted output: ** printf() takes two arguments. The first argument is a format string. 
  Such like ("%14d",x) begin with % and ends with one-letter convertion code.

* Between % and the conversion code is an integer value that specifies the field width 
  of the conversion value. If we want a space on right, just insert a minus sign before
  the field width. Following the width, we have the options to assign the number of 
  digits to put after the decimal point (thr precision) for a double value or the number
  of characters to take from the beginning of the string for a string value. 

* ** Redirection and Piping **
  We can redirect the standard output or input to a file.

* data abstraction: known as object-oriented programming.

##### BinarySearch

```java

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
```

```sh
$ java BinarySearch tinyW.txt < tinyT.txt
50
99
13

```
// 1. tinyW.txt is white list, sorted it at first;
   2. redirct input, take white list as an input other than pinput by user;
   3. testing tinyT.txt records line by line, if the records is not on the 
      whilt list, then print this records.

1.2 Data Abstraction
--------------------

An abstract data type (ADT) is a data type whose representation is hidden from 
the clien. Implementing an ADT as a Java class is similar with implementing a
function library as a set of static methods. 

##### Using Abstract Data Type

** The differents between ADT and static method library: **

* Instance methods has no static key word.
* ADT could have the constructor function, which has the same name as the 
  class and no return type.
* coding will be:
  1. Declare variables;
  2. Create objects to hold data-type values;
  3. Provide access to the values for instance methods to operate on them.

** Objects ** 
  
* An object is an entity that can take on a data-type value.
* Three essential properties of object: * state, identity, and behavior. *
* The implementation has the sole responsibility for maintaining an objects identity 
  so that the client code can use a data-type without regard to representation of 
  its state by comforming to an API that describes an object's behavior.



##### Example of Abstract Data Type



##### Implementing an Abstract Data Type



##### Data Type Design



1.3 Bags, Queues and Stacks
---------------------------




1.4 Analysis of Algorithms
--------------------------




1.5 Case Study: Union Find
--------------------------

