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

Typical digraph applications

| 	application   |    vertex    |         edge         |
| ---------------:|:------------:|:--------------------:|
| food web        | species      | predator-prey        |
| internet        | content page | hyperlink            |
| program         | module       | external reference   |
| cellphone       | phone        | call                 |
| scholarship     | paper        | citation             |
| financial       | stock        | transaction          |
| internet        | machine      | connection           |

**Digraph**
A directed graph (or digraph) is a set of vertices and a collection of directed edges that 
each connects an ordered pair of vertices. A directed edge points from the first vertex in 
the pair and points to the second vertex in the pair. Named `0` through `V-1` for the
vertices in a V-vertex graph. Ignoring anomalies, there are four different ways in which 
two vertices might be related in a digraph: no edge; an edge `v->w` from `v` to `w`; an edge `w->v`
from `w` to `v`; or two edges `v->w` and `w->v`, which indicate connections in both directions. 

**Definitions**

* A self-loop is an edge that connects a vertex to itself.
* Two edges are parallel if they connect the same ordered pair of vertices.
* The outdegree of a vertex is the number of edges pointing from it. 
  The indegree of a vertex is the number of edges pointing to it.
* A subgraph is a subset of a digraph's edges (and associated vertices) 
  that constitutes a digraph.
* A directed path in a digraph is a sequence of vertices in which there is a (directed) edge 
  pointing from each vertex in the sequence to its successor in the sequence. 
  A simple path is one with no repeated vertices.
* A directed cycle is a directed path (with at least one edge) whose first and last vertices 
  are the same. A simple cycle is a cycle with no repeated edges or vertices (except the 
  requisite repetition of the first and last vertices).
* The length of a path or a cycle is its number of edges.
* A vertex `w` is reachable from a vertex `v` if there exists a directed path from `v` to `w`.
* Two vertices `v` and `w` are strongly connected if they are mutually reachable: 
  there is a directed path from `v` to `w` and a directed path from `w` to `v`.
* A digraph is strongly connected if there is a directed path from every vertex to every other vertex.
* A digraph that is not strongly connected consists of a set of strongly-connected components, 
  which are maximal strongly-connected subgraphs.
* A directed acyclic graph (or DAG) is a digraph with no directed cycles.

Directed graph (digraph) data type
{% highlight java %}
public class Digraph
{
	private final int V;
 	private int E;
 	private Bag<Integer>[] adj;
 	public Digraph(int V)
 	{
 		this.V = V;
 		this.E = 0;
 		adj = (Bag<Integer>[]) new Bag[V];
 		for (int v = 0; v < V; v++)
 			adj[v] = new Bag<Integer>();
 	}
 	public int V() { return V; }
 	public int E() { return E; }
 	public void addEdge(int v, int w)
 	{
 		adj[v].add(w);
 		E++;
 	}
 	public Iterable<Integer> adj(int v)
 	{	return adj[v]; }
 	public Digraph reverse()
 	{
 		Digraph R = new Digraph(V);
 		for (int v = 0; v < V; v++)
 			for (int w : adj(v))
 				R.addEdge(w, v);
 		return R;
 	}
}
{% endhighlight %}

**Reachability in digraphs**
* Single-source reachability. Given a digraph and a source vertex `s`, support queries
of the form Is there a directed path from `s` to a given target vertex `v`? 

* Multiple-source reachability. Given a digraph and a set of source vertices, support
queries of the form Is there a directed path from any vertex in the set to a given
target vertex `v`? 

* Mark-and-sweep garbage collection. An important application of multiple-source reachability
is found in typical memory-management systems, including many implementations of Java. 

**Proposition D** 
DFS marks all the vertices in a digraph reachable from a given set of sources in time 
proportional to the sum of the outdegrees of the vertices marked.

**Reachability in digraphs**

