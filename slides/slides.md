# CMPT231
## Lecture 13: ch34
### Tractability, Semester Review

---
## Devotional

---
## Outline for today
+ All-Pairs Shortest Path
  + Johnson's 
+ Tractability
+ Semester Review

---
## All-pairs shortest path
+ **Bellman-Ford** on each vertex: \`O(|V|^4)\`
+ Dyn prog by **path length**: \`O(|V|^4)\`
+ **Floyd-Warshall** (by *subset* of *V*): \`O(|V|^3)\`
+ **Johnson** (*Dijkstra* on each vertex): \`O(|V|^2 log |V| + |V| |E|)\`
  + **Reweight** edges for positive weights

---
## Johnson's reweighting
+ Johnson's trick: Add a **new** vertex *s*
  + Add **zero-weight** edges from *s* to all vertices:
    + *V'* = *V* &cup; {*s*},
    + *E'* = *E* &cup; {*(s,v)*: *v* &in; *V*},
    + *w(s,v)* = 0 &forall; *v*
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

---
## Outline
