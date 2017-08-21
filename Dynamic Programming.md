# Dynamic Programming
## Strategies  
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
Time: O(nk)   k is the largest input[i]
Space: O(n)  


### Variation: Minimum number of jumps
**return the minimum number of jumps needed to reach the end?**

**1. Base case**: M[n-1] = 0, so it can jump out  
**2. Induction rule**:
Arrays.fill(M, Integer.MAX_VALUE);  
If there's a j <= input[i] so that M[i+j] = true, M[i] = Math.min(M[i], M[i+j] + 1)     
return M[0]  


Time: O(n\*k)   
Space: O(n)  
```
M[n-1] = 0
M[i] = 1 + min(M[i+j])  // j <= input[i]
```

## Example 3: Longest ascending subarray
### Followup 1: Space O(1)
### Followup 2: Get the head and tail index of the subarray 