{% highlight java %}
public class DirectedDFS
{
	private boolean[] marked;
 	public DirectedDFS(Digraph G, int s)
 	{
 		marked = new boolean[G.V()];
 		dfs(G, s);
 	}
 	public DirectedDFS(Digraph G, Iterable<Integer> sources)
 	{
 		marked = new boolean[G.V()];
 		for (int s : sources)
 			if (!marked[s]) dfs(G, s);
 	}
 	private void dfs(Digraph G, int v)
 	{
 		marked[v] = true;
 		for (int w : G.adj(v))
 			if (!marked[w]) dfs(G, w);
 	}
 	public boolean marked(int v)
 	{	return marked[v]; }
 	public static void main(String[] args)
 	{
 		Digraph G = new Digraph(new In(args[0]));
 		Bag<Integer> sources = new Bag<Integer>();
 		for (int i = 1; i < args.length; i++)
 			sources.add(Integer.parseInt(args[i]));
 	DirectedDFS reachable = new DirectedDFS(G, sources);
 	for (int v = 0; v < G.V(); v++)
 		if (reachable.marked(v)) StdOut.print(v + " ");
 	StdOut.println();
 	}
}

{% endhighlight %}

**Finding a directed cycle**

{% highlight java %}
public class DirectedCycle
{
	private boolean[] marked;
 	private int[] edgeTo;
 	private Stack<Integer> cycle; // vertices on a cycle (if one exists)
 	private boolean[] onStack;    // vertices on recursive call stack
 	public DirectedCycle(Digraph G)
 	{
 		onStack = new boolean[G.V()];
 		edgeTo  = new int[G.V()];
 		marked  = new boolean[G.V()];
 		for (int v = 0; v < G.V(); v++)
 			if (!marked[v]) dfs(G, v);
 	}
 	private void dfs(Digraph G, int v)
 	{
 		onStack[v] = true;
 		marked[v] = true;
 		for (int w : G.adj(v))
 			if (this.hasCycle()) return;
 			else if (!marked[w])
 			{	edgeTo[w] = v; dfs(G, w); }
 			else if (onStack[w])
 			{
 				cycle = new Stack<Integer>();
 				for (int x = v; x != w; x = edgeTo[x])
 					cycle.push(x);
 					cycle.push(w);
 					cycle.push(v);
 			}
 		onStack[v] = false;
 	}
 	public boolean hasCycle()
 	{	return cycle != null; }
 	public Iterable<Integer> cycle()
 	{	return cycle; }
}

{% endhighlight %}

**Depth-first search vertex ordering in a digraph**

{% highlight java %}

public class DepthFirstOrder
{
	private boolean[] marked;
 	private Queue<Integer> pre;         // vertices in preorder
 	private Queue<Integer> post;        // vertices in postorder
 	private Stack<Integer> reversePost; // vertices in reverse postorder
 	public DepthFirstOrder(Digraph G)
 	{
 		pre         = new Queue<Integer>();
 		post        = new Queue<Integer>();
 		reversePost = new Stack<Integer>();
 		marked = new boolean[G.V()];
 		for (int v = 0; v < G.V(); v++)
 			if (!marked[v]) dfs(G, v);
 	}
 	private void dfs(Digraph G, int v)
 	{
 		pre.enqueue(v);
 		marked[v] = true;
 		for (int w : G.adj(v))
 			if (!marked[w])
 				dfs(G, w);
 	post.enqueue(v);
 	reversePost.push(v);
 	}
 	public Iterable<Integer> pre()
 	{	return pre; }
 	public Iterable<Integer> post()
 	{	return post; }
 	public Iterable<Integer> reversePost()
 	{	return reversePost; }
}
{% endhighlight %}

**Topological sort**

