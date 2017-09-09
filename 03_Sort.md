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
| Space Complexity   | Depends          |
| Stable             | Stable           |
| In place?          | No               |

```
// I used all primitive operations rather than Java functions to show all the processes
public static int[] mergeSort(int[] list) {
  int len = list.length;
  if (len <= 1) 
    return list;
  
  // Divide: Split the array into two, could use System.arraycopy here.
  // To copy an array, System.arraycopy performs well for large array and primitive loop method is good for small array.  
  int[] sub1 = new int[len/2];  
  int[] sub2 = new int[len - len/2];  
  int index1 = 0;
  int index2 = 0;
  for (int i = 0; i < len; i++) {
    if (i < len/2) {
      sub1[index1] = list[i];
      index1++;
    }
    else {
      sub2[index2] = list[i];
      index2++;
    }
  }
  
  // Conquer
  sub1 = mergeSort(sub1);
  sub2 = mergeSort(sub2);
  
  // Combine
  return merge(sub1, sub2);
}

public static int[] merge(int[] sub1, int[] sub2) {
  int len1 = sub1.length;
  int len2 = sub2.length;
  int index1 = 0;
  int index2 = 0;
  int[] result = new int[len1 + len2];
  
  for (int i = 0; i < len1 + len2; i++) {
    if (index1 < len1 && index2 < len2) { // Could use Math.min() here.
      if (sub1[index1] <= sub2[index2]) {
         result[i] = sub1[index1];
         index1++;
      }
      else {
         result[i] = sub2[index2];
         index2++;
      }                              
    }
    else if (index1 < len1) {
        result[i] = sub1[index1];
        index1++;
    }
    else if (index2 < len2) {
        result[i] = sub2[index2];
        index2++;
    }
  }
  
  return result;
}
```

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
