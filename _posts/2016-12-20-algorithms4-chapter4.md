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

Graph data type
{% highlight java %}

public class Graph
{
	private final int V; // number of vertices
 	private int E; // number of edges
 	private Bag<Integer>[] adj; // adjacency lists
 	public Graph(int V)
 	{
 		this.V = V; this.E = 0;
 		adj = (Bag<Integer>[]) new Bag[V]; // Create array of lists.
 		for (int v = 0; v < V; v++) // Initialize all lists
 			adj[v] = new Bag<Integer>(); // to empty.
 	}
 	public Graph(In in)
 	{
 		this(in.readInt()); // Read V and construct this graph.
 		int E = in.readInt(); // Read E.
 		for (int i = 0; i < E; i++)
 		{	// Add an edge.
 			int v = in.readInt(); // Read a vertex,
 			int w = in.readInt(); // read another vertex,
 			addEdge(v, w); // and add edge connecting them.
 		}
 	}
 	public int V() { return V; }
 	public int E() { return E; }
 	public void addEdge(int v, int w)
 	{
 		adj[v].add(w); // Add w to v’s list.
 		adj[w].add(v); // Add v to w’s list.
 		E++;
 	}
 	public Iterable<Integer> adj(int v)
 	{ return adj[v]; }
}

{% endhighlight %}

**Order-of-growth performance for typical Graph implementations**

![sample post]({{site.baseurl}}/images/algorithms4/performance1.png)

**Depth-first search**

Depth-first search to find paths in a graph

{% highlight java %}
public class DepthFirstPaths
{
	private boolean[] marked; // Has dfs() been called for this vertex?
 	private int[] edgeTo; // last vertex on known path to this vertex
 	private final int s; // source
 	public DepthFirstPaths(Graph G, int s)
 	{
 		marked = new boolean[G.V()];
 		edgeTo = new int[G.V()];
 		this.s = s;
 		dfs(G, s);
 	}
 	private void dfs(Graph G, int v)
 	{
 		marked[v] = true;
 		for (int w : G.adj(v))
 			if (!marked[w])
 			{
 				edgeTo[w] = v;
 				dfs(G, w);
 			}
 	}
 	public boolean hasPathTo(int v)
 	{ return marked[v]; }
 	public Iterable<Integer> pathTo(int v)
 	{
 		if (!hasPathTo(v)) return null;
 		Stack<Integer> path = new Stack<Integer>();
 		for (int x = v; x != s; x = edgeTo[x])
 			path.push(x);
 		path.push(s);
 		return path;
 	}
} 

{% endhighlight %}

**Proposition A** 
DFS marks all the vertices connected to a given source in time proportional to 
the sum of their degrees.

**Proposition A (continued)**
DFS allows us to provide clients with a path from a given source to any marked 
vertex in time proportional its length. 

**Breadth-first search** 

Breadth-first search to find paths in a graph

{% highlight java %}
public class BreadthFirstPaths
{
	private boolean[] marked; // Is a shortest path to this vertex known?
 	private int[] edgeTo; // last vertex on known path to this vertex
 	private final int s; // source
 	public BreadthFirstPaths(Graph G, int s)
 	{
 		marked = new boolean[G.V()];
 		edgeTo = new int[G.V()];
 		this.s = s;
 		bfs(G, s);
 	}
 	private void bfs(Graph G, int s)
 	{
 		Queue<Integer> queue = new Queue<Integer>();
 		marked[s] = true; // Mark the source
 		queue.enqueue(s); // and put it on the queue.
 		while (!q.isEmpty())
 		{
 			int v = queue.dequeue(); // Remove next vertex from the queue.
 			for (int w : G.adj(v))
 				if (!marked[w]) // For every unmarked adjacent vertex,
 				{
 					edgeTo[w] = v; // save last edge on a shortest path,
 					marked[w] = true; // mark it because path is known,
 					queue.enqueue(w); // and add it to the queue.
 				}
 		}
 	}
 	public boolean hasPathTo(int v)
 	{	return marked[v]; }
 	public Iterable<Integer> pathTo(int v)
 	// Same as for DFS Code.
}

{% endhighlight %}

