## String Match
Find pattern of length M in a text of length N, usually N>>M.  
1. Naive method (brute force)
2. Boyer-Moore method
3. Knuth-Morris-Pratt (KMP) method
4. Rabin-Karp method

### Naive Method
For every position in the string, consider it a starting position of the pattern and see if we get a match.  
In natural language processing, the naive method gives decent performance, since breakpoint is usually quick to identify. For the same reason, this method is not good for long needle.  

Efficiency:
Worst case: O(nm)  
With very short needles, naive method may outperform other methods.

```
public static void naiveMatcher(String needle, String haystack) {
  int m = needle.length();
  int n = haystack.length();
  int count = 0;
  for (int i = 0; i < n - m ; i++) {
    for (int j = 0; j < n; j++) {
      if (needle.charAt(j) != haystack.charAt(i + j))
        break;
      if (j == m) 
        count++; // orprint out this match
    }
  }
  System.out.println(count + " match(es) found!");
}
```

### Boyer-Moore Method

Worst case: O(nm)  

```
public static int search(String needle, String haystack) {
  int m = needle.length();
  int n = haystack.length();
  int skip;
  for (int i = 0; i <= n - m; i += skip) {
    skip = 0;
    for (int j = m - 1; j >= 0; j--) {
      if (needle.charAt(j) != haystack.charAt(i+j)) {
        skip = Math.max(1, j - haystack.charAt(i+j));
        break;
      }
    }
    if (skip == 0) 
	  return i;
  }
  return 100;
} 
```

### Knuth-Morris-Pratt (KMP) Matcher  
Avoid backup in naive method.  

```
public static void naiveMatcher(String needle, String haystack) {
  int m = 0;
  int i = 0;
}
```

### Rabin-Karp Algorithm (RK)
Using hashtable. Easy to implement and improved speed. However, a good hash function is required for this method to work. It could also consumes a lot of memory.



### References
http://algs4.cs.princeton.edu/lectures/53SubstringSearch.pdf  
http://www.geeksforgeeks.org/searching-for-patterns-set-2-kmp-algorithm/  
