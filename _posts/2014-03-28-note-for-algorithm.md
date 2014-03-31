### Graph
* Undirected Graphs
  * Depth-First Search
  * Breadth-First Search
* Directed Graphs
  * Toplogical sort
    * Only a **DAG** could be sorted.
    * Compute DFS on the Digraph, the reverse postorder is the toplogical sort.

  * Strong-connected component
    * Kasaraju-Sharir algorithm 
      * Phase 1. Compute reverse postorder in G(R). G(R) means the reverse graph. *Put vertices visited into a stack, then result is a reversed postorder.*
      * Phase 2. Run DFS in G, visiting unmarded vertices in reverse postorder of G(R).

