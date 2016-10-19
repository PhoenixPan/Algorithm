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
![image](https://cloud.githubusercontent.com/assets/14355257/19501766/1b4a55ba-9576-11e6-927b-9c02717798bc.png)

![image](https://cloud.githubusercontent.com/assets/14355257/19501774/1e6a7e00-9576-11e6-8a8e-a062bf77fbbc.png)

![image](https://cloud.githubusercontent.com/assets/14355257/19501777/218fbb4a-9576-11e6-9c91-c422a2bafdb5.png)

## Greedy Algorithms: Prim’s Algorithm  
![image](https://cloud.githubusercontent.com/assets/14355257/19501812/64cfeb64-9576-11e6-9760-328aa6fb4fb1.png)

## One Dynamic Programming: Floyd Warshall  
![image](https://cloud.githubusercontent.com/assets/14355257/19501786/35276d4c-9576-11e6-873e-22655f5c723d.png)

![image](https://cloud.githubusercontent.com/assets/14355257/19501799/4ae8d292-9576-11e6-8719-4b7481b67b91.png)

![image](https://cloud.githubusercontent.com/assets/14355257/19501801/4ecc6b1c-9576-11e6-8f09-139174a6909c.png)

![image](https://cloud.githubusercontent.com/assets/14355257/19501808/568fbcbe-9576-11e6-9be2-8d7ba2ef437c.png)
