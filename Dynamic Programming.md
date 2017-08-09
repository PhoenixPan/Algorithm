## Steps
1. Base case
When weight of each smallest element in the original data is identical or similar, we can usually use liner scan and look back to the previous result. 

2. Induction rule f(n)
```
s[0] = ? // usually doesn't matter
s[1] = ? // start to give you the induction rule, find the relationship between s[0] and s[1]
...
s[n] = global solution
```
3. English expression of the meaning
4. Coding

## Example
**1. Give a rope with integer length n, how to cut the rope into m integer length parts with length p[0], p[1], ..., p[m-1] in order to get the maximal product of p[0] * p[1] * ... * p[m-1]? m is determined by you and must be greater than 0 (at least one cut must be made)**

### 1. Base case: 
M[1], can't be cut

### 2. Induction rule
#### Method 1
M[2] = max(1, M[1]) * max(1, M[1])  --|--  

M[3] = max(1, M[1]) * max(2, M[2])  --|-- --  
M[3] = max(2, M[2]) * max(1, M[1])  -- --|--  

M[4] = max(1, M[1]) * max(3, M[3])  --|-- -- --  
M[4] = max(2, M[2]) * max(2, M[2])  -- --|-- --  
M[4] = max(3, M[3]) * max(1, M[1])  -- -- --|--  

We only consider one cut, other cuts will be performed in max(n, M[n]).  

#### Method 2

### 3. English expression of the meaning (skipped)
### 4. Code
#### Method 1
Time: O(n^2)  
Space: O(n)  
```java
int cutRope(int n) {
    if(n < 2) return -1; // n has to be at least 2, as one cut has to be made

    int max = 1;
    int[] record = new int[n];

    for(int i = 1; i < n; i++) {
      int temp = i;
      for (int j = 1; j < i; j++) {
        temp = Math.max(
            Math.max(j, record[j]) * Math.max(i - j, record[i-j])
            , temp);
      }
      record[i] = temp;
      max = Math.max(max, temp);
    }
    return max;
  }
```

### Optimization
1-9 and 9-1 are the same

## Key words
Iteration and recursion, sub-array and sub-sequence
