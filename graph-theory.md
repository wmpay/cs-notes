# The Fascinating World of Graph Theory

# Chapter 1

- Graph - finite nonempty set V of objects together with a set E consisting of 2-element subsets of V
- Multigraph - one edge joins the same set of vertices. E.G. V = {u,w} E = {uw, uw}
- order of a graph - n = |V| (magnitude of V, number of vertices)
- size of graph - m = |E|
- vertices are adjacent if they are joined by an edge
- vertices can be labeled or unlabeled
- degree of vertex - number of edges joining it
- graph with 1 vertex - trivial graph
- graph with at least 2 - non trivial
- graph with no edges - empty
- graph with at least one edge - nonempty
- sum of degrees of vertices is twice the number of edges
	- ```math
			degv1 + degv2 + ... + devvn = 2m
		```
	- every graph has an even number of odd vertices

# Chapter 2

- irregular graph - graph order 2 or more with every 2 vertices having different degrees
	 - no graph is irregular
- almost irregular graph - exactly one pair of vertices with same degree
- complement of a graph - v and w are adjacent in G and not adjacent in complement of G, for every edge
	- deg compG v = n - 1 - deg G v
- for each integer n greater or equal to 2, there are exactly two almost irregular graphs and they are complements of each other
- for every given set of positive integers whose largest integer is n, there is a graph of order n + 1, the degrees of
 whose vertices are precisely these integers
- a multigraphs edges between the same vertices can be replaced with a single edge that is weighted
- for every graph G of order 3 or more having at most one isolated vertex and not containing two adjacent end vertices,
 there is a regular weighted graph H whose underlying graph is G
- r regular graph - every vertex is degree r
- for every two integers r and n, both odd, with 0 lte r lte n - 1, there exists an r regular graph of order n
- subgraph - H is a subgraph of G if every vertex and edge of H is a vertex and edge of G
- proper subgraph - a subgraph of G that is not G (G is a subgraph of itself)
- spanning subgraph - same vertices
- for any graph G there exists a regular graph F containing G as an induced subgraph
- induced subgraph - vertex set S where two vertices u and v are only adjacent of they are adjacent in G
- special kind of induced subgraph - vertex deleted subgraph - one vertex is removed
- for any graph G, there exists a regular graph F containing G is an induced subgraph
- isomorphic graphs - same structure - G can be turned into H by relabeling vertices

# Chapter 3

- Path - subgraph of G whose vertices are ordered such that adjacent list elements are connected by edges
	- P = (u=v0, v1, ... , vk=v) such that v0v1, ... , vk-1vk are all edges of P. P is u -v path.
	- number of edges of a path = length
- G is connected if it contains an x-y path for every two vertices x and y
	- else it is disconnected
- component of G is a connected subgraph of G that is not a proper subgraph of any other connected subgraph of G
	 - a connected graph G has one component, G
- a vertex v is a cut vertex if G - v has more components than G
	 - v is a cut vertex of connected graph G if G - v is disconnected
- an edge e is a bridge of G if G - e has more components than G
	- it is a bridge if does not belong to a cycle of G
- length of Path Pi = i - 1
	- e.g. P3 = (x, y, z) length = 2
- distance d(x, y) between vertices x and y is the minimum length of an x - y path in G
- eccentricity of a vertex v is the distance to the farthest vertex
- central vertex - vertex of minimum eccentricity
- v is an eccentric vertex of u if v is the farthest vertex from u
- a graph G of order n gte 2 is bipartite if the vertex set of G can be partitioned into two sets U and W such that every vertex of U joins a vertex of W
	- complete bipartite graph - Ks,t
	- order s + t
	- if s = t = r, it is an r regular graph Kr,r
- a graph G is bipartite if and only if G contains no odd cycles
- locating set - an ordered set S of vertices in a connected Graph G such that every two vertices u and v of G there is some vertex wi in S whose distance to u is different from its distance to v
- minimum number of vertices in locating set S is location number
- a vertex v in graph G is said to dominate a vertex u if u is adjacent to v or u=v
- a set S of vertices in G is a dominating set of G if every vertex of G either belongs to S or is adjacent to some vertex in S
- minimum number of vertices dominating a graph is domination number y(G) y=gamma

Chapter 4

- Tree - a connected graph that contains no cycles
- a graph G is a tree if and only if every two vertices of G are connected by only one path
- each vertex of degee 1 is called a leaf
- every tree with at least two vertices has at least two leaves
- every tree with n vertices has n - 1 edges
- let d = maximum degree of T, and ni = vertex with degree i
	- number of leaves = n1 = 2 + n3 + 2n4 + 4n5 + ... + (d-2)nd
	- number of leaves in a tree is not dependant on vertices of degree 2 i.e. n2