{% highlight java %}
public class Topological
{
	private Iterable<Integer> order;           // topological order
 	public Topological(Digraph G)
 	{
 		DirectedCycle cyclefinder = new DirectedCycle(G);
 		if (!cyclefinder.hasCycle())
 		{
 			DepthFirstOrder dfs = new DepthFirstOrder(G);
 			order = dfs.reversePost();
 		}
 	}
 	public Iterable<Integer> order()
 	{	return order; }
 	public boolean isDAG()
 	{	return order == null; }
 	public static void main(String[] args)
 	{
 		String filename = args[0];
 		String separator = args[1];
 		SymbolDigraph sg = new SymbolDigraph(filename, separator);
 		Topological top = new Topological(sg.G());
 		for (int v : top.order())
 			StdOut.println(sg.name(v));
 	}
}

{% endhighlight %}

**Proposition**
A digraph has a topological order if and only if it is a DAG.

**Proposition**
Reverse postorder in a DAG is a topological sort.

**Proposition**
With depth-first search, we can topologically sort a DAG in time proportional to $V + E$.

Strong connectivity. 
Strong connectivity is an equivalence relation on the set of vertices:
* Reflexive: Every vertex `v` is strongly connected to itself.
* Symmetric: If `v` is strongly connected to `w`, then `w` is strongly connected to `v`.
* Transitive: If `v` is strongly connected to `w` and `w` is strongly connected to `x`, 
  then `v` is also strongly connected to `x`.

**Kosaraju’s algorithm for computing strong components**

{% highlight java %}
public class KosarajuSCC
{
	private boolean[] marked;    // reached vertices
 	private int[] id;            // component identifiers
 	private int count;           // number of strong components
 	public KosarajuSCC(Digraph G)
 	{
 		marked = new boolean[G.V()];
 		id = new int[G.V()];
 		DepthFirstOrder order = new DepthFirstOrder(G.reverse());
 		for (int s : order.reversePost())
 			if (!marked[s])
 			{	dfs(G, s); count++; }
 	}
 	private void dfs(Digraph G, int v)
 	{
 		marked[v] = true;
 		id[v] = count;
 		for (int w : G.adj(v))
 			if (!marked[w])
 				dfs(G, w);
 	}
 	public boolean stronglyConnected(int v, int w)
 	{	return id[v] == id[w]; }
 	public int id(int v)
 	{	return id[v]; }
 	public int count()
 	{	return count; }
}

{% endhighlight %}

**Proposition**
The Kosaraju-Sharir algorithm uses preprocessing time and space proportional to $V + E$ to 
support constant-time strong connectivity queries in a digraph.

**Transitive closure** 
The transitive closure of a digraph `G` is another digraph with the same set of vertices, 
but with an edge from `v` to `w` if and only if `w` is reachable from `v` in `G`.

**Digraph-processing problems addressed in this section**

|                       problem                     |          solution             |
|:-------------------------------------------------:|:-----------------------------:|
| single- and multiple-source reachability          |  DirectedDFS                  |
| single-source directed paths                      |  DepthFirstDirectedPaths      |
| single-source shortest directed paths             |  BreadthFirstDirectedPaths    |
| directed cycle detection                          |  DirectedCycle                |
| depth-first vertex orders                         |  DepthFirstOrder              |
| precedence-constrained scheduling                 |  Topological                  |
| topological sort                                  |  Topological                  |
| strong connectivity                               |  KosarajuSCC                  |
| all-pairs reachability                            |  TransitiveClosure            |

**The difference between DFS and BFS**
* DFS use stack, BFS use queue
* Both have the similar build and control systerm, the only basic difference is how to choose
the next node. DFS choose the child node as the next node, BFS choose the brother node.
* DFS can find the result quickly, BSF can find the optimal answer.

4.3 Minimum Spanning Trees
--------------------------

**Minimum spanning tree**
An edge-weighted graph is a graph where we associate weights or costs with each edge. 
A minimum spanning tree (MST) of an edge-weighted graph is a spanning tree whose weight (the 
sum of the weights of its edges) is no larger than the weight of any other spanning tree.

