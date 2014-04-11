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
