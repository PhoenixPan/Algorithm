##Euclidean Algorithm for Greatest Common Divisor (GCD)  
gcd(a, b) = gcd(a, b − a)  
  
Example:  
  gcd(1989, 867) = gcd(1989 − 2 × 867, 867)  
= gcd(255, 867)  
= gcd(255, 102)  
= gcd(51, 102)  
= gcd(51, 0)  
= 51  
  
Running time: O(log(a + b))  
  
