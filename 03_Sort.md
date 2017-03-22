# Sort
1. in-place and not-in-place: whether the sorting will use additional spaces
2. stable or unstable: whether the sorting will change the order of elements that have the same value: 8A 8B -> 8A 8B or 8B 8A?
3. adaptive or non-adaptive: whether the sorting will take advantage of the already sorted elements

## Bubble sort
Moving the largesting element in the unsorted part to the end in each iteration through constant swaps. The sorted parted doesn't change once positioned.  

| Features           | Value            |
| ------------------ | ---------------- |
| Time Complexity    | O(n^2)           |  
| Space Complexity   | O(n)             |
| Stable             |                  |

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

## Insertion sort
Swap each element to as left as it can be. New element could still insert into the ordered part.  

| Features           | Value            |
| ------------------ | ---------------- |
| Time Complexity    | O(n^2)           |  
| Space Complexity   | O(n)             |
| Stable             |                  |

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
Select the smallest element and move it to the leftmost by swapping it with the current leftmost element.  

| Features           | Value            |
| ------------------ | ---------------- |
| Time Complexity    | O(n^2)           |  
| Space Complexity   | O(n)             |
| Stable             |                  |

```
public static int[] insertionSort(int[] list) {
  int len = list.length;
  if (len == 1) 
    return list;

  for (int i = 0;i < len - 1; i++) { // We don't need to check the last element, it will be the largest
    int min = list[i];
    int minIndex = i;
    for (int j = i;j < len; j++) {
      if (min > list[j]) {
        min = list[j];
        minIndex = j;
      }
    }
    int temp = list[minIndex];
    list[minIndex] = list[i];
    list[i] = temp;
  }
  return list;
}
```
##
##
