# Preface
This is a school project that has special requirements, causing low efficiency and poor implementation.

# Node 
```
/**
 * Class for nodes in the two D tree.
 * @author Phoenix
 *
 */
public class TwoDTreeNode {

	int depth = -1;
	int axis = -1; // 0 represents dividing by X (left and right), 1 represents dividing by Y (up and down).
	double[] indexes = new double[2]; // [x, y]
	TwoDTreeNode parent = null;
	TwoDTreeNode left = null;
	TwoDTreeNode right = null;
	String time;
	String street;
	String offense;
	String date;
	String tract;
	String latitude;
	String longtitude;

	public TwoDTreeNode(double x, double y, String time, String street, String offense, String date, String tract,
			String latitude, String longtitude) {

		indexes[0] = x;
		indexes[1] = y;
		this.time = time;
		this.street = street;
		this.offense = offense;
		this.date = date;
		this.tract = tract;
		this.latitude = latitude;
		this.longtitude = longtitude;
	}

	/**
	 * A recursive method to add a node in the tree.
	 * 
	 * precondition: the index values of the node are not null.
	 * 
	 * postcondition: the new node will be added to an appropriate position.
	 * 
	 * 
	 * @param node The node to be added.
	 */
	public void addNode(TwoDTreeNode node) {
		/**
		 * If this.axis == 0, we are comparing the values of x;
		 * If this.axis == 1, we are comparing the values of y;
		 * If node[x] < this[x] or node[y] < this[y], 
		 * we pass the node to the left, otherwise right
		 * 
		 */
		if (node.indexes[this.axis] < this.indexes[this.axis]) {
			// If the left side is null, make the new node left
			if (this.left == null) {
				this.left = node;
				node.parent = this;
				node.depth = node.parent.depth + 1;
				node.axis = node.depth % 2;
			// If the left side is not null, keep passing
			} else {
				this.left.addNode(node);
			}

		}
		else {
			// If the right side is null, make the new node right
			if (this.right == null) {
				this.right = node;
				node.parent = this;
				node.depth = node.parent.depth + 1;
				node.axis = node.depth % 2;
			// If the right side is not null, keep passing
			} else {
				this.right.addNode(node);
			}
		}

	}
	
	/**
	 * Get the distance between a node and a point.
	 * 
	 * precondition: the incoming node is not null.
	 * 
	 * postcondition: the distance between the invoking node and incoming node will be known.
	 * 
	 * @param coordinates of a point.
	 * @return distance
	 */
	public double getDistance(double x, double y) {
		double distance = 0;
		double xpow = Math.pow(Math.abs(x) - Math.abs(this.indexes[0]),2);
		double ypow = Math.pow(Math.abs(y) - Math.abs(this.indexes[1]),2);
		distance = Math.sqrt(xpow + ypow);
		return distance;
	}

	@Override
	public String toString() {
		String coordinates = indexes[0] + "," + indexes[1] + "," + time + ",\n" + 
							street + "," + offense + "," + date + "," + tract + ",\n" + 
							latitude + "," + longtitude;
		return coordinates;
	}

}
```

