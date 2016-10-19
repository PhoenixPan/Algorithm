# B-Tree
Characteristics:  
1.	Root may have 0 or 1 element to MAX, other nodes have at least MIN elements  
2.	MAX = 2MIN  
3.	Elements in a node are stored in a sorted array, from the smallest to the largest  
4.	Number of subtrees = number of elements in parent + 1  
5.	Every leaf has the same depth  

Operations:
Add a new node: ascend the middle node to its parent when reach maximum limit of a node    
Remove from a B-Tree: replace with the smallest node from its right subtree or the largest node from its left subtree. Merge siblings and rotate if necessary, making sure each node has minimum number of elements  

Number of values contained: maximum * (1 - (maximum + 1) ^ (height + 1)) / 1 - (maximum + 1)
Example: a B tree with minimum of 10, height of 2 can store 9260 values  

