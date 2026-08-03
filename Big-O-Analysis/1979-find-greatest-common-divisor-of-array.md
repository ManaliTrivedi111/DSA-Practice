# 1979. Find Greatest Common Divisor of Array

# Problem Summary:
You are given an integer array nums.
Find:
* The smallest element in the array.
* The largest element in the array.

Return the greatest common divisor (GCD) of these two numbers. The GCD of two integers is the largest positive integer that divides both numbers without leaving a remainder.

# Approach Used:
The solution first traverses the array once to find the minimum and maximum values. Once these values are known, it applies the Euclidean Algorithm to compute their GCD efficiently.

# Steps:
1. Initialize variables to store the minimum and maximum values.
2. Traverse the array:
   * Update the minimum value.
   * Update the maximum value.
3. Compute the GCD of the minimum and maximum values using the Euclidean Algorithm.
4. Return the result.

# Solution:

```
class Solution {

    public int findGCD(int[] nums) {
        int minNum = 1001;                                    // T(n) = O(1), S(n) = O(1)
        int maxNum = 0;                                       // T(n) = O(1), S(n) = O(1)

        for(int num : nums) {
            minNum = Math.min(minNum, num);                   // T(n) = O(1)
            maxNum = Math.max(maxNum, num);                   // T(n) = O(1)
        }
        return gcd(minNum, maxNum);                           // T(n) = O(log M)
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);                    // T(n) = O(log M)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
