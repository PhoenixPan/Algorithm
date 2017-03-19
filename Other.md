## Random number without duplication
1. Use hashmap to exclude element that has already been chosen (Getting very slow when the number of random figures approach the size of the hashmap)
2. Remove element from the array when chosen (Slow in most cases due to remove method O(n) each removal)
3. Swap the random element (from i to n) with the ith element. The first n elements will be the random numbers we need (efficient, O(n) at all) 
```
// JS code
function getRandom(need, range) {
    var input = [];
    for (i = 0; i < range; i++) {
        input.push(i);
    }
    for (j = 0; j < need; j++) {
        var index = Math.floor(Math.random() * (range - j) + j);
        var temp = input[j];
        input[j] = input[index];
        console.log(input[j]); // Print it out
        input[index] = temp;
    }
}
```
