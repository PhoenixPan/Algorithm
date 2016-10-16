# Merkle–Hellman knapsack cryptosystem

First, create a superincreasing sequence, w:  
```
w = {2, 7, 11, 21, 42, 89, 180, 354}
```
Second, get the sum of the sequence and find a q, which should be larger than the sum. 
```
wSum = 706
q = 881
```
Also find a r, which is between (1, q) and coprime to q:     
```
r = 588
```
Now we have the private key, which consists of w, q, r. To generate the public key β, manipulate each element in w as:  
```
w[i] = w[i] * r % q

β = {295, 592, 301, 14, 28, 353, 120, 236}
```
For the message to encrypt, we firstly translate each letter to binary. As a result, we can process message that has lenth of w.length / 8. Assume the first letter is 'a', then we have 0110001. 




```
import java.math.BigInteger;
import java.util.Scanner;

public class Main {

	public static void main(String[] args) {

		// Get the message to be encrypted.
		String inputText = getUserInput();

		// Start encrypting
		BigInteger base = new BigInteger("1");
		BigInteger multiplier = new BigInteger("2");
		BigInteger wSum = new BigInteger("0");

		// Get superincreasing sequence w
		DoublyLinkedList w = new DoublyLinkedList();
		for (int index = 0; index < 640; index++) {
			w.addBigIntegerAtEnd(base);
			wSum = wSum.add(base);
			base = base.multiply(multiplier);
		}

		// Generate q and r
		BigInteger q = wSum.add(base.divide(multiplier)); // q = 1.5 * sum
		BigInteger r = calculateR(q);

		// Generate public key
		DoublyLinkedList b = new DoublyLinkedList();
		DoubleNode dummy = w.getHead();
		for (int index = 0; index < 640; index++) {
			b.addBigIntegerAtEnd((dummy.getC().multiply(r)).mod(q));
			dummy = dummy.getNext();
		}

		// Encrypt the text
		BigInteger encryptSum = new BigInteger("0");
		int position = 0; // 0 - 640
		for (int index = 0; index < inputText.length(); index++) {

			// Get the binary string
			char currentChar = inputText.charAt(index);
			int ascii = currentChar;
			String binary = Integer.toBinaryString(ascii);
			while (binary.length() < 8) {
				binary = "0" + binary;
			}

			// Get the sum (encrypted code) for this node and add it to total
			BigInteger nodeSum = new BigInteger("0");
			for (int n = 0; n < binary.length(); n++) {
				BigInteger currentDigit = new BigInteger(binary.charAt(n) + "");
				nodeSum = nodeSum.add(currentDigit.multiply(b.getNthValue(position)));
				position++;
			}
			encryptSum = encryptSum.add(nodeSum);
		}

		System.out.println(inputText + " is encryped as\n" + encryptSum);

		// Decrypt
		BigInteger decryptKey = encryptSum.multiply(r.modInverse(q)).mod(q);

		// Restore the original binary sequence
		int[] binary = new int[position];
		for (int index = position - 1; index >= 0; index--) {
			if (decryptKey.compareTo(w.getNthValue(index)) >= 0) {
				binary[index] = 1;
				decryptKey = decryptKey.subtract(w.getNthValue(index));
			} else {
				binary[index] = 0;
			}
		}

		// Calculate and print original string.
		String result = getResult(binary);
		System.out.println("Result of decryption: " + result);
	}

	/**
	 * Get user input and display required information.
	 * 
	 * Precondition: none. 
	 * Postcondition: return user input as a string.
	 * 
	 * @return user input as a string.
	 */
	public static String getUserInput() {
		
		System.out.println("Enter a string and I will encrypt it as single large integer.");
		Scanner input = new Scanner(System.in);
		String inputText = input.nextLine();
		while (inputText.length() >= 80) {
			System.out.println("Your input is too long, please try again (within 80 characters)");
			inputText = input.nextLine();
		}

		System.out.println("Clear text:");
		System.out.println(inputText.toString());

		int textBytes = 0;
		for (int index = 0; index < inputText.length(); index++) {
			textBytes++;
		}
		System.out.println("Number of clear text bytes = " + textBytes);
		return inputText;
	}
	
	/**
	 * Calculate r using given q.
	 * 
	 * @param q key q.
	 * @return key r.
	 */
	public static BigInteger calculateR(BigInteger q) {
		BigInteger r = new BigInteger("1111111111111111111111111111111111111111");
		while (r.compareTo(q) != 0) {
			// If the greatest common divisor is 1, they are coprime.
			if (q.gcd(r).equals(new BigInteger("1"))) {
				break;
			}
			r = r.add(new BigInteger("2")); // Increment r
		}
		return r;
	}
	
	
	/**
	 * Calculate the original string with binary sequence provided. 
	 * 
	 * Precondition: input should be a valid binary array that has 8n elements of 0 or 1.
	 * Postcondition: return the original string the user entered.
	 * 
	 * Time complexity: No case Θ(n)
	 * 
	 * @param binary binary sequence of original string.
	 * @return original string.
	 */
	public static String getResult(int[] binary) {
		// Restore the original text according to the binary sequence
		StringBuilder temp = new StringBuilder();
		StringBuilder result = new StringBuilder();
		for (int index = 0; index < binary.length; index++) {
			temp.append(binary[index]);
			if (temp.length() == 8) {
				// Transfer the binary sequence back to char, and append it to
				// our result
				result.append((char) Integer.parseUnsignedInt(temp.toString() + "", 2));
				temp.setLength(0);
			}
		}
		return result.toString();
	}
}
```


