# CMPT231
## Lecture 13: ch34
### Tractability, Semester Review

---
## Devotional

---
## Outline for today
+ All-pairs shortest path
  + Johnson (reweighted Dijkstra): \`O(|V|^2 log |V| + |V| |E|)\`
+ Tractability
  + Complexity classes: *P*, *EXP*, *R*
  + Non-deterministic verification: *NP*
  + Reductions
  + NP-hard and NP-complete
+ Semester review

---
## All-pairs shortest path
+ **Bellman-Ford** on each vertex: \`O(|V|^4)\`
+ Dyn prog by **path length**: \`O(|V|^3 log |V|)\`
+ **Floyd-Warshall** (by *subset* of *V*): \`O(|V|^3)\`
+ **Johnson** (*Dijkstra* on each vertex): \`O(|V|^2 log |V| + |V| |E|)\`
  + **Reweight** edges for positive weights

---
## Johnson's reweighting
+ Johnson's trick: Add a **new** vertex *s*
  + Add **zero-weight** edges from *s* to all vertices:
    + *V'* = *V* &cup; {*s*},
    + *E'* = *E* &cup; {*(s,v)*: *v* &in; *V*}, and
      *w(s,v)* = 0 &forall; *v*
  + **Compute** *&delta;(s,v)* &forall; *v* (e.g., with Bellman-Ford)
  + **Reweight** edges using *h(v)* = *&delta;(s,v)*

![Johnson reweight, Fig-25-6(ab)](static/img/Fig-25-6ab.png)

---
## Johnson's reweighting
+ Why does this eliminate **negative weights**?
  + **Triangle inequality**: *&delta;(s,v)* &le; *&delta;(s,u)* + *w(u,v)*
  + Substitute **defn** of *h*: *h(v)* &le; *h(u)* + *w(u,v)*
  + Apply **reweighting**: *w'(u,v)* = *w(u,v)* + *h(u)* - *h(v)* &ge; 0

![Johnson reweight, Fig-25-6(ab)](static/img/Fig-25-6ab.png)

---
## Johnson all-pairs
```
def Johnson( G, w ):
  create G' = (V', E') with new vertex s
  BellmanFord( G', w, s )                   # find dlt[ s, v ]: O(VE)
    if returned FALSE, quit                 # net-neg cycle
  w'[ u, v ] = w[ u, v ] + dlt[ s, u ] - dlt( s, v ]
  for u in V:
    Dijkstra( G, w', u )                    # find dlt'[ u, v ]: O(VlgV + E)
    for v in V:
      d[ u, v ] = dlt'[ u, v ] - dlt[ s, u ] + dlt[ s, v ]
  return d
```

+ Innermost loop **converts** back to original weighting
+ **Complexity**: \`O(|V|^2 log |V| + |V||E|)\`

---
## All-pairs shortest path: summary
+ **Negative** edges allowed (but not net-negative *cycles*)
+ **Floyd-Warshall**: \`O(|V|^3)\`
  + Dyn prog by **subset** of vertices for intermediate nodes
+ **Johnson**: \`O(|V|^2 log |V| + |V||E|)\`
  + **Bellman Ford** from new source *s*
  + **Reweight**: *h(v)* = *&delta;(s,v)*
    and *w'(u,v)* = *w(u,v)* + *h(u)* - *h(v)*
  + Run **Dijkstra** from each vertex

---
## Outline

---
## Decision problems
+ Phrase problem as yes/no **decision**:
  + **input** is a string *s*,
  + **Problem** is a set of strings *X*,
  + **Algorithm** *A* returns boolean: \`A(s) = TRUE iff s in X\`
+ **Complexity** of *A* in terms of **length** *n* of *s*
+ e.g., given graph *(V, E, w)*, is there a **path** of weight &le; 4
  between nodes *u* and *v*?
  + **Input** *s* includes entire *graph* spec, "*4*", *u*, and *v*
  + Floyd-Warshall: \`O(|V|^3) = O(n^3)\`
+ e.g., given an integer *j*, is it **prime**?
  + Writing *j* out, **length** of input is \`n=log j\`
  + Lenstra-Pomerance [AKS](https://en.wikipedia.org/wiki/AKS_primality_test)
    algorithm (2011): \`O(n^6 log^k n)\`
    + roughly **polynomial** in length of input

---
## Complexity classes
+ *P*: decision problems for which **polynomial-time** algorithms exist
  + Most of the algorithms in this course! \`O(n^c)\`
+ *EXP*: problems solvable in **exponential** time: \`O(2^(n^c))\`
+ *R*: problems solvable in **finite** time
  + R = "**recursive**" (Turing 1936, Church 1941)

![P, EXP, and R](static/img/P-EXP-R.svg)

---
## Examples
+ Does a weighted graph have a **net-negative cycle**?
  + In **P**: *Bellman-Ford* \`O(|V||E|) = O(n^2)\`
+ Given a **chess** board configuration (n x n), can White **win**?
  + In **EXP** but not in **P**
+ Given a **Tetris** board and sequence of pieces, can you **survive**?
  + In **EXP** but not *known* whether in **P**
+ Given a computer **program**, does it ever **halt**?
  + [Uncomputable](https://en.wikipedia.org/wiki/Halting_problem)! (&notin; *R*)
  + No algorithm can solve this in *finite* time for **all** programs, for **all** inputs

---
## Turing machine
+ Computational model

---
## NP
+ Verification algorithm
+ Nondeterministic Turing machine

![NP](static/img/NP-complete.svg)

---
## Reductions

---
## NP-hard and NP-complete

![NP complete](static/img/NP-complete.svg)

---
## P =? NP

---
## For further reading
+ [MIT 6.890: Fun with Hardness Proofs](http://courses.csail.mit.edu/6.890/fall14/)

---
## Outline

---
## Semester overview
+ **Complexity**, &Theta; O &Omega; o &omega;, recurrence, induction
  *(ch1-5)*
+ **Sorting** *(ch4-8)*
  + Comparison sorts: insert, merge, heap, quick
  + Linear sorts: counting, radix, bucket
+ **Data Structures** *(ch10-12,18)*
  + Hash tables, linked lists, BSTs, B-trees
+ **Divide and Conquer** *(ch15-16)*
  + Dynamic prog: rod cut, matrix-chain, LCS, opt BST
  + Greedy: act sel, list merge, Huffman, frac knapsack
+ **Graph Algorithms** *(ch22-25)*
  + Spanning trees: Kruskal, Prim
  + Single-source shortest paths: Bellman-Ford, Dijkstra
  + All-pairs shortest paths: Floyd-Warshall, Johnson

---
## Outline

---
