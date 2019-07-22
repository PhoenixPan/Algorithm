# Dynamic Programming

1. DP: Using basic elements to fill up the entire structure (bottom-up or top-down)  
  Recursion: Does not record steps in the middle (top-down)
2. Usually more efficient than recursion (such as in Fibonacci) which does not record anything
3. Relieve the bottleneck of von Neumann architecture
4. N-dimension DP: data can be stored in a N-dimension data structure (array, list, etc.)

##### One-dimension DP:  
- [Example 1: Cutting rope](#example1)
- [Example 2: Jump game](#example2)
- [Example 3: Longest ascending subarray](#example3)
- [Example 4: Dictionary problem](#example4)

## Resolving strategies  
1. Find the maximum or minimum in one-dimension data  

    1.1 When weight of each smallest element in the original data is identical(length, standard unit) or similar(letter, number), we can usually use **liner scan and look back** to the previous result. Examples:
    
    *Longest acsending subarray (when at i, look back at i-1)  
    Longest acsending subsequence (when at i, look back at i-1 to 1)  
    Cut rope  
    Cut palindrome*  
    
    1.2 When weight is not identical  

## Steps
1. Base case: starting point (from left or from right)    
2. Induction rule f(n)
```
s[0] = ? // usually doesn't matter
s[1] = ? // start to give you the induction rule, find the relationship between s[0] and s[1]
...
s[n] = global solution
```
3. English expression of the meaning
4. Coding

## Key words
base case, induction rule, iteration and recursion, sub-array and sub-sequence

<a id="example1"></a>  
## Example 1: Cutting rope
Give a rope with integer length n, how to cut the rope into m integer length parts with length p[0], p[1], ..., p[m-1] in order to get the maximal product of p[0] * p[1] * ... * p[m-1]? m is determined by you and must be greater than 0 (at least one cut must be made) 

### Method 1: DP on both sides 左大段 右大段
We only consider one cut position(i) at a time, then we should find the largest products on both left (M[i]) and right sides (M[n-i]), which are gained dynamically from previous calculations. We will always find M[i], since i < n.   

**1. Base case**: M[1], can't be cut  
**2. Induction rule**:  
M[1] = 1  

M[2] = max(1, M[1]) * max(1, M[1])  --|--  

M[3] = max(1, M[1]) * max(2, M[2])  --|-- --  
M[3] = max(2, M[2]) * max(1, M[1])  -- --|--  

M[4] = max(1, M[1]) * max(3, M[3])  --|-- -- --  
M[4] = max(2, M[2]) * max(2, M[2])  -- --|-- --  
M[4] = max(3, M[3]) * max(1, M[1])  -- -- --|--  

Time: O(n^2)  
Space: O(n)  

```java
    int cutRope1(int n) {
    if(n < 2) return 1; // n has to be at least 2, as one cut has to be made
    int[] dp = new int[n + 1];

    for(int i = 1; i <= n; i++)
      for (int j = 1; j < i; j++)
        dp[i] = Math.max(Math.max(j, dp[j]) * Math.max(i-j, dp[i-j]), dp[i]);

    return dp[n];
}
```

### Method 2：DP only on one side 左大段 右小段
Enumerate concrete numbers on the right side. It's a more general application comparing with method 1.  

**1. Base case**: M[1], can't be cut  
**2. Induction rule**:  
M[1] = 1  

M[2] = max(1, M[1]) * 1    (right side can only be 1)  

M[3] = max(2, M[2]) * 1    (right side can be 1 or 2)  
M[3] = max(1, M[1]) * 2  

M[4] = max(3, M[3]) * 1    (right side can be 1 or 2 or 3)   
M[4] = max(2, M[2]) * 2  
M[4] = max(1, M[1]) * 3  

Time: O(n^2)  
Space: O(n)  

```java
int cutRope2(int n) {
    if(n < 2) return 1; // n has to be at least 2, as one cut has to be made
    int[] dp = new int[n + 1];
    dp[1] = 1;

    for(int i = 2; i <= n; i++)
      for(int j = 1; j < i; j++)
        dp[i] = Math.max(j * Math.max(i-j, dp[i-j]), dp[i]);
        
    return dp[n];
}
```
### Method 3: Recursion (skipped)

<a id="example2"></a>  
## Example 2: Jump game
Given an array of non-negative integers, you are initially positioned at the first index of the array. Each element in the array represents your maximum jump length at that position. Determine if you are able to reach the last index.

**Key: from right to left.**
**1. Base case**: M[n-1] = true, so it can jump out  
**2. Induction rule**:
If there's a j <= input[i] so that M[i+j] = true, M[i] = true     
return M[0]   

```
M[], input[]
M[length-1] = true (requirement)  
for (j < input[i])
    if (M[i+j] = true) M[i] = true
return M[0]  
```
Time: O(nk) (k is the largest input[i])  
Space: O(n)  

### Follow-up: Minimum number of jumps
**return the minimum number of jumps needed to reach the end?**

**1. Base case**: M[n-1] = 0, so it can jump out  
**2. Induction rule**:
Arrays.fill(M, Integer.MAX_VALUE);  
If there's a j <= input[i] so that M[i+j] = true, M[i] = 1 + min(M[i+j])       
return M[0]  


Time: O(nk) (k is the largest input[i])  
Space: O(n)  
```
M[n-1] = 0
for (int j: 0< j < input[i])
  M[i] = Math.min(M[i], M[i+j] + 1)  // j <= input[i]
```

<a id="example3"></a>  
## Example 3: Longest ascending subarray
Given an unsorted array, find the subarray that has the greatest sum. Return the sum.  

**1. Base case**: M[0]  
**2. Induction rule**: M[i] represents [from 0th element to the i-th element] the value of the largest sum of the subarray (including the i-th element)  
M[i] = M[i] + M[i-1] if M[i] > 0 (provide positive contribution)  
     = M[i]          else  
     
Time: O(n)  
Space: O(n)  
```
M[0] = input[0];
int max = Integer.MIN_VALUE;
for (1<= i < n) {
  if (M[i-1] >= 0)
    M[i] = input[i] + M[i-1];
  else 
    M[i] = input[i];
  max = max(max, M[i]);
}
return max;
```

### Follow-up 1: How to optimize space to Space O(1)?
We don't need to know every number in M, we only need M[i-1]. Use a variable instead of M.  

### Follow-up 2: Get the head and tail index of the subarray 
Keep two pointers, reset the pointers when event M[i-1] < 0 is triggered.  

```
M[0] = input[0];
int max = Integer.MIN_VALUE;
int start, end = 0;
int globalStart, globalEnd = 0;
for (1<= i < n) {
  if (M[i-1] >= 0)
    M[i] = input[i] + M[i-1];
    end++;
  else
    M[i] = input[i];
    start = i;
    end = i;
    
  if (M[i] > max)
    max = M[i];
    globalStart = start;
    globalEnd = end;
}
return start, end;
```

<a id="example4"></a>  
## Example 4: Dictionary problem
Given a word, can it be composed by concatenating words from a given dictionary? 
Dictionary: bob, car, rob  
Example: bcobat(F), bobcatrob(T)  

**1. Base case**: M[0] = false  
**2. Induction rule**:  M[i] = OR (M[j] AND substring(j+1...i) is in dictionary), 0<= j < i  
M[1] = false input[1] = b  
One case only, false  

M[2] = false input[2] = bo  
Two cases  
1. No cut, false  
2. 左大段 b = M[1], 右小段 o: check dictionary. false  

M[3] = true input[3] = bob  
Three cases  
1. No cut. true  
2. 左大段 b = M[1], 右小段 ob: check dictionary. false  
3. 左大段 bo = M[2], 右小段 b: check dictionary. false  

M[4] = false input[4] = bobc  
Four cases  
1. No cut, false  
2. 左大段 b = M[1], 右小段 obc: check dictionary. false  
3. 左大段 bo = M[2], 右小段 bc: check dictionary. false  
4. 左大段 bob = M[3], 右小段 c: check dictionary. false  

左大段右大段行不通，因为右小段无历史记录，所以用左大段右小段。  

Time: O(n^3), two for loop and s.substring()   
Space: O(n)  
```
M[] = int[n+1];
M[0] = false; 
for(1<= 1 < n) {
  if (dict.contains(s.substring(0, i))
    M[i] = true;
    continue;
  
  for(1<= j < i) {
    if (M[j] && s.substring(j, i))
      M[i] = true;
      break;
  }
}
return M[n-1];
```
### How to optimize time complexity? 
Avoid using substring().  
```
```

## Example 5: Edit Distance
Given two strings of alphaumeric characters, determine the minimum number of Replace, Delete, and Insert operations needed to transform one string into the other.  

Example: s1="asdf", s2="sghj"
