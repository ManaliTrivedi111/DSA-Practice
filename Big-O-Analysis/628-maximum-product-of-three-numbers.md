# 628. Maximum Product of Three Numbers

# Problem Summary:

You are given an integer array nums. Return the maximum product that can be obtained by choosing any three numbers from nums.

# Approach Used:

The maximum product of three numbers can come from one of two possibilities:
* The three largest numbers:max1 * max2 * max3
* The two smallest numbers and the largest number:min1 * min2 * max1

The second possibility is important because multiplying two negative numbers produces a positive number. For example, with [-10, -9, 5], the product is (-10) * (-9) * 5 = 450. Therefore, while traversing the array once, the solution keeps track of the two smallest numbers and the three largest numbers.

# Steps:

1. Initialize min1 and min2 to track the two smallest numbers.
2. Initialize max1, max2, and max3 to track the three largest numbers.
3. Traverse every number in nums.
4. Update min1 and min2 when a smaller number is found.
5. Update max1, max2, and max3 when a larger number is found.
6. Calculate min1 * min2 * max1.
7. Calculate max1 * max2 * max3.
8. Return the larger of the two products.

# Solution:

```
class Solution {
    public int maximumProduct(int[] nums) {
        int min1 = 1001;                                                   // T(n) = O(1), S(n) = O(1)
        int min2 = 1001;                                                   // T(n) = O(1), S(n) = O(1)
        int max1 = -1001;                                                  // T(n) = O(1), S(n) = O(1)
        int max2 = -1001;                                                  // T(n) = O(1), S(n) = O(1)
        int max3 = -1001;                                                  // T(n) = O(1), S(n) = O(1)

        for(int num : nums) {
            if(num < min1) {
                min2 = min1;                                               // T(n) = O(1)
                min1 = num;                                                // T(n) = O(1)
            }else if(num < min2) {
                min2 = num;                                                // T(n) = O(1)
            }

            if(num > max1) {
                max3 = max2;                                               // T(n) = O(1)
                max2 = max1;                                               // T(n) = O(1)
                max1 = num;                                                // T(n) = O(1)
            }else if(num > max2) {
                max3 = max2;                                               // T(n) = O(1)
                max2 = num;                                                // T(n) = O(1)
            }else if(num > max3) {
                max3 = num;                                                // T(n) = O(1)
            }
        }

        return Math.max(min1 * min2 * max1,                                // T(n) = O(1)
                        max1 * max2 * max3);                               // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
