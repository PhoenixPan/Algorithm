# Binary Search

Conditions:
1. Sorted
2. Array

在一个包含x的数组内，二分查找通过对范围的跟综来解决问题。开始时，范围就是整个数组。通过将范围中间的元素
与x比较并丢弃一半范围，范围就被缩小。这个过程一直持续，直到在x被发现，或者那个能够包含t的范围已成为空。




## Basic: Find a target from an array
```java
public int BinarySearch(int[] nums, int target) {
  int start = 0;
  int end = nums.length - 1;
  while (start <= end) {
    int mid = start + (end - start) / 2;
    if (nums[mid] < target) 
      start = mid;
    else if (nums[mid] > target)
      end = mid;
    else 
      return mid;
  }
}
```
Q1: `while (start <= end)` We need to check the point where start == end. For example: [1,2,3,4], 4 (target at last).  
Q2: `start = mid + 1` and `end = mid - 1" make sure they will change to avoid infinite loop. For instance, when end = start + 1, mid will always be start (+ 1/2 -> 0) 

More efficient way to get mid = low + ((high - low) >> 1) 