```
import java.math.BigInteger;
/**
 * This class implements a doubly linked list of BigInteger in Java. The
 * instance variables head and tail are initially null. As elements are added
 * head points to the first element on the list and tail points to the last
 * element. Each node on the list is of type DoubleNode. Each DoubleNode holds a
 * pointer to the previous node and a pointer to the next node in the list.
 * 
 * @author Phoenix
 *
 */
public class DoublyLinkedList {

	private int n = 0; // Number of nodes in the list
	private DoubleNode head = null; // Head node of the list
	private DoubleNode tail = null; // Tail node of the list

	/**
	 * Constructs a new DoublyLinkedList object with head and tail as null.
	 */
	public DoublyLinkedList() {
		// Head and tail are already set to null
	}

	/**
	 * Test driver for DoublyLinkedList.
	 * 
	 * @param args
	 */
	public static void main(String args[]) {
		// Already implemented in DoubleNode.java
	}

	/**
	 * Add a BigInteger node containing the BigInteger c to the end of the linked
	 * list. This routine does not require a search.
	 * 
	 * Precondition: parameter is an BitInteger instance. 
	 * Postcondition: a new node with BigInteger the same as the parameter will be created and placed at the end of the list. 
	 * 
	 * Time complexity: No case Θ(1)
	 * 
	 * @param bi
	 *            a single BigInteger.
	 */
	public void addBigIntegerAtEnd(BigInteger bi) {
		DoubleNode newTail = new DoubleNode(tail, bi, null);
		if (tail == null) {
			tail = newTail;
			head = newTail;
		}
		else {
			tail.setNext(newTail);
			tail = newTail;
		}
		n++;
	}

	/**
	 * Add a BigInteger node containing the BigInteger c to the front of the
	 * linked list. No search is required.
	 * 
	 * Precondition: parameter is an BigInteger instance. 
	 * Postcondition: a new node with BigInteger the same as the parameter will be created and placed at the beginning of the list. 
	 * 
	 * Time complexity: No case Θ(1)
	 * 
	 * @param bi
	 *            a single BigInteger.
	 */
	public void addBigIntegerAtFront(BigInteger bi) {
		DoubleNode newHead = new DoubleNode(null, bi, head);
		if (head == null) {
			head = newHead;
			tail = newHead;
		}
		else {
			head.setPrev(newHead);
			head = newHead;
		}
		n++;
	}

	/**
	 * Counts the number of nodes in the list. We are not maintaining a counter
	 * so a search is required.
	 * 
	 * Postcondition: return the int value of the number of nodes in the list. 
	 * 
	 * Time complexity: No case Θ(1)
	 * 
	 * @return the number of nodes in the doubly linked list between head and
	 *         tail inclusive.
	 */
	public int countNodes() {
		return n;
	}

	/**
	 * Deletes the first occurence of the BigInteger c from the list. If the
	 * BigInteger c is not in the list then no modifications are made. This
	 * method needs to search the list.
	 * 
	 * Precondition: parameter is an BigInteger instance.
	 * Postcondition: the first node that has the value the same as the parameter would be removed from the list.
	 * 
	 * Time complexity: Best case Θ(1), worst case No case Θ(n)
	 * 
	 * @param bi
	 *            the BigInteger to be searched for.
	 * @return true if a deletion occurred and false otherwise.
	 */
	public boolean deleteBigInteger(BigInteger bi) {
		DoubleNode dummy = head;
		for (int i = n; i > 0; i--) {
			if (dummy.getC().toString().equals(bi.toString())) {
				if (n == 1) { // If the list has only one element
					head = null;
					tail = null;
				} else if (i == n) { // If the first element matches the BigInteger
					head = head.getNext();
					head.setPrev(null);
				} else if (i == 1) { // If the last element matches the BigInteger
					tail = tail.getPrev();
					tail.setNext(null);
				} else { // If an element in middle matches the BigInteger
					dummy.getNext().setPrev(dummy.getPrev());
					dummy.getPrev().setNext(dummy.getNext());
				}
				n--;
				return true;
			}
			dummy = dummy.getNext();
		}
		return false;

	}

	/**
	 * Returns true if the list is empty false otherwise.
	 * 
	 * Time complexity: No case Θ(1)
	 * 
	 * @return true if the list empty false otherwise.
	 */
	public boolean isEmpty() {
		return n == 0;
	}

	/**
	 * Remove and return the BigInteger at the end of the doubly linked list. No
	 * searching is required.
	 * 
	 * Postcondition: remove the last node (if not null) from the list.
	 * 
	 * Time complexity: No case Θ(1)
	 * 
	 * @return the BigInteger at the end precondition: the list is not empty.
	 */
	public BigInteger removeBigIntegerAtEnd() {
		BigInteger removed = null;
		if (tail != null) {
			removed = tail.getC();
			tail = tail.getPrev();
			n--;
			if (n == 0) {  // Removed the only node, head should be null, too
				head = null;
			}
			else { 
				tail.setNext(null);
			}
		}
		return removed;
	}

	/**
	 * Remove and return the BigInteger at the front of the doubly linked list.
	 * 
	 * Postcondition: remove the first node (if not null) from the list.
	 * 
	 * Time complexity: No case Θ(1)
	 * 
	 * @return the BigInteger at the front precondition: the list is not empty.
	 */
	public BigInteger removeBigIntegerFromFront() {
		BigInteger removed = null;
		if (head != null) {
			removed = head.getC();
			head = head.getNext();
			n--;
			if (n == 0) { // Removed the only node, tail should be null, too
				tail = null;
			}
			else { 
				head.setPrev(null);
			}
			
		}
		return removed;
	}

	/**
	 * Reverse the list. a -> b -> c becomes c -> b -> a
	 * 
	 * Postcondition: reverse the order of the list if there are more than two nodes. Otherwise, no changes. 
	 * 
	 * Time complexity: No case Θ(n)
	 */
	public void reverse() {
		head = tail;
		while (true) {
			DoubleNode dummy = tail.getPrev();
			tail.setPrev(tail.getNext());
			tail.setNext(dummy);
			if (dummy == null)
				break;
			tail = dummy;
		}
	}
	
	/**
	 * Get the BigInteger value from the list.
	 * 
	 * Precndition: n should be from 0 - 639.
	 * Postcondition: a corresponding BigInteger will be returned.
	 * 
	 * Time complexity: best case Θ(1), worst case Θ(n)
	 * 
	 * @param n node position.
	 * @return the corresponding BigInteger value.
	 */
	public BigInteger getNthValue(int n) {
		DoubleNode dummy = head;
		for (int i = 0; i < n; i++) {
			dummy = dummy.getNext();
		}
		return dummy.getC();
	}
	
	/**
	 * Get the head node.
	 * @return head node.
	 */
	public DoubleNode getHead() {
		return head;
	}

	@Override
	/**
	 * Returns the list as a String. Thes class DoubleNode has a toString that
	 * will be called from this toString. The String returned must be presented
	 * clearly. Null pointers must be represented differently than non-null
	 * pointers.
	 * 
	 * Time complexity: No case Θ(1)
	 * 
	 * @return a String containing the BigInteger in the list.
	 */
	public String toString() {
		DoubleNode dummy = head;
		StringBuilder sb = new StringBuilder();
		while (dummy != null) {
			sb.append(dummy.getC().toString() + " ");
			dummy = dummy.getNext();
		}
		return sb.toString();
	}

}
```

