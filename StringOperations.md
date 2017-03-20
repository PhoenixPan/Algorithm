## String match


1. Naive method: For every position in the string, consider it a starting position of the pattern and see if we get a match. Worst case: O(nm), where n is the length of the pattern and m is the length of the string. In natural language processing, the naive method gives decent performance, since breakpoint is usually easy to identify.
2. Rabin-Karp Algorithm (RK): 
