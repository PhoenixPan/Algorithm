# Sort
1. in-place and not-in-place: whether the sorting will use additional spaces
2. stable or unstable: whether the sorting will change the order of elements that have the same value: 8A 8B -> 8A 8B or 8B 8A?
3. adaptive or non-adaptive: whether the sorting will take advantage of the already sorted elements
4. 

## Bubble sort
Worst case: O(n^2)  
Swap the elements if A[n] > A[n+1], moving the largesting element in the unsorted part to the end during each iteration.  

```
public static int[] bubbleSort(int[] list) {
  int len = list.length;
  if (len == 1) 
    return list;

  for (int j = len - 1; j > 0; j--) {
    for (int i = 0; i < j; i++) {
      if (list[i] > list[i + 1]) {
        int temp = list[i];
        list[i] = list[i + 1];
        list[i + 1] = temp;
      }
    }
  }
}
```

## Insertion sort
Worst case: O(n^2)  

```
public static int[] insertionSort(int[] list) {
  int len = list.length;
  if (len == 1) 
    return list;

  for (int i = 0;i < len - 1; i++) {
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

## Selection sort


##
##
