---
layout: post
title: Algorithm 4th  
--- 

### **Two Classic Algorithm**

#### **Quick-find**

##### **Data Structure**

* Integer Array id[] of size N
* Interpretation: p and q are connected if and only if they have the same id.

##### This is so called eager algorithm,for solving kind activity problem.

**Find** Check if p and q have the same id.
**Union** To merge components containing p and q,change all entries whose id equals id[p] to id[q].

**Cost model**

 | algorithm | initialize | union | find |
 | --------- |:----------:|:-----:|-----:|
 |quick-find |      N     |   N   |   1  |


#### **Quick-union**

##### **Data Structure**

* Integer Array id[] of size N
* Interpretation: id[i] is parent of i.
* Root of i is id[id[id[...id[i]...]]].

**Find** Check if p and q have the same root.
**Union** To merge components containing p and q, set the id of p's root to the id of q's root.

**Cost model**

 | algorithm | initialize | union | find |
 | --------- |:----------:|:-----:|-----:|
 |quick-find |      N     |   N   |   1  |
 |quick-union|      N     |   N   |   N  | <--worst case

#### **Quick-find defect**

* Union too expensive(N array accesses).
* Trees are flat, but too expensive to keep them flat.

#### **Quick-union defect**

* Trees can get tall.
* Find too expensive(could be N array accesses).

#### **Improvements**Weighted quick-union

##### **Data Structure**

 Same as quick-union, but maintain extra array sz[i] to count number of objects in the tree rooted at i.

**Find** Identical to quick-union.
**Union** Modify quick-union to:
* Link root of smaller tree to root of larger tree.
* Update the sz[] array.

**Cost model**

 | algorithm | initialize | union | find |
 |-----------|------------|-------|------|
 |quick-find |      N     |   N   |   1  |
 |quick-union|      N     |   N   |   N  | 
 |weighted QU|      N     |  lgN  | lgN  |

### **Analysis of Algorithms**

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

##### A scientific method applied to analysis of algorithms has it's Principles, that are experiments must be reproducible and hypotheses must be falsifiable.

###### I will write the classic 3-sum problem next time.
