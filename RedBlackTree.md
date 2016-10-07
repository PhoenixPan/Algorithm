# Basic  
1. Insertion begins by adding the node as any binary search tree insertion does and by coloring it red  
2. The term uncle node will be used to refer to the sibling of a node's parent  


Read situation 1-5 and their *illustrations*    
https://en.wikipedia.org/wiki/Red%E2%80%93black_tree  
while watching this video    
https://www.youtube.com/watch?v=vDHFF4wjWYU  


# Java Implementation:
## Node
```

/**
 * RedBlackNode class.
 * 
 * @author Phoenix
 *
 */
public class RedBlackNode {
	private int color;
    private static final int RED   = 0;
    private static final int BLACK = 1;
    String key;
	String value;
	RedBlackNode parent;
	RedBlackNode leftChild;
	RedBlackNode rightChild;
	
	public RedBlackNode(String key, String value, int color, RedBlackNode p, RedBlackNode lc, RedBlackNode rc) {
		this.key = key;
		this.value = value;
		this.color = color;
		this.parent = p;
		this.leftChild = lc;
		this.rightChild = rc;
	}
	
	public int getColor() {
		return this.color;
	}
	
	public void setColor(int color) {
		this.color = color;
	}
	public String getKey() {
		return this.key;
	}
	
	public String getValue() {
		return this.value;
	}
	
	public void setValue(String value) {
		this.value = value;
	}
	
	public RedBlackNode getLc() {
		return this.leftChild;
	}
	
	public void setLc(RedBlackNode lc) {
		this.leftChild = lc;
	}
	
	public RedBlackNode getP() {
		return this.parent;
	}
	
	public void setP(RedBlackNode p) {
		this.parent = p;
	}
	
	public RedBlackNode getRc() {
		return this.rightChild;
	}
	
	public void setRc(RedBlackNode rc) {
		this.rightChild = rc;
	}
	
	public String toString() {
		String color = "Red";
		if (this.color == 1) 
			color = "Black";
		return key + ":" + value + "," + color + ", Parent: " + parent.getKey(); 
	}
	
}
```

