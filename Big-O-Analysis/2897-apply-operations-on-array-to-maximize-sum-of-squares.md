# 2897. Apply Operations on Array to Maximize Sum of Squares

# Problem Summary

Given an integer array nums and an integer k, perform the allowed bitwise AND/OR operations any number of times. Then choose k elements from the resulting array to maximize the sum of their squares. Return the maximum sum modulo 10^9 + 7.

# Approach Used

Count how many numbers in nums have each of the 32 bit positions set.

To construct each of the k numbers, iterate through all 32 bit positions. If a bit is still available (its count is greater than zero), include that bit in the current number and decrease its count.

This greedily creates the largest possible numbers by combining the available set bits. Add the square of each constructed number to the answer, taking the result modulo 10^9 + 7.

# Steps

1. Create a count array of size 32 to store the number of set bits at each bit position.
2. Traverse every number in nums and update the count for each of its 32 bit positions.
3. Repeat k times to construct k numbers:
   - Start with the current number c = 0.
   - Traverse all 32 bit positions.
   - If a bit is available, set it in c and decrement its count.
   - Add c * c to the answer, modulo 10^9 + 7.
4. Return the answer.

# Solution

```
class Solution {
    public int maxSum(List<Integer> nums, int k) {
        int[] count = new int[32];                                           // T(n) = O(1), S(n) = O(1)
        int mod = 1_000_000_007;                                             // T(n) = O(1), S(n) = O(1)

        for(int num : nums){                                                 // T(n) = O(n * 32)
            for(int i = 0; i < 32; i++) {                                    
                count[i] += num >> i & 1;                                    // T(n) = O(1)
            }
        }

        long ans = 0;                                                        // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < k; i++){                                          // T(n) = O(k * 32)
            int c = 0;                                                       // T(n) = O(1), S(n) = O(1)

            for(int j = 0; j < 32; j++){                                     
                if(count[j] == 0) {                                          // T(n) = O(1)
                    continue;
                }
                c |= 1 << j;                                                 // T(n) = O(1)
                --count[j];                                                  // T(n) = O(1)
            }
            ans = (ans + 1L * c * c) % mod;                                  // T(n) = O(1)
        }
        return (int) ans;                                                    // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n, k) = O(n + k), where n = number of elements in nums, and k = number of elements to select from the final array 

# Space Complexity:
S(n, k) = O(1)
