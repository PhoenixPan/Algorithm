# Binary Search

Conditions:
1. Sorted
2. Random access (array, not linked list)

在一个包含x的数组内，二分查找通过对范围的跟综来解决问题。开始时，范围就是整个数组。通过将范围中间的元素与x比较并丢弃一半范围，范围就被缩小。这个过程一直持续，直到在x被发现，或者那个能够包含t的范围已成为空。

Low and high (start and end): a closed interval which contains the result. It's important to move boundaries carefully, as it's where bugs find their ways.  

## Basic: Find a target in an array
Answers: \[no, no, no, no, yes, no, no, no]  

```java
public int BinarySearch(int[] nums, int target) {
  int lo = 0;
  int hi = nums.length - 1;
  while (lo <= hi) {
    int mid = lo + (hi - lo) / 2;
    if (nums[mid] < target) 
      lo = mid;
    else if (nums[mid] > target)
      hi = mid;
    else 
      return mid;
  }
  return -1;
}
```

## Range control
`mid` cannot sit outside of the range.  
If `hi = A.length`, then `while(lo < hi)`, `hi = mid`  
If `hi = A.length - 1`, then `whilw(lo <= hi)`, `hi = mid - 1`   

### Q1: `while (lo <= hi)`
We need to check the point where lo == hi. For example: [1,2,3,4], 4 (target at boundary: 1 or 4).  

### Q2: `lo = mid + 1` and `hi = mid - 1`
Make sure they will change to avoid infinite loop. When hi = lo + 1, mid will always be lo (0 + 1/2 -> 0). When we try to find the lower bound when lo = mid, we may get stuck there. . To resolve this, we can change the code to `mid = lo + (hi-lo+1) / 2`. 

Remeber, always to set test case \[no, yes], such as \[1,2], 2.  


## Phase 2: Find the best answer among many candidates 
Answers: \[no, no, no, no, yes, yes, yes, yes]
Given an array A and a target value, return the index of the first element in A equal to or greater than the target value.  

It's more efficient way to get mid = low + ((high - low) >> 1)   

```java
  /**
   * Find the minimum element that is larger than the target.
   * (no duplication)
   * @param A array
   * @param target target
   * @return index
   */
  public static int BinarySearchMinLarge(int[] A, int target) {
    int lo = 0;
    int hi = A.length - 1;
    while (lo < hi) {
      int mid = lo + (hi - lo) / 2;
      if (A[mid] >= target)
        hi = mid;          // A[mid] could be a solution, so we only include mid, not mid - 1
      else
        lo = mid + 1;      // since A[mid] < target, it's not a solution, we could use + 1
    }
    return lo;
  }
```

## References
http://www.1point3acres.com/bbs/forum.php?mod=viewthread&tid=161799&highlight=binary%2Bsearch  
https://www.topcoder.com/community/data-science/data-science-tutorials/binary-search/  
https://github.com/julycoding/The-Art-Of-Programming-By-July/blob/master/ebook/zh/04.01.md  
