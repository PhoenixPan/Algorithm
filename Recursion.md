# Recursion

1. 表面上: A function that calls itself 
2. 实质上: Boil down a big problem to smaller ones  
3. 实现上: 
    1. Base case: smallest problem to resolve
    2. Recursive rule: how to make the problem smaller  

## Example 1: Fibonacci 
1. Base case: F(0) = 0, F(1) = 1
2. Recursive rule: F(n) = F(n-1) + F(n-2)
```java
public int fibonacci(int n)
  if (n == 0)
    return 0;
  else if (n == 1)
    return 1;
  else 
    return fibonacci(n-1) + fibonacci(n-2);
```
