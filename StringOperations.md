# String Match
Find pattern of length n in a text of length N, usually N>>M.  
1. Naive method (brute force)
2. Boyer-Moore method
3. Knuth-Morris-Pratt (KMP) method
4. Rabin-Karp method

### Naive Method
For every position in the string, consider it a starting position of the pattern and see if we get a match.  
In natural language processing, the naive method gives decent performance, since breakpoint is usually quick to identify. For the same reason, this method is not good for long pattern.  

Efficiency:
Worst case: O(nm)  
With very short patterns, naive method may outperform other methods.

```
public static void naiveSearch(String pattern, String text) {
  int m = text.length();
  int n = pattern.length();
  int count = 0;
  for (int i = 0; i < m - n ; i++) {
    for (int j = 0; j < n; j++) {
      if (pattern.charAt(j) != text.charAt(i + j))
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
public static int BMSearch(String pattern, String text) {
  int m = text.length();
  int n = pattern.length();
  int skip;
  for (int i = 0; i <= m - m; i += skip) {
    skip = 0;
    for (int j = n - 1; j >= 0; j--) {
      if (pattern.charAt(j) != text.charAt(i+j)) {
        skip = Math.max(1, j - text.charAt(i+j));
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
Start from the point where mismatch occurs. 
If the checked part contains no pattern headings, then start from the next index in the text:  t[n + 1], p[0]  
Otherwise, start from the mismatch position in text and also a later position in the pattern: t[n + 1], p[2]
Avoid backup in naive method. 

```
public class KMPSearch {
  public static void main(String[] args) {
  	System.out.println(KMPMatcher("abcde", "bcfabcdabcde"));
  }

  public static String KMPMatcher(String pattern, String text) {
    int m = text.length();
    int n = pattern.length();
    int i = 0;
    int j = 0;
    
    while (i < m) {
      if (text.charAt(i) == pattern.charAt(j)) {
        i++;
        j++;
      }
      else {
      	String matchPart = text.substring(i - j, i);
      	int matchLen = matchPart.length();
	if(matchLen > 0 && matchPart.substring(1, matchLen).indexOf(pattern.charAt(0)) > -1) {
	  System.out.println(matchPart);
	  i = i - j;
	  j = 0;
	}
	else {
      	  i++;
      	  j = 0;
      	}
      }
      if (j == n) {
        return text.substring(i - j, i);
      } 
    }
    return "None"; 
  }
}
```

### Rabin-Karp Algorithm (RK)
Using hashtable. Easy to implement and improved speed. However, a good hash function is required for this method to work. It could also consumes a lot of memory.



### References
http://algs4.cs.princeton.edu/lectures/53SubstringSearch.pdf  
http://www.geeksforgeeks.org/searching-for-patterns-set-2-kmp-algorithm/  

# Longest Common Subsequence


