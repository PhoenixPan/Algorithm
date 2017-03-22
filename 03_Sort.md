# Sort
1. in-place and not-in-place: whether the sorting will use additional spaces
2. stable or unstable: whether the sorting will change the original order of elements that have the same value: 8A 8B -> 8A 8B or 8B 8A?
3. adaptive or non-adaptive: whether the sorting will take advantage of the already sorted elements

## Bubble Sort
Repeatedly compare neighbor pairs and swap if necessary.  
In each iteration, check all pairs for whether they need to be swapped. After each swap, the largest element (bubble) in the unsorted part should be moved to the right side. The sorted parted doesn't change once positioned.

| Features           | Value            |
| ------------------ | ---------------- |
| Best Time          | O(n)             |  
| Worst Time         | O(n^2)           |  
| Space Complexity   | O(n)             |
| Stable             | Stable           |

```
public static int[] bubbleSort(int[] list) {
  int len = list.length;
  if (len == 1) 
    return list;

  for (int j = len - 1; j > 0; j--) {
    for (int i = 0; i < j; i++) { // The right side of the list is ordered and have no need to check
      if (list[i] > list[i + 1]) {
        int temp = list[i];
        list[i] = list[i + 1];
        list[i + 1] = temp;
      }
    }
  }
}
```

## Insertion Sort
Repeatedly add new element to the sorted result (by swaping each element to as left as it can be). The sorted parted still change as new elements come in.  

| Features           | Value            |
| ------------------ | ---------------- |
| Best Time          | O(n)             |  
| Worst Time         | O(n^2)           |  
| Space Complexity   | O(n)             |
| Stable             | Stable           |

```
public static int[] insertionSort(int[] list) {
  int len = list.length;
  if (len == 1) 
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

## Selection Sort
Repeatedly select the smallest element in the list and append it to the result (by swapping it with the element in the nth position). The sorted parted doesn't change once positioned.    

| Features           | Value            |
| ------------------ | ---------------- |
| Best Time          | O(n)             |  
| Worst Time         | O(n^2)           |  
| Space Complexity   | O(n^2)           |
| Stable             | Unstable         |

```
public static int[] selectionSort(int[] list) {
  int len = list.length;
  if (len == 1) 
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

## Merge Sort
## Quick Sort
