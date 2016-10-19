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
## Greedy Algorithms: Dijkstra’s Algorithm  
Not only record the shortest distance, but also the path by storing the previous node.   
![image](https://cloud.githubusercontent.com/assets/14355257/19501766/1b4a55ba-9576-11e6-927b-9c02717798bc.png)  
![image](https://cloud.githubusercontent.com/assets/14355257/19501774/1e6a7e00-9576-11e6-8a8e-a062bf77fbbc.png)  
![image](https://cloud.githubusercontent.com/assets/14355257/19501777/218fbb4a-9576-11e6-9c91-c422a2bafdb5.png)  

## Greedy Algorithms: Prim’s Algorithm  
Sudo code:
```
MST-Prim(G,w,r)
  Q = V[G]
  foreach u in Q
      do: key[u] = ? // initialize to ‘infinity’
  key[r] = 0
  pi[r] = null
  while Q is not empty
      do: u = ExtractMin(Q)  
            // find light edge; u = r first time through
            foreach v in Adj[u]
                 do: if v in Q && w(u,v) < key[v] 
                          // update adjacent nodes
                          then pi[v] = u
                                  key[v] = w(u,v)
```
![image](https://cloud.githubusercontent.com/assets/14355257/19501812/64cfeb64-9576-11e6-9760-328aa6fb4fb1.png)    
  
## One Dynamic Programming: Floyd Warshall  

Sudo code:  
```
for each vertex u in G do
   for each vertex v in G do
      cost[u,v] = c[u,v]  
for each vertex w in G do  
    for each vertex u in G do
       for each vertex v in G do
         cost[u,v] = min(cost[u,v],
                   cost[u,w] + cost[w,v]
```

Reference video: https://www.youtube.com/watch?v=4OQeCuLYj-4  

![image](https://cloud.githubusercontent.com/assets/14355257/19501786/35276d4c-9576-11e6-873e-22655f5c723d.png)  
![image](https://cloud.githubusercontent.com/assets/14355257/19501799/4ae8d292-9576-11e6-8719-4b7481b67b91.png)  
![image](https://cloud.githubusercontent.com/assets/14355257/19501801/4ecc6b1c-9576-11e6-8f09-139174a6909c.png)  
![image](https://cloud.githubusercontent.com/assets/14355257/19501808/568fbcbe-9576-11e6-9be2-8d7ba2ef437c.png)  