**Proposition B**
For any vertex $v$ reachable from $s$, BFS computes a shortest path from $s$ to $v$ 
(no path from $s$ to $v$ has fewer edges)

**Proposition B (continued)**
BFS takes time proportional to $V+E$ in the worst case.

**Depth-first search to find connected components in a graph**

{% highlight java %}
public class CC
{
	private boolean[] marked;
 	private int[] id;
 	private int count;
 	public CC(Graph G)
 	{
 		marked = new boolean[G.V()];
 		id = new int[G.V()];
 		for (int s = 0; s < G.V(); s++)
 			if (!marked[s])
 			{
 				dfs(G, s);
 				count++;
 			}
 	}
 	private void dfs(Graph G, int v)
 	{
 		marked[v] = true;
 		id[v] = count;
 		for (int w : G.adj(v))
 			if (!marked[w])
 				dfs(G, w);
 	}
 	public boolean connected(int v, int w)
 	{	return id[v] == id[w]; }
 	public int id(int v)
 	{	return id[v]; }
 	public int count()
 	{	return count; }
}

{% endhighlight %}

**Proposition C**
DFS uses preprocessing time and space proportional to $V+E$ to support 
constant-time connectivity queries in a graph.

**Symbol graph data type**

{% highlight java %}
public class SymbolGraph
{
	private ST<String, Integer> st;                // String -> index
 	private String[] keys;                         // index -> String
 	private Graph G;                               // the graph
 	public SymbolGraph(String stream, String sp)
 	{
 		st = new ST<String, Integer>();
 		In in = new In(stream);                    // First pass
 		while (in.hasNextLine())                   // builds the index
 		{
 			String[] a = in.readLine().split(sp);  // by reading strings
 			for (int i = 0; i < a.length; i++)     // to associate each
 				if (!st.contains(a[i]))            // distinct string
 					st.put(a[i], st.size());       // with an index.
 		}
 		keys = new String[st.size()];               // Inverted index
 		for (String name : st.keys())               // to get string keys
 			keys[st.get(name)] = name;              // is an array.
 		G = new Graph(st.size());
 		in = new In(stream);                        // Second pass
 		while (in.hasNextLine())                    // builds the graph
 		{
 			String[] a = in.readLine().split(sp);   // by connecting the
 			int v = st.get(a[0]);                   // first vertex
 			for (int i = 1; i < a.length; i++)      // on each line
 				G.addEdge(v, st.get(a[i]));         // to all the others.
 		}
 	}
 	public boolean contains(String s)  {	return st.contains(s); }
 	public int index(String s)         {	return st.get(s); }
 	public String name(int v)          {	return keys[v]; }
 	public Graph G()                   {	return G; }
}

{% endhighlight %}

**Degrees of separation**

{% highlight java %}
public class DegreesOfSeparation
{
	public static void main(String[] args)
 	{
 		SymbolGraph sg = new SymbolGraph(args[0], args[1]);
 		Graph G = sg.G();
 		String source = args[2];
 		if (!sg.contains(source))
 		{	StdOut.println(source + " not in database."); return; }
 		int s = sg.index(source);
 		BreadthFirstPaths bfs = new BreadthFirstPaths(G, s);
 		while (!StdIn.isEmpty())
 		{
 			String sink = StdIn.readLine();
 			if (sg.contains(sink))
 			{
 				int t = sg.index(sink);
 				if (bfs.hasPathTo(t))
 					for (int v : bfs.pathTo(t))
 						StdOut.println(" " + sg.name(v));
 				else StdOut.println("Not connected");
 			}
 			else StdOut.println("Not in database.");
 		}
 	}
}

{% endhighlight%}

(Undirected) graph-processing problems addressed in this section
|            problem                 |        solution                       |
|:----------------------------------:|:-------------------------------------:|
| single-source connectivity               DepthFirstSearch  
| single-source paths                      DepthFirstPaths 
| single-source shortest paths             BreadthFirstPaths 
| connectivity                             CC  
| cycle detection                          Cycle
| two-colorability (bipartiteness)         TwoColor 

4.2 Directed Graphs
-------------------



4.3 Minimum Spanning Trees
--------------------------


4.4 Shortest Paths
------------------