## Tree
```
import java.math.BigInteger;

/**
 * RedBlackTree Class.
 * 
 * @author Phoenix
 *
 */
public class RedBlackTree {

	private int size = 0;
	private static final int RED = 0;
	private static final int BLACK = 1;
	private static RedBlackNode nil;
	private String lookupResult = null;
	private boolean contains = false;
	RedBlackNode root;

	public RedBlackTree() {
		nil = new RedBlackNode(null, null, BLACK, nil, nil, nil);
		root = nil;
		root.setP(nil);
		root.setLc(nil);
		root.setRc(nil);
	}

	/**
	 * Insert a new node into the tree.
	 * 
	 * @param value
	 */
	public void insert(String key, String value) {

		// Reset the value of the key if it's already exist.
		if (root != nil && contains(key, this)) {
			resetValue(key, value, root);
			return;
		}

		RedBlackNode z = new RedBlackNode(key, value, RED, nil, nil, nil);
		this.size++;
		RedBlackNode x = root;
		RedBlackNode y = nil;
		while (x != nil) {
			y = x;
			if (z.getKey().compareTo(x.getKey()) < 0)
				x = x.getLc();
			else
				x = x.getRc();
		}
		z.setP(y);

		if (y == nil)
			root = z;
		else if (z.getKey().compareTo(y.getKey()) < 0)
			y.setLc(z);
		else
			y.setRc(z);

		z.setLc(nil);
		z.setRc(nil);
		z.setColor(RED);
		this.RBInsertFixup(z);
	}

	public void RBInsertFixup(RedBlackNode z) {
		while (z.getP().getColor() == RED) {
			RedBlackNode grand = z.getP().getP();
			RedBlackNode parent = z.getP();
			RedBlackNode uncle = nil;
			if (parent == grand.getLc()) {
				uncle = grand.getRc();

				// If both parent and uncle are red, simply change them to
				// black.
				if (uncle.getColor() == RED) {
					parent.setColor(BLACK);
					uncle.setColor(BLACK);
					grand.setColor(RED);
					z = grand;
				} else {
					if (z == parent.getRc()) {
						z = parent;
						leftRotate(z);
					}
					z.getP().setColor(BLACK);
					z.getP().getP().setColor(RED);
					rightRotate(z.getP().getP());
				}
			} else {
				uncle = grand.getLc();
				// If both parent and uncle are red, simply change them to
				// black.
				if (uncle.getColor() == RED) {
					parent.setColor(BLACK);
					uncle.setColor(BLACK);
					grand.setColor(RED);
					z = grand;
				} else {
					if (z == parent.getLc()) {
						z = parent;
						rightRotate(z);
					}
					z.getP().setColor(BLACK);
					z.getP().getP().setColor(RED);
					leftRotate(z.getP().getP());
				}
			}
		}
		root.setColor(BLACK);
	}

	/**
	 * A method to do left rotation.
	 * 
	 * @param x
	 *            The node based on which the rotation is executed.
	 */
	public void leftRotate(RedBlackNode x) {

		RedBlackNode y = x.getRc();
		x.setRc(y.getLc());
		y.getLc().setP(x);
		y.setP(x.getP());

		if (x.getP() == nil) {
			root = y;
		} else {
			if (x.getP().getLc() == x) {
				x.getP().setLc(y);
			} else {
				x.getP().setRc(y);
			}
		}
		y.setLc(x);
		x.setP(y);
	}

	/**
	 * A method to do right rotation.
	 * 
	 * @param x
	 *            The node based on which the rotation is executed.
	 */
	public void rightRotate(RedBlackNode x) {

		RedBlackNode y = x.getLc();
		x.setLc(y.getRc());
		y.getRc().setP(x);
		y.setP(x.getP());

		if (x.getP() == nil) {
			root = y;
		} else {
			if (x.getP().getLc() == x) {
				x.getP().setLc(y);
			} else {
				x.getP().setRc(y);
			}
		}
		y.setRc(x);
		x.setP(y);
	}

	/**
	 * Traverse an entire tree.
	 */
	public void inOrderTraversal() {
		inOrderTraversal(this.root);
	}

	/**
	 * Traverse the tree from a node.
	 * 
	 * @param node
	 */
	public void inOrderTraversal(RedBlackNode node) {
		if (node.getLc() != nil)
			inOrderTraversal(node.getLc());
		System.out.println(node);
		if (node.getRc() != nil)
			inOrderTraversal(node.getRc());
	}

	/**
	 * A method to verify the existence of a key.
	 * 
	 * @param key
	 *            The key given.
	 * @param tree
	 *            Node to start.
	 * @return true if the key exists, otherwise false.
	 */
	public boolean contains(String key, RedBlackTree tree) {
		contains = false;
		ifContain(key, tree.root);
		return contains;
	}

	/**
	 * An internal method to verify the existence of a key.
	 * 
	 * @param key
	 *            The key given.
	 * @param node
	 *            Node to start.
	 */
	private void ifContain(String key, RedBlackNode node) {
		if (key.equals(node.getKey())) {
			contains = true;
		} else if (node.getLc() != nil) {
			if (key.compareTo(node.getKey()) < 0)
				ifContain(key, node.getLc());
			else
				ifContain(key, node.getRc());
		}
	}

	/**
	 * Clean up the previous result and try to find the value of the given key.
	 * 
	 * @param key
	 *            The key given.
	 * @param node
	 *            Node to start.
	 * @return The value of the key or null.
	 */
	public String lookup(String key, RedBlackTree tree) {
		lookupResult = null;
		findValue(key, tree.root);
		return lookupResult;
	}

	/**
	 * An internal method to find the value of the key (if exist).
	 * 
	 * @param key
	 *            The key given.
	 * @param node
	 *            Node to start.
	 * @return The value of the key or null.
	 */
	private void findValue(String key, RedBlackNode node) {
		if (key.equals(node.getKey())) {
			lookupResult = node.getValue();
		} else if (node.getLc() != nil) {
			if (key.compareTo(node.getKey()) < 0)
				findValue(key, node.getLc());
			else
				findValue(key, node.getRc());
		}
	}

	/**
	 * Reset the value of the key if it's already exist.
	 * 
	 * @param key
	 *            The key given.
	 * @param value
	 *            The value to be set.
	 * @param node
	 *            Root node of the tree.
	 */
	private void resetValue(String key, String value, RedBlackNode node) {
		if (key.equals(node.getKey())) {
			node.setValue(value);
		} else if (node.getLc() != nil) {
			if (key.compareTo(node.getKey()) < 0)
				resetValue(key, value, node.getLc());
			else
				resetValue(key, value, node.getRc());
		}
	}
}
```
## Test main function
```
public class Main {

	public static void main(String[] args) {
		RedBlackTree tree = new RedBlackTree();
		tree.insert("A", "10");
		tree.insert("B", "85");
		tree.insert("C", "15");
		tree.insert("D", "70");
		tree.insert("E", "20");
		tree.insert("E", "30");  // Reset the value of key "E"
		
		System.out.println("Stert to traverse the tree:");
		tree.inOrderTraversal(tree.root);
		
		System.out.println("\nStert to look up keys:");
		System.out.println("Value found from key \"E\":" + tree.lookup("E", tree));
		System.out.println("Value found from key \"R\":" + tree.lookup("R", tree));

	}
}

```

