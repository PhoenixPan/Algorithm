## Random number without duplication
1. Use hashmap to exclude element that has already been chosen (Getting very slow when the number of random figures approach the size of the hashmap)
2. Remove element from the array when chosen (Slow in most cases due to remove method O(n) each removal)
3. Swap the random element (from i to n) with the ith element. The first n elements will be the random numbers we need (efficient, O(n) at all) 
    ```
    // JS code
    function randomIds(range) {

        var randomIds = [];
        
        // populate an array of numbers from 0 to range
        for (i = 0; i < range; i++) {
            randomIds.push(i);
        }
        
        // randomize all indexes
        for (j = 0; j < range; j++) {
            var index = Math.floor(Math.random() * (range - j) + j);
            var temp = randomIds[j];
            randomIds[j] = randomIds[index];
            randomIds[index] = temp;
        }
        return randomIds;
    }
    ```
