# Graph basic
#####Graphs:  
undirected graphs  
directed graphs   
#####Graph Traversals:  
depth-first (recursive vs. stack)  
breadth-first (queue)  

Simple Graphs: have no loops and no multiple edges


Depth-first traversal (stack): visit next neighbor only after traversing all possible edges from the current neighbor and also its neighbors, until the end.  
Breadth-first traversal (queue): Visit all neighbors before visiting any of their neighbors (order does not matter).

# Find the shortest path  
The shortest (minimal) path has the lowest cost, not the fewest edges. There could be more than one shortest path.
## Two Greedy Algorithms
Dijkstra’s Algorithm  
Prim’s Algorithm  

## One Dynamic Programming 
Floyd Warshall  
