## String match
Find pattern of length M in a text of length N, usually N>>M.

### Naive method
For every position in the string, consider it a starting position of the pattern and see if we get a match. In natural language processing, the naive method gives decent performance, since breakpoint is usually easy to identify. For the same reason, this method is not good for long text or systemic text.  

Worst case: O(nm)  

```
public static void naiveMatcher(String needle, String haystack) {
  int n = needle.length();
  int m = haystack.length();
  int count = 0;
  for (int i = 0; i < m - n ; i++) {
    for (int j = 0; j < n; j++) {
      if (needle.charAt(j) != haystack.charAt(i + j))
        break;
      if (j == n) 
        count++; // orprint out this match
    }
  }
  System.out.println(count + " match(es) found!");
}
```

### Rabin-Karp Algorithm (RK)
Using hashtable. Easy to implement and improved speed. However, a good hash function is required for this method to work. It could also consumes a lot of memory.

### Knuth-Morris-Pratt (KMP) Matcher


