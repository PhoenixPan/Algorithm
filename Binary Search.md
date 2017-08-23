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
1. `mid` cannot sit outside of the range.  
  If `hi = A.length`, then `while(lo < hi)`, `hi = mid`  
  If `hi = A.length - 1`, then `whilw(lo <= hi)`, `hi = mid - 1`   
2.  根据right和left收缩时+1还是-1，决定mid如何计算。如果是left=mid, 那么计算mid时就取(left + right + 1)/2，否则取(left + right)/2（这是因为后者的mid是偏向左边的，当right-left=1时，left本身就是mid，如果此时用left=mid的话，搜索范围就不会变，无限循环);
3. 退出条件必定是left<right或者left<=right中的一种。至于是哪种，取决于找到的元素是否一定是正确的元素。比如Search Insert Position这道题，当left=right时，找到的位置必然是可行的位置。这种情况下就不需要再检查了；而如果要看数组中究竟有没有某个元素，那最后缩到范围内只有一个数时，还是要再检查一下，这时就要加上=。
4. 如果在循环内返回，那么总是返回mid，如果在循环外返回，永远返回left。循环外除特殊情况外不做判断。当做到1、2、3以后，4一般情况下会自动成立。

### Highlight 1: `while (lo <= hi)`
We need to check the point where lo == hi. For example: [1,2,3,4], 4 (target at boundary: 1 or 4).  

### Highlight 2: `lo = mid + 1` and `hi = mid - 1`
Make sure they will change to avoid infinite loop. When hi = lo + 1, mid will always be lo (0 + 1/2 -> 0). When we try to find the lower bound when lo = mid, we may get stuck there. . To resolve this, we can change the code to `mid = lo + (hi-lo+1) / 2`. 

Remeber, always to set test case \[no, yes], such as \[1,2], 2.  

## Wildcard solution
The range control is complicated isn't it? So we introduce a while card solution:
```
while (lo + 1 < hi) // hi - lo >= 2, so mid will always change
if (mid = target)
  return mid;
else if (mid > target)
  lo = mid;         // no need to worry about +1 or not anymore
else if (mid < target)
  hi = mid;

// check the values of lo and hi at last
if (lo == target or hi == target)
 return lo or hi;
return -1;
```

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
