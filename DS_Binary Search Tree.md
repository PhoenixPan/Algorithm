# BST basic

1. Difference between binary tree and binary search tree
	1. In binary tree each node has two leaves
	2. In BST, left child contains only nodes with values less than the parent's and right child contains only nodes with vlaues larger than the parent's.  

2. Degree: 
	1. The **degree** of a tree is the maximum degree of a node in the tree. For example, a binary tree has a degree of exactly 2.  
	2. The degree of anode is the number of subtrees of it. In a binary tree, all nodes have degree 0, 1, or 2.  
	
3. Full binary tree: each node is either a leaf or has degree exactly 2.  
4. A complete k-ary tree is a k-ary tree in which all leaves have the same depth and all internal nodes have degree k.  
5. Remove from a binary tree: replace with the smallest node from its right subtree or the largest node from its left subtree.    
6. In a complete tree:  
Number of leafs: (n+1) /2  
Number of internal nodes: N – above = (n-1)/2  

## Heap
1. A heap is a complete binary tree, where each node is greater than or equal to its children.
2. Reheapification upward: add new node by swapping with parent.
3. Reheapification downward: delete the top node by replacing it with the last node and then swap that node with its smaller (min heap) or larger (max heap) children.
