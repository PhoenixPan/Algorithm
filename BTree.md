# B-Tree
Characteristics:  
1.	Root may have 0 or 1 element to MAX, other nodes have at least MIN elements  
2.	Space in one node: maximum = 2 * minimum  
3.	Elements in a node are stored in a sorted array, from the smallest to the largest  
4.	Number of subtrees = number of elements in parent + 1  
5.	Every leaf has the same depth  

Operations:
Add a new node: ascend the middle node to its parent when reach maximum limit of a node    
Remove from a B-Tree: replace with the smallest node from its right subtree or the largest node from its left subtree. Merge siblings and rotate if necessary, making sure each node has minimum number of elements  

Number of values contained: maximum * (1 - (maximum + 1) ^ (height + 1)) / 1 - (maximum + 1)
Example: a B tree with minimum of 10, height of two can store 9260 values   


# B+ Tree
When split a leaf, we __copy__ the middle key up:  
![image](https://cloud.githubusercontent.com/assets/14355257/19502056/31f921cc-9578-11e6-96c9-f1560ace8da5.png)  
When split an internal node or root, we __move__ the middle key up:  
![image](https://cloud.githubusercontent.com/assets/14355257/19502057/350156aa-9578-11e6-8679-3492f8b3e804.png)  
Because there’s no need to redundantly store the association for internal nodes.  

Adding is the same as B tree except for the leaf node, where you need to leave a copy behind.

Removing from a B+ tree:  
![image](https://cloud.githubusercontent.com/assets/14355257/19502064/384746ee-9578-11e6-857a-d1272e9e35aa.png)
-> remove 70 ->
![image](https://cloud.githubusercontent.com/assets/14355257/19502066/3aa3f248-9578-11e6-855e-85524c68bc50.png)
-> remove 80 ->
![image](https://cloud.githubusercontent.com/assets/14355257/19502068/3cd1815c-9578-11e6-91df-4efbf03875be.png)
