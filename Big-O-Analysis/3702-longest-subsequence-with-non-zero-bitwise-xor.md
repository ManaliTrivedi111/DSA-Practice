# 3702. Longest Subsequence With Non-Zero Bitwise XOR

# Problem Summary:

You are given an integer array nums. Return the length of the longest subsequence whose bitwise XOR is non-zero. If no such subsequence exists, return 0.

# Approach Used:

The solution is based on the fact that we want the longest possible subsequence.

First, we calculate the XOR of all elements in the array.

There are two cases:
* If the XOR of the entire array is non-zero, then the entire array itself is a valid subsequence. Since it contains all n elements, it is automatically the longest possible subsequence.
* If the XOR of the entire array is zero, we need to remove as few elements as possible to make the XOR non-zero.

If we remove one element num, the XOR of the remaining elements becomes: xor ^ num
Since the original xor is 0, this becomes: 0 ^ num = num
Therefore, if there is any non-zero element in the array, removing that one element produces a subsequence with non-zero XOR.
The resulting subsequence has length n - 1, which is the longest possible after the full array fails.

If every element is zero, removing any element still leaves XOR equal to zero. Therefore, no subsequence can have a non-zero XOR, and the answer is 0.

# Steps:

1. Calculate the XOR of all elements in nums.
2. If the total XOR is non-zero, return n.
3. Otherwise, traverse the array again.
4. For each element, check whether xor ^ num is non-zero.
5. If it is non-zero, removing that one element gives a valid subsequence of length n - 1.
6. If no such element exists, all elements must be zero, so return 0.

# Solution:

```
class Solution {
    public int longestSubsequence(int[] nums) {
        final int n = nums.length;                                           // T(n) = O(1), S(n) = O(1)
        int xor = 0;                                                         // T(n) = O(1), S(n) = O(1)

        for(int num : nums) {
            xor ^= num;                                                      // T(n) = O(1), S(n) = O(1)
        }

        if(xor != 0) {
            return n;                                                        // T(n) = O(1), S(n) = O(1)
        }else{
            for(int num : nums) {
                if((xor ^ num) != 0) {
                    return n - 1;                                            // T(n) = O(1), S(n) = O(1)
                }
            }

            return 0;                                                        // T(n) = O(1), S(n) = O(1)
        }
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
