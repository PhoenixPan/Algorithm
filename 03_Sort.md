# Sort
1. in-place and not-in-place: whether the sorting will use additional spaces
2. stable or unstable: whether the sorting will change the original order of elements that have the same value: 8A 8B -> 8A 8B or 8B 8A?
3. adaptive or non-adaptive: whether the sorting will take advantage of the already sorted elements

## Bubble Sort
1. Repeatedly compare neighbor pairs and swap if necessary.  
2. In each iteration, check all pairs for whether they need to be swapped. After each swap, the largest element (bubble) in the unsorted part should be moved to the right side.  
3. The sorted parted doesn't change once positioned.  

| Features           | Value            |
| ------------------ | ---------------- |
| Best Time          | O(n)             |  
| Worst Time         | O(n^2)           |  
| Space Complexity   | O(1)             |
| Stable             | Stable           |
| In place?          | Yes              |

```java
public static int[] bubbleSort(int[] list) {
  int len = list.length;
  if (len <= 1) 
    return list;

  for (int i = 0; i < len - 1; i++) {
     // The right side of the list is ordered and have no need to check
    for (int j = 0; j < len - i - 1; j++) { 
      if (list[j] > list[j + 1]) {
        int temp = list[j];
        list[j] = list[j + 1];
        list[j + 1] = temp;
      }
    }
  }  
}
```

## Selection Sort
1. Repeatedly select the smallest element in the list and append it to the result (by swapping it with the element in the nth position).  2. The sorted parted doesn't change once positioned.    
3. Always need quadratic number of compares((n-1)+(n-2)+...+2+1=(1/2\*n^2)) but only meed liner number of swaps(n). 

| Features           | Value            |
| ------------------ | ---------------- |
| Best Time          | O(n)             |  
| Worst Time         | O(n^2)           |  
| Space Complexity   | O(1)             |
| Stable             | Unstable         |
| In place?          | Yes              |

```java
public static int[] selectionSort(int[] list) {
  int len = list.length;
  if (len <= 1) 
    return list;

  for (int i = 0; i < len - 1; i++) { // We don't need to check the last element, it will be the largest
    for (int j = i; j < len; j++) {
      if (list[i] > list[j]) {
        int temp = list[i];
        list[i] = list[j];
        list[j] = temp;
      }
    }
  }
  return list;
}
```
## Insertion Sort
1. Repeatedly add new element to the sorted result.   
2. Take the first element as sorted sub-array and add new element by swaping it to the proper, sorted position.  
3. Could use a fewer number of compares(n) in best cases but more swaps in worst cases(1/2\*n^2), same as compares) 

| Features           | Value            |
| ------------------ | ---------------- |
| Best Time          | O(n)             |  
| Worst Time         | O(n^2)           |  
| Space Complexity   | O(1)             |
| Stable             | Stable           |
| In place?          | Yes              |

```java
public static int[] insertionSort(int[] list) {
  int len = list.length;
  if (len <= 1) 
    return list;

  for (int i = 0; i < len - 1; i++) {
    while (i >= 0 && list[i] > list[i + 1]) {
        int temp = list[i];
        list[i] = list[i + 1];
        list[i + 1] = temp;
        i--;
    }
  }
  return list;
}
```

## Merge Sort
1. Divide, conquer, combine: Dividethe array into one-element-long sub-array, conquer (sort) each sub-array pair, and recursively combine (merge) them together. 

| Features           | Value            |
| ------------------ | ---------------- |
| Best Time          | O(nlogn)         |  
| Worst Time         | O(nlogn)         |  
| Space Complexity   | O(n)             |
| Stable             | Stable           |
| In place?          | No               |

```java
public class MergeSort {

  private static void sort(Comparable[] a) {
    Comparable[] aux = new Comparable[a.length];
    sort(a, aux, 0, a.length - 1);
  }
  
  private static void sort(Comparable[] a, Comparable[] aux, int lo, int hi) {
    if (lo >= hi) {
      return;
    }
    int mid = lo + (hi - lo) / 2;
    sort(a, aux, lo, mid);
    sort(a, aux, mid + 1, hi);
    // optimization: already sorted
    if (a[mid + 1].compareTo(a[mid]) > 0) {
      return;
    }
    merge(a, aux, lo, mid, hi);
  }
  
  private static void merge(Comparable[] a, Comparable[] aux, int lo, int mid, int hi) {
    for (int k = lo; k <= hi; k++) {
      aux[k] = a[k];
    }
    int i = lo; // start of the first half
    int j = mid + 1; // start of the second half
    for (int k = lo; k <= hi; k++) {
      if (i > mid) { // if all elements in the first half are recorded
        a[k] = aux[j++]; 
      } else if (j > hi) { // if all elements in the second half are recorded
        a[k] = aux[i++];
      } else if (less(aux[j], aux[i]) {
        a[k] = aux[j++];
      } else {
        a[k] = aux[i++];
      }
    }
  }
}
```

![mergesort](https://user-images.githubusercontent.com/14355257/30248039-eb6bce38-9663-11e7-84af-d6a2c8fb79ca.png)
Compare: 1 in each operation  
Array access: 6 in each operation  
Total: nlogn + 6nlogn = 7nlogn = O(nlogn)   


## Quick Sort
1. One of the fastest sorting algorithm, usually outperform merge sort.  

| Features           | Value            |
| ------------------ | ---------------- |
| Best Time          | O(nlogn)         |  
| Worst Time         | O(n^2)           |  
| Space Complexity   | O(1)             |
| Stable             | Unstable         |
| In place?          | No               |

```
public static void main(String[] args) {
  int[] list = {6, 3, 4, 7, 1, 5};
  quickSort(list, 0, list.length - 1);
  System.out.println(Arrays.toString(list));
}

public static void quickSort(int[] list, int start, int end) {
  if (start < end) {
    int pivotIndex = partition(list, start, end);
    quickSort(list, start, pivotIndex - 1);
    quickSort(list, pivotIndex + 1, end);
  }
}

public static int partition(int[] list, int start, int end) {
  int pivot = list[end];
  int pointer = 0;
  for (int i = 0; i < end; i++) {
    if (list[i] < pivot) {
      int temp = list[pointer];
      list[pointer] = list[i];
      list[i] = temp;
      pointer++;
    }
  }
  int temp = list[pointer];
  list[pointer] = pivot;
  list[end] = temp;

  return pointer;
}
```

### References
http://www.titrias.com/ultimate-sorting-algorithms-comparison/
http://theoryapp.com/selection-insertion-and-bubble-sort/
