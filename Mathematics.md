## Euclidean Algorithm for Greatest Common Divisor (GCD)  
gcd(a, b) = gcd(a, b − a)  
  
Example:  
  gcd(1989, 867) = gcd(1989 − 2 × 867, 867)  
= gcd(255, 867)  
= gcd(255, 102)  
= gcd(51, 102)  
= gcd(51, 0)  
= 51  
  
Running time: O(log(a + b))  
  
## Modular Inverse:
a x ≡ 1 (mod m)

Input:  a = 3, m = 11
Output: 4
Since (4*3) mod 11 = 1, 4 is modulo inverse of 3
One might think, 15 also as a valid output as "(15*3) mod 11" 
is also 1, but 15 is not in ring {0, 1, 2, ... 10}, so not 
valid.

Input:  a = 10, m = 17
Output: 12
Since (10*12) mod 17 = 1, 12 is modulo inverse of 3

