--- 
layout: post 
title: How to set up java programming environment 
---

This document instructs you on how to set up java programming environment for
Macbook by iterm. It provides a step-by-step guide for creating, compiling and
exacuting my first java program using iterm.  Here I take the example from 
[Algorithms 4th Edition](http://algs4.cs.princeton.edu/). And the [Java code](http://algs4.cs.princeton.edu/code/) 
of this book is given.

0 Install the programming Environment 
-------------------------------------

Mac OS X Terminal(manual). Downloads stdlib.jar and algs4.jar to a folder, say
`~/algs4`. Depending on your shell, add the following line or lines to the file
specified:

* First you chan check your shell type as follow: typed `echo $SHELL` on your
terminal, if the output is `/bin/bash`, that means your shell is bash.
* Add the following line to the file` ~/.bash_profile` or `~/.bash_login`(if it
  exists); otherwise create a file and add it to the file `~/.profile`(create it
first). 

Here is some basic commands covered in this section:

* changed the directory to the created file as follow:

```sh
$ cd ~/Workspace/edu.priceton.cs.algs4/
```

* then, get the file [BinarySearch.java]( http://algs4.cs.princeton.edu/code/edu/princeton/cs/algs4/BinarySearch.java)
through `wget` command add the link address from the book(algs4)

```sh
$ wget http://algs4.cs.princeton.edu/code/edu/princeton/cs/algs4/BinarySearch.java
```

* vim to the file, here should be noticed that you should change something in
  here:

you will see `package edu.princeton.cs.algs4` that shoul be change to
`import edu.princeton.cs.algs4.*` Why we chage the `package` to `import`?
This is manually input to show the dependency so that the computer could find 
the comipling class file through file name

```sh 
$ export CLASSPATH=$CALSSPATH:/Users/weinaguo/Workspace/algs4.jar:/Users/weinaguo/Workspace/edu.princeton.cs.algs4
```

The first part of this file is introductions, here we can easily find the link 
address of the data file. We still need to get this data file through `wget`

```sh
$ wget http://algs4.cs.princeton.edu/11model/tinyW.txt
``` 

* and then, you can write your java code here. saved and quit after you done.

1 Compile the program 
----------------------
To compile the program, we can use the following command:

```sh
$ javac BinarySearch.java 
```

After the set up steps, you will compiling successful.

2 Exacute the program 
---------------------

* input the data file and run it 

```sh 
$ java BinarySearch < tinyW.txt 
```

Classpath is the connection between the java runtime and the filesystem. 
It defines where the interpreter looks forclass files to load. 
The command "echo $Classpath" means that to find the compiled files through the 
name. So, here we need to add the path manually that's our first import. 

**Note**: In the path here has".", before the dot that is the package, after
the dot that is the complied java class file.