# Tree
```
public class TwoDTree {

	TwoDTreeNode root = null; // Root of the tree
	int nodeSearched = 0;     // Trace the number of nodes scanned
	double nearestDistance = -1;
	TwoDTreeNode nearestNode = null;

	/**
	 * A constructor that generates the tree.
	 * 
	 * pre-condition: The String crimeDataLocation contains the path to a file
	 * formatted in the exact same way as CrimeLatLonXY.csv P
	 * 
	 * post-condition: The 2d tree is constructed and may be printed or queried.
	 * 
	 * @param crimeDataLocation
	 *            path to a file
	 */
	public TwoDTree(String crimeDataLocation) {

		int count = 0;
		ArrayList<TwoDTreeNode> allNodes = new ArrayList<>();
		File sourceData = new File(crimeDataLocation);

		try (BufferedReader input = new BufferedReader(new InputStreamReader(new FileInputStream(sourceData)))) {

			// Skip the first line for table head.
			String rawLine = input.readLine();
			while ((rawLine = input.readLine()) != null) {

				// Extract data for a new node and add it to the list.
				String[] elements = rawLine.split(",");
				allNodes.add(new TwoDTreeNode(Double.parseDouble(elements[0]), Double.parseDouble(elements[1]),
						elements[2], elements[3], elements[4], elements[5], elements[6], elements[7], elements[8]));
				
				// If it's the first node, make it the root.
				if (count == 0) {
					root = allNodes.get(count);
					root.depth = 0;
					root.axis = 0;
				} else {
					TwoDTreeNode temp = allNodes.get(count);
					root.addNode(temp);
				}
				count++;
			}
		} catch (IOException io) {
			System.out.println(io.getMessage());
			System.exit(1);
		}
		System.out.println("Crime file loaded into 2d tree with " + count + " records.");

	}

	/**
	 * A overridden method to traverse the tree. pre-condition: The 2d tree has
	 * been constructed.
	 * 
	 * post-condition: The 2d tree is displayed with a pre-order traversal.
	 * 
	 */
	public void preOrderPrint() {
		System.out.println(this.root);
		if (root.left != null)
			preOrderPrint(root.left);
		if (root.right != null)
			preOrderPrint(root.right);
	}

	/**
	 * The overrider method to help traverse the tree. pre-condition: The 2d
	 * tree has been constructed.
	 * 
	 * post-condition: The next node will be printed and the cycle continues.
	 * 
	 * @param node
	 *            parent node used to print and trace the next node.
	 */
	private void preOrderPrint(TwoDTreeNode node) {
		System.out.println(node);
		if (node.left != null)
			preOrderPrint(node.left);
		if (node.right != null)
			preOrderPrint(node.right);
	}

	/**
	 * A overridden method to traverse the tree. pre-condition: The 2d tree has
	 * been constructed.
	 * 
	 * post-condition: The 2d tree is displayed with an in-order traversal.
	 * 
	 */
	public void inOrderPrint() {
		if (root.left != null)
			inOrderPrint(root.left);
		System.out.println(root);
		if (root.right != null)
			inOrderPrint(root.right);
	}

	/**
	 * The overrider method to help traverse the tree. pre-condition: The 2d
	 * tree has been constructed.
	 * 
	 * post-condition: The next node will be printed and the cycle continues.
	 * 
	 * @param node
	 *            parent node used to print and trace the next node.
	 */
	private void inOrderPrint(TwoDTreeNode node) {
		if (node.left != null)
			inOrderPrint(node.left);
		System.out.println(node);
		if (node.right != null)
			inOrderPrint(node.right);
	}

	/**
	 * A overridden method to traverse the tree. pre-condition: The 2d tree has
	 * been constructed.
	 * 
	 * post-condition: The 2d tree is displayed with a post-order traversal.
	 * 
	 */
	public void postOrderPrint() {
		if (root.left != null)
			postOrderPrint(root.left);
		if (root.right != null)
			postOrderPrint(root.right);
		System.out.print(root);
	}

	/**
	 * The overrider method to help traverse the tree. pre-condition: The 2d
	 * tree has been constructed.
	 * 
	 * post-condition: The next node will be printed and the cycle continues.
	 * 
	 * @param node
	 *            parent node used to print and trace the next node.
	 */
	private void postOrderPrint(TwoDTreeNode node) {
		if (node.left != null)
			inOrderPrint(node.left);
		if (node.right != null)
			inOrderPrint(node.right);
		System.out.println(node);
	}

	/**
	 * pre-condition: The 2d tree has been constructed.
	 * 
	 * post-condition: The 2d tree is displayed with a level-order traversal.
	 * Note: the level order traversal is not recursive. It uses a queue that
	 * you must write. This queue is defined in a separate file named
	 * Queue.java. Queue.java is built with an array based queue – using front
	 * and rear pointers. You will write the methods to add to the rear and
	 * remove from the front of the queue. You will also design the signatures
	 * for these methods. An example appears on the slides and will be discussed
	 * in class.
	 * 
	 */
	public void levelOrderPrint() {
		putInQueueAndPrint(root);
		System.out.println();
	}

	private void putInQueueAndPrint(TwoDTreeNode root) {
		Queue queue = new Queue();
		queue.add(root);
		while (queue.nodeCount != 0) {
			TwoDTreeNode node = queue.poll();
			System.out.println(node);
			if (node.left != null)
				queue.add(node.left);
			if (node.right != null)
				queue.add(node.right);
		}
	}

	/**
	 * pre-condition: The 2d tree has been constructed
	 *
	 * post-condition: A list of 0 or more crimes is returned. These crimes
	 * occurred within the rectangular range specified by the four parameters.
	 * The pair (x1, y1) is the left bottom of the rectangle. The pair (x2, y2)
	 * is the top right of the rectangle.
	 *
	 * @param x1 
	 * @param y1 
	 * @param x2 
	 * @param y2 
	 * @return a list of crimes in this rectangular
	 */
	public ListOfCrimes findPointsInRange(double x1, double y1, double x2, double y2) {
		ListOfCrimes crimes = new ListOfCrimes();
		findPointsInRange(root, crimes, x1, y1, x2, y2);
		return crimes;
	}
	
	public void findPointsInRange(TwoDTreeNode node, ListOfCrimes crimes, double x1, double y1, double x2, double y2) {
		nodeSearched++;
		double x = node.indexes[0];
		double y = node.indexes[1];
		
		// If in range, add to the list.
		if (x1 <= x && x <= x2 && y1 <= y && y <= y2) {
			crimes.add(node);
		}	
		
		// Otherwise, move on with given conditions.
		// If the node is on even level.
		if (node.axis == 0) {
			if (x > x2) {
				if (node.left != null)
					findPointsInRange(node.left, crimes, x1, y1, x2, y2);
			}
			else if (x < x1) {
				if (node.right != null)
					findPointsInRange(node.right, crimes, x1, y1, x2, y2);
			}
			else {
				if (node.left != null)
					findPointsInRange(node.left, crimes, x1, y1, x2, y2);
				if (node.right != null)
					findPointsInRange(node.right, crimes, x1, y1, x2, y2);
			}
		}
		// If the node is on odd level.
		else if (node.axis == 1) {
			if (y > y2) {
				if (node.left != null)
					findPointsInRange(node.left, crimes, x1, y1, x2, y2);
			}
			else if (y < y1) {
				if (node.right != null)
					findPointsInRange(node.right, crimes, x1, y1, x2, y2);
			}
			else {
				if (node.left != null)
					findPointsInRange(node.left, crimes, x1, y1, x2, y2);
				if (node.right != null)
					findPointsInRange(node.right, crimes, x1, y1, x2, y2);
			}
		}	
	}

	/**
	 * pre-condition: the 2d tree has been constructed. The (x1,y1) pair
	 * represents a point in space near Pittsburgh and in the state plane
	 * coordinate system. The parameter “nearest” holds a reference to the
	 * result node (with no data)
	 *
	 * post-condition: the distance in feet to the nearest node is returned. The
	 * reference parameter now has the nearest neighbor's data
	 *
	 * @param x1 targetX x coordinate of target.
	 * @param y1 targetY y coordinate of target.
	 * @param nearest root node passed in.
	 * @return nearest distance nearest distance to the target node.
	 */
	public double nearestNeighbor(double x1, double y1, TwoDTreeNode nearest) {
		findNeighbor(x1, y1, root);
		System.out.println("Looked at " + nodeSearched + " nodes in tree. Found the nearest crime at:");
		nodeSearched = 0;
		nearest = nearestNode;
		return nearestDistance;
	}
	
	/**
	 * Recursive function to trace the nearest point.
	 * @param targetX x coordinate of target.
	 * @param targetY y coordinate of target.
	 * @param node the parent node passed in.
	 */
	public void findNeighbor(double targetX, double targetY, TwoDTreeNode node) {

		nodeSearched++;
		double nodeX = node.indexes[0];
		double nodeY = node.indexes[1];
		
		// Get distance from the node to the target point and update if necessary
		double distanceToTarget = node.getDistance(targetX, targetY);
		if (nearestDistance == -1 || nearestDistance > distanceToTarget) {
			nearestDistance = distanceToTarget;
			nearestNode = node;
		}
		
		// Get distance from the target point to the node axis
		double distanceToX = Math.abs(nodeX - targetX);
		double distanceToY = Math.abs(nodeY - targetY);
		
		// If the node is dividing by X.
		if (node.axis == 0) {
			if (distanceToX <= nearestDistance && Math.abs(nodeY - targetY) <= nearestDistance) {
				if (node.left != null) 					
					findNeighbor(targetX, targetY, node.left);
				if (node.right != null) 					
					findNeighbor(targetX, targetY, node.right);
			}
			else {
				if (nodeX > targetX && node.left != null) {
					findNeighbor(targetX, targetY, node.left);
				}
				else if (nodeX <= targetX && node.right != null) {
					findNeighbor(targetX, targetY, node.right);
				}
			}
		} 
		// If the node is dividing by Y.
		else {
			if (distanceToY <= nearestDistance && Math.abs(nodeX - targetX) <= nearestDistance){ //  && Math.abs(nodeX - targetX) <= nearestDistance   40
				if (node.left != null) 					
					findNeighbor(targetX, targetY, node.left);
				if (node.right != null) 					
					findNeighbor(targetX, targetY, node.right);
			}
			else {   //&& Math.abs(nodeX - targetX) <= nearestDistance   not working alone
				if (nodeY <= targetY && node.right != null) {
					findNeighbor(targetX, targetY, node.right);
				}
				else if (nodeY > targetY && node.left != null) {
					findNeighbor(targetX, targetY, node.left);
				}
			}
			
		}
		
	}
		
}
```
