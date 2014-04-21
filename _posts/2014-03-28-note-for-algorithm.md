---
layout: post
---

[Book Reference](http://algs4.cs.princeton.edu/)

### Graph

#### Undirected Graphs

##### Depth-First Search

##### Breadth-First Search

#### Directed Graphs
+ Topological sort

  Topological sort is used for resolving precedence-constrained scheduling problem.

    + Only a **DAG** could be applied for this sorting.
    + Compute DFS on the Digraph, the reverse postorder is the topological sort.

+ Strong-connected component

  Kasaraju-Sharir algorithm

  + Phase 1.

    Compute reverse postorder in G(R) using DFS. G(R) is the reverse graph of G. *Put vertices visited into a stack, then result is a reversed postorder.*

  + Phase 2.

    Create a array with size *V* to store the component ID for a vertex. Visiting all the vertices according to the sequence of the result from *Phase 1* using DFS, give the vertex an component ID. component ID will be added by 1 when the DFS visiting finished.

#### Minimum spanning tree

+ Kurska's algorithm 
  1. Put all the edges into a priority queue by the sort of the ascending weight.
  2. Add next edge to tree *T* unless doing so would create a cycle. *Using union-find for judging the cycle.*

+ Prim's algorithm

  + Lazy implement
    1. Start from a vertex, put this vertex into the tree *T*.
    2. Add all the edges that incident to the vertex which just be added into the tree *T* into a priority queue sorted by ascending weight, not including the edges that both endpoints are already in the tree *T*.
    3. Find the shortest edges and add the vertex that not in the tree *T* into the *T*.
    4. Repeat until *V* - 1 edges found.

  + Eager implement
    1. Start from a vertex, pub this vertex into the tree *T*.
    2. Add all the edges that incident to the vertex which just be added into the tree *T*, replace the edges from the same source endpoint when the weight is smaller than it was. (This is the major difference between the lazy and eager implement, and can use smaller space than lazy implement)
    3. Find the shortest edges and add the vertex that not in the tree *T* into the *T*.
    4. Repeat until *V* - 1 edges found.

#### Shortest Path

  + Dijkstra algorithm

    Compute all the path from source vertex *s* to any other vertices. Cannot resolve the graph has **negative edges**.

    1. Preparing
      1. Prepare a distTo\[v\], this storing the shortest path weight from vertex *s* to vertex *v*. Initialize all the items with the MAX value.
      2. Prepare a edgeTo\[v\], this storing the shortest path from vertex *s* to vertex *v*.
      3. Prepare a priority queue instant pq\[v\], this storing the shortest path weight from vertex *s* to vertex *v*, the key is *v*, and the value is the path weight. This priority queue sorted by the ascending weight.
    2. start from the source vertex *s*. Add the *s* into pq\[v\] with value 0.0.
    3. Pick the most smallest one from pq\[\] as vertex v.
    4. Iterate each edge that source from vertex v. If the edge e that from v to w has a smaller weight thant distTo\[w\], then replace the distTo\[w\] with the value *distTo\[v\] + e.weight*. , replace edgeTo\[w\] with e, add or replace an item in pq\[\] that with the key is w and the value is *distTo\[v\] + e.weight*. We call this step as name of "Relex"
    5. Repeat until pq\[\] is empty.

  + Shortest path in DAG

    Compute all the path from the source vertex *s* to any other vertices.

    1. Visiting the DAG with the topological order.
    2. "Relex" the all the edges that source from the vertex we are visiting.
    3. Repeat until all the vertices are visitied in the topological order.
  
  + Bellman-For algorithm
  
    Compute all the path from the source vertex *s* to any other vertices.
  
    1. Initialize distTo\[s\] = 0 and distTo\[v\] = INFINITY for all other vertices.
    2. Visit all the vertices sequently and "Relex" all the edges that source from the vertex being visiting.
    3. Repeat *V* times for the step 2.

```java
    for (int i=0; i < G.V(); i++) 
        for (int v=0; v < G.V(); v++)
            for (DirectedEdge e : G.adj(v))
                relax(e);
```