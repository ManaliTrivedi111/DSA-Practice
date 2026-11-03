# 2778. Sum of Squares of Special Elements

# Problem Summary

Given a 1-indexed integer array nums of length n, an element nums[i] is special if its 1-based index i divides n.

Return the sum of the squares of all special elements.

# Approach Used

The solution iterates through every element of the array.

Because the array is 0-indexed in Java, the 1-based index of nums[i] is i + 1.

For each element, the solution checks whether n % (i + 1) == 0. If so, the element is special, and its square is added to sum.

# Steps

1. Store the array length in n.
2. Initialize sum to 0.
3. Traverse every index i from 0 to n - 1.
4. Convert the 0-based index i to the required 1-based index using i + 1.
5. Check whether i + 1 divides n.
6. If it does, add nums[i] * nums[i] to sum.
7. Return sum.

# Solution

```
class Solution {
   public int sumOfSquares(int[] nums) {
       final int n = nums.length;                                            // T(n) = O(1), S(n) = O(1)
       int sum = 0;                                                          // T(n) = O(1), S(n) = O(1)

       for(int i = 0; i < n; i++) {                                          // T(n) = O(n)
           if(n % (i + 1) == 0) {                                            // T(n) = O(1)
               sum += nums[i] * nums[i];                                     // T(n) = O(1)
           }
       }
       return sum;                                                           // T(n) = O(1)
   }
}
```

# Time Complexity
T(n) = O(n)

# Space Complexity
S(n) = O(1)