**Assumptions**
* The graph is connected. 
* The edge weights are not necessarily distances. 
* The edge weights may be zero or negative. 
* The edge weights are all different. 

**Underlying principles**
* Adding an edge that connects two vertices in a tree creates a unique cycle.
* Removing an edge from a tree breaks it into two separate subtrees.

**Proposition. (Cut property)**
Given any cut in an edge-weighted graph (with all edge weights distinct), the crossing edge 
of minimum weight is in the MST of the graph.

**Proposition. (Greedy MST algorithm)** 
The following method colors black all edges in the the MST of any connected edge-weighted 
graph with `V` vertices: Starting with all edges colored gray, find a cut with no black edges, 
color its minimum-weight edge black, and continue until `V-1` edges have been colored black.

**Prim's algorithm**
Prim's algorithm works by attaching a new edge to a single growing tree at each step: Start 
with any vertex as a single-vertex tree; then add `V-1` edges to it, always taking next (coloring 
black) the minimum-weight edge that connects a vertex on the tree to a vertex not yet on the tree 
(a crossing edge for the cut defined by tree vertices).

How do we (efficiently) find the crossing edge of minimal weight?
* Lazy implementation. 
We use a priority queue to hold the crossing edges and find one of minimal weight. Each time that 
we add an edge to the tree, we also add a vertex to the tree. To maintain the set of crossing 
edges, we need to add to the priority queue all edges from that vertex to any non-tree vertex. But 
we must do more: any edge connecting the vertex just added to a tree vertex that is already on the 
priority queue now becomes ineligible (it is no longer a crossing edge because it connects two tree 
vertices). The lazy implementation leaves such edges on the priority queue, deferring the 
ineligibility test to when we remove them.

![sample post]({{site.baseurl}}/images/algorithms4/lazy.png)

* Eager implementation. 
To improve the lazy implementation of Prim's algorithm, we might try to delete ineligible edges 
from the priority queue, so that the priority queue contains only the crossing edges. But we can 
eliminate even more edges. The key is to note that our only interest is in the minimal edge from 
each non-tree vertex to a tree vertex. When we add a vertex `v` to the tree, the only possible change 
with respect to each non-tree vertex `w` is that adding `v` brings `w` closer than before to the tree. In 
short, we do not need to keep on the priority queue all of the edges from `w` to vertices tree—we just 
need to keep track of the minimum-weight edge and check whether the addition of `v` to the tree 
necessitates that we update that minimum (because of an edge `v-w` that has lower weight), which we can 
do as we process each edge in s adjacency list. In other words, we maintain on the priority queue just 
one edge for each non-tree vertex: the shortest edge that connects it to the tree.

![sample post]({{site.baseurl}}/images/algorithms4/eager.png)

**Proposition**
Prim's algorithm computes the MST of any connected edge-weighted graph. The lazy version of Prim's 
algorithm uses space proportional to `E` and time proportional to `E log E` (in the worst case) to 
compute the MST of a connected edge-weighted graph with `E` edges and `V` vertices; the eager 
version uses space proportional to `V` and time proportional to `E log V` (in the worst case).

**Kruskal's algorithm** 
Kruskal's algorithm processes the edges in order of their weight values (smallest to largest), taking 
for the MST (coloring black) each edge that does not form a cycle with edges previously added, stopping 
after adding `V-1` edges. The black edges form a forest of trees that evolves gradually into a single
tree, the MST.

![sample post]({{site.baseurl}}/images/algorithms4/kruskals.png)

**Proposition**
Kruskal's algorithm computes the MST of any connected edge-weighted graph with `E` edges and `V` vertices
using extra space proportional to `E` and time proportional to `E log E` (in the worst case).

4.4 Shortest Paths
------------------

**Shortest paths**
An edge-weighted digraph is a digraph where we associate weights or costs with each edge. 
A shortest path from vertex `s` to vertex `t` is a directed path from `s` to `t` with the property that 
no other such path has a lower weight.

