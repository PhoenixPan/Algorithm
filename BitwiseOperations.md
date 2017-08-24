# Bit representation and operations

## Bit representation
### Integer representation  
  int index = 1;  
  0000 0000 0000 0000 0000 0000 0000 0000 0001  
  integer has 4 bytes = 32 bits (1 byte = 8 bit)   
  The first digit is sign bit (positive or negative) so the maximum number we can represent is:   
  [sign] 2^0 + 2^1 + 2^2 + ... + 2^29 + 2^30 = 2^31     

### Ordinary binary representation 源码表示法
The leading 1 tells the number is negative  
"+1" 0000 0000 0000 0000 0000 0000 0000 0000 0001  
"-1" 1000 0000 0000 0000 0000 0000 0000 0000 0001  

However it is not good! Why? because we will have a positive zero and a negative zero!  
"+0" 0000 0000 0000 0000 0000 0000 0000 0000 0000  
"-0" 1000 0000 0000 0000 0000 0000 0000 0000 0000  


### Two's complement 补码表示法  
Positive numbers are the same as ordinary binary, only negative number has two's complement, whose value is the flip of positive number then plus one.  

For example:  
0000 0101  (binary 5)  
↓ flip  
1111 1010  (one's completement of 5)   
↓ add one  
1111 1011  (two's completement of -5)  

Another example:  
0000 0000 (0)  
↓ flip  
1111 1111 (0)  
↓ add one  
overflow, no negative 0   

### int range\[-2^31, 2^31-1\]: Why negative has one more value, -2^31?  
(1)  
0000 0001 (positive 1)  
1111 1111 (negative 1)  
(2)  
2^31-1:  0111 1111 1111 1111 1111 1111 1111 1111 1111 (the largest positive)  
-2^31+1: 1000 0000 0000 0000 0000 0000 0000 0000 0001 (corresponding largest negative)  
-2^31:   1000 0000 0000 0000 0000 0000 0000 0000 0000 (the additional negative number)  

![minimum](https://cloud.githubusercontent.com/assets/14355257/20159085/7c3d3382-a6ac-11e6-8db7-728e722cadf9.png)  

### Unsigned integers
1. Unsigned can hold a larger non-negative value (0 and positive).
2. Unsigned uses the leading bit as a part of the value. 
3. Signed integers can hold both positive and negative numbers.
Check Leetcode question 191  

### Summary
[+1] = [0000 0001]原码 = [0000 0001]反码 = [0000 0001]补码  
[-1] = [1000 0001]原码 = [1111 1110]反码 = [1111 1111]补码  

One's and Two's complement are not for humans to calculate. We usually need to change it back to original binary to calculate them. 

##### References:  
https://www.cnblogs.com/zhangziqiu/archive/2011/03/30/ComputerCode.html  
http://blog.csdn.net/morewindows/article/details/7354571  

## Bit operations
Only for integers, not float or double.  

Operator | Name | Result
--- | --- | ---
& | AND 与 | 0101 & 0110 = 0100
\| | OR 或 | 0101 \| 0110 = 0111
^ | XOR 异或 | 0101 ^ 0110 = 1100
~ | NOT 取反 | ~1000 = 0111


### Left shift (<<) 
Simply move left and append 0s at right, 补零  

int | 15 | -15 
--- | --- | --- 
Original | 0000 1111 | 1000 1111
One's | 0000 1111 | 1111 0000
Two's | 0000 1111 | 1111 0001
<< 2 | 0011 1100 | 1100 0100
One's | 0011 1100 | 1100 0011
Original | 0011 1100 | 1011 1100
int | 60 | -60

### Right shift (>>)  
Move right and append sign bit at left, 算术移位的高位是补符号位

int | 15 | -15 
--- | --- | --- 
Original | 0000 1111 | 1000 1111
One's | 0000 1111 | 1111 0000
Two's | 0000 1111 | 1111 0001
\>> 2 | 0000 0011 | 1111 1100
One's | 0000 0011 | 1111 1011
Original | 0000 0011 | 1000 0100
int | 3 | -4

### Unsigned right shift (>>>)  
Move right and append a 0 at left, ignore sign bit, 逻辑移位的高位补0  
Notice: for this operation, the bit sign will be counted towards a regular bit, which may make a negative number a positive integer 


## Practice
### Q1.How to determine an integer i is an odd number?  
Odd: XXXX XXX1 & 0000 0001 = 0000 0001 = 1  
Even: XXXX XXX0 & 0000 0001 = 0000 0000 = 0  
(true = 1, false = 0)  
```
return (i & 1) == 1? true : false
```  

### Q2.Given a number x, how to set x's k-th bit to 1?  
```
int change(int x, int k) {
    return x | (1<<k);    
}
```

### Q3.How to verify whether a number is power of two (n>=0)? 
int x = 0000 1000 has only one 1  
x-1 = 0000 0111   
```
return (x & (x - 1) == 0 && x != 0)
```

### Q4.Whether letters in a word is unique?  
//

### Other applications  
Use an integer to replace an array of booleans to represent situation:   
true true false false -> 1 1 0 0    
Three situations?   
Use two bits to represent three (more than two) situation:  
1 2 3 -> 00 01 10   


##### References:  
http://blog.csdn.net/morewindows/article/details/7354571