```
import java.math.BigInteger;

/**
 * The class DoubleNode holds two pointers and a BigInteger. It is used to
 * represent a single node on a double linked list.
 * 
 * @author Phoenix
 *
 */
public class DoubleNode {

	private BigInteger bi = new BigInteger("0"); // object of this node
	private DoubleNode next = null; // the next node
	private DoubleNode prev = null; // the previous node

	/**
	 * Constructor with no arguments. Assign null to previous, next and
	 * the null to c.
	 */
	public DoubleNode() {
		// Previous and next are already set to null
	}

	/**
	 * A constructor takes in the previous and the next nodes and also BigInteger
	 * instance of this node.
	 * 
	 * @param p
	 *            is a pointer to a previous node.
	 * @param bi
	 *            is a BigInteger for this node.
	 * @param n
	 *            is a pointer to a next node.
	 */
	public DoubleNode(DoubleNode p, BigInteger bi, DoubleNode n) {
		this.prev = p;
		this.bi = bi;
		this.next = n;
	}

	/**
	 * Test driver for DoubleNode
	 * 
	 * @param a
	 */
	public static void main(String a[]) {

		
	}

	/**
	 * Get the BigInteger instance of this node.
	 * 
	 * Time complexity: No case Θ(1)
	 * 
	 * @return current BigInteger instance of this node.
	 */
	public BigInteger getC() {
		return this.bi;
	}

	/**
	 * Set the BigInteger instance of this node.
	 * 
	 * Precondition: parameter is an BigInteger instance.
	 * Postcondition: the BigInteger instance of this node would be set to the parameter instance. 
	 * 
	 * Time complexity: No case Θ(1)
	 * 
	 * @param ch
	 *            BigInteger assigned to this node.
	 */
	public void setC(BigInteger bi) {
		this.bi = bi;
	}

	/**
	 * Get next node.
	 * 
	 * Time complexity: No case Θ(1)
	 * 
	 * @return a pointer to the next node or null if none exists
	 */
	public DoubleNode getNext() {
		return this.next;
	}

	/**
	 * Set next node.
	 * 
	 * Precondition: parameter is an instance of DoubleNode class or null. 
	 * Postcondition: the next node of the current node would be the parameter instance.  
	 * 
	 * Time complexity: No case Θ(1)
	 * 
	 * @param next
	 *            the intended node to be set.
	 */
	public void setNext(DoubleNode next) {
		this.next = next;
	}

	/**
	 * Get previous node.
	 * 
	 * Time complexity: No case Θ(1)
	 * 
	 * @return a pointer to the previous node or null if none exists
	 */
	public DoubleNode getPrev() {
		return this.prev;
	}

	/**
	 * Set previous node.
	 * 
	 * Precondition: parameter is an instance of DoubleNode class or null. 
	 * Postcondition: the previous node of the current node would be the parameter instance.  
	 * 
	 * Time complexity: No case Θ(1)
	 * 
	 * @param prev
	 *            the intended node to be set.
	 */
	public void setPrev(DoubleNode prev) {
		this.prev = prev;
	}

	@Override
	/**
	 * Return the BigInteger value of this node.
	 * 
	 * @return a String value of BigInteger in the node.
	 */
	public String toString() {
		return this.bi.toString();
	}
}
```
