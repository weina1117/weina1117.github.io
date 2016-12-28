---
layout: post
title: Algorithms 4th Chapter 4
---

Chapter 4 Graphs
----------------------

This chapter will cover these four main topics:

- [4.1 Undirected Graphs](#41-undirected-graphs)
- [4.2 Directed Graphs](#42-directed-graphs)
- [4.3 Minimum Spanning Trees](#43-minimum-spanning-trees)
- [4.4 Shortest Paths](#44-shortest-paths)

4.1 Undirected Graphs
----------------------

**Graphs** 
A graph is a set of vertices and a collection of edges that each connect a pair of vertices. 
We use the names $0$ through $V-1$ for the vertices in a V-vertex graph.

**Definitions**

* A self-loop is an edge that connects a vertex to itself.
* Two edges are parallel if they connect the same pair of vertices.
* When an edge connects two vertices, we say that the vertices are adjacent to one another and 
  that the edge is incident on both vertices.
* The degree of a vertex is the number of edges incident on it.
* A subgraph is a subset of a graph's edges (and associated vertices) that constitutes a graph.
* A path in a graph is a sequence of vertices connected by edges. 
  A simple path is one with no repeated vertices.
* A cycle is a path (with at least one edge) whose first and last vertices are the same. 
  A simple cycle is a cycle with no repeated edges or vertices (except the requisite repetition 
  of the first and last vertices).
* The length of a path or a cycle is its number of edges.
* We say that one vertex is connected to another if there exists a path that contains both of them.
* A graph is connected if there is a path from every vertex to every other vertex.
* A graph that is not connected consists of a set of connected components, which are maximal 
  connected subgraphs.
* An acyclic graph is a graph with no cycles.
* A tree is an acyclic connected graph.
* A forest is a disjoint set of trees.
* A spanning tree of a connected graph is a subgraph that contains all of that graph's vertices and is 
  a single tree. A spanning forest of a graph is the union of the spanning trees of its connected components.
* A bipartite graph is a graph whose vertices we can divide into two sets such that all edges connect 
  a vertex in one set with a vertex in the other set.

Typical graph-processing code 

{% highlight java %} 

//compute the degree of v
public static int degree(Graph G, int v)
{
	int degree = 0;
	for (int w : G.adj(v)) degree++;
	return degree;
}

//compute maximum degree
public static int maxDegree(Graph G)
{
	int max = 0;
	for (int v = 0; v < G.V(); v++)
		if (degree(G, v) > max)
			max = degree(G, v);
	return max;
}

//compute average degree 
public static int avgDegree(Graph G)
{ return 2 * G.E() / G.V(); }

//count self-loops
public static int numberOfSelfLoops(Graph G)
{
	int count = 0;
	for (int v = 0; v < G.V(); v++)
		for (int w : G.adj(v))
			if (v == w) count++;
	return count/2; // each edge counted twice
}

//string representation of the graph’s adjacency lists (instance method in Graph)
public String toString()
{
	String s = V + " vertices, " + E + " edges\n";
	for (int v = 0; v < V; v++)
	{
		s += v + ": ";
		for (int w : this.adj(v))
			s += w + " ";
		s += "\n";
	}
	 return s;
}

{% endhighlight %}

4.2 Directed Graphs
-------------------



4.3 Minimum Spanning Trees
--------------------------


4.4 Shortest Paths
------------------

