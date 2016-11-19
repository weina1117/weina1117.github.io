---
layout: post
title: How to set up java programming environment 
---

This document instructs you on how to set up java programming environment for Macbook by iterm. It provides a step-by-step guide for creating, compiling and exacuting my first java program using iterm.

> 0 Install the programming Environment

Mac OS X Terminal(manual). Downloads stdlib.jar and algs4.jar to a folder, say **~/algs4**. Depending on your shell, add the following line or lines to the file specified:

* first you chan check your shell type as follow:
  typed**\"echo $SHELL\"** on your terminal, if the output is **/bin/bash**, that means your shell is bash.
* Add the following line to the file** ~/.bash_profile** or **.bash_login**(if it exists);
  otherwise create a file and add it to the file **~/.profile**(create it first)
    
    'export CALSSPATH = $CALSSPATH: /Users/weinaguo/Workspace/algs4.jar:/Users/weinaguo/Workspace/edu.princeton.cs.algs4'

* then, get the file through **"wget"** command add the link address from the book(algs4)
* vim to the file, here should be noticed that you should change something in here:
    
    'you will see \" package edu. \" that shoul be change to \"import edu.princeton.cs.algs4.\*\"'

* and then, you can write your java code here. saved and quit after you done.

> 1 Compile the program

* compiling
    
    'javac BinarySearch.java (here we take BinarySearch for example)'
  
    after the set up steps, you will be compiling successful.

>2 Exacute the program

* input the file and run it
    
    'java BinarySearch'
  
I'm at very begining. The first thing I learnt is managing the Java classpath after classpass set up. Classpath is the connection between the java runtime and the filesystem. It defines where the interpreter looks forclass files to load. the command "echo $Classpath" means that to find the compiled files through the name. So, here we need to add the path manually that's our first import. 
**Note**:
In the path here has".", before the dot that is the package, after the dot that is the complied java class file.
