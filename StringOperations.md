## String match


### Naive method
For every position in the string, consider it a starting position of the pattern and see if we get a match. In natural language processing, the naive method gives decent performance, since breakpoint is usually easy to identify. For the same reason, this method is not good for long text or systemic text.  

Worst case: O(nm), where n is the length of the pattern and m is the length of the string.  

```
public static void naiveMatch(String needle, String haystack) {
  int target = needle.length();
  int total = haystack.length();
  int count = 0;
  for (int i = 0; i < total - target ; i++) {
    if (needle.equals(haystack.substring(i, i + target)))
      count++; // Or we can print index here
  }
  System.out.println(count + " match(es) found!");
}
```

### Rabin-Karp Algorithm (RK): 