**Properties & Assumptions**

* Paths are directed. 
A shortest path must respect the direction of its edges.
* The weights are not necessarily distances. 
Geometric intuition can be helpful, but the edge weights weights might represent time or cost.
* Not all vertices need be reachable. 
If `t` is not reachable from `s`, there is no path at all, and therefore there is no shortest path 
from `s` to `t`.
* Negative weights introduce complications. 
We assume that edge weights are positive (or zero).
* Shortest paths are normally simple. 
Here we ignore zero-weight edges that form cycles, so that the shortest paths they find have no cycles.
* Shortest paths are not necessarily unique. 
There may be multiple paths of the lowest weight from one vertex to another; we are content to find 
any one of them.
* Parallel edges and self-loops may be present. Here we assume that parallel edges are not present 
and use the notation `v->w` to refer to the edge from `v` to `w`, but our code handles them without 
difficulty.

**Single-source shortest paths**
Given an edge-weighted digraph and a designated vertex `s`, a shortest-paths tree (SPT) is a subgraph 
containing `s` and all the vertices reachable from `s` that forms a directed tree rooted at `s` such 
that every tree path is a shortest path in the digraph.

The shortest paths with two vertex-indexed arrays:
* Edges on the shortest-paths tree: `edgeTo[v]` is the the last edge on a shortest path from `s` to `v`.
* Distance to the source: `distTo[v]` is the length of the shortest path from `s` to `v`.

**Relaxation**
Our shortest-paths implementations are based on an operation known as relaxation. 
We initialize `distTo[s]` to `0` and `distTo[v]` to infinity for all other vertices `v`.

* Edge relaxation. 
To relax an edge `v->w` means to test whether the best known way from `s` to `w` is to go from `s` to `v`,
then take the edge from `v` to `w`, and, if so, update our data structures.
* Vertex relaxation. 
All of our implementations actually relax all the edges pointing from a given vertex.

**Dijkstra's algorithm**
Dijkstra's algorithm initializing `dist[s]` to `0` and all other `distTo[]` entries to positive infinity. 
Then, it repeatedly relaxes and adds to the tree a non-tree vertex with the lowest `distTo[]` value, 
continuing until all vertices are on the tree or no non-tree vertex has a finite `distTo[]` value.

**Proposition**
Dijkstra's algorithm solves the single-source shortest-paths problem in edge-weighted digraphs with 
nonnegative weights using extra space proportional to `V` and time proportional to `E log V` (in the worst case).

Acyclic edge-weighted digraphs.
* Single-source shortest paths problem in edge-weighted DAGs. 
	It solves the single-source problem in linear time.
	It handles negative edge weights.
	It solves related problems, such as finding longest paths.
* The algorithm combines vertex relaxation with topological sorting. 
* Single-source longest paths problem in edge-weighted DAGs. 
* Critical path method. 

**PoProposition**
By relaxing vertices in topological order, we can solve the single-source shortest-paths and longest-paths 
problems for edge-weighted DAGs in time proportional to `E + V`.

**Shortest paths in general edge-weighted digraphs**
* Negative cycles. 

* Bellman-Ford algorithm. 
* Queue-based Bellman-Ford algorithm.
* Negative cycle detection. 
* Arbitrage detection. 

**Proposition**
There exists a shortest path from `s` to `v` in an edge-weighted digraph if and only if there exists at 
least one directed path from `s` to `v` and no vertex on any directed path from `s` to `v` is on a negative cycle.

**Proposition**
The Bellman-Ford algorithm solves the single-source shortest-paths problem from a given source `s` 
(or finds a negative cycle reachable from `s`) for any edge-weighted digraph with `V` vertices and `E` edges, 
in time proportional to `E V` and extra space proportional to `V`, in the worst case.

Performance characteristics of shortest-paths algorithms

![sample post]({{site.baseurl}}/images/algorithms4/performance.png)

