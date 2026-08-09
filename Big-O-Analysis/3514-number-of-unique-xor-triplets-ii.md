# 3514. Number of Unique XOR Triplets II

# Problem Summary:

You are given an integer array nums. A XOR triplet is defined as: nums[i] XOR nums[j] XOR nums[k], where i <= j <= k.
Return the number of unique XOR triplet values that can be formed.

# Approach Used:

The solution breaks the XOR of three elements into two parts: 
1. nums[i] XOR nums[j]
2. nums[k]

Since XOR is associative, we can write: 'nums[i] XOR nums[j] XOR nums[k]' as '(nums[i] XOR nums[j]) XOR nums[k]'.

Therefore, the solution first finds all unique XOR values that can be obtained from every pair (i, j) where i <= j. A boolean array of size 2048 is used because the possible XOR values are bounded by 2047.
After finding all unique pair-XOR values, the solution stores them in an array. It then combines every unique pair-XOR value with every element of nums and marks the resulting three-element XOR value in the boolean array.

Finally, it counts how many XOR values were marked.

# Steps:

1. Create a boolean array freq to record which pair-XOR values have been seen.
2. Generate nums[i] XOR nums[j] for every pair where i <= j.
3. Store each unique pair-XOR value in freq and count how many unique values exist.
4. Copy all unique pair-XOR values into arr.
5. Reset freq.
6. For every unique pair-XOR value and every element of nums, calculate:
   arr[i] XOR nums[j]
7. Mark every resulting value in freq.
8. Count the marked values and return the count.

# Solution:

```
class Solution {
    public int uniqueXorTriplets(int[] nums) {
        final int n = nums.length;                                      // T(n) = O(1), S(n) = O(1)
        boolean[] freq = new boolean[2048];                             // T(n) = O(1), S(n) = O(1)
        int len = 0;                                                    // T(n) = O(1), S(n) = O(1)
        int idx = 0;                                                    // T(n) = O(1), S(n) = O(1)
        int ans = 0;                                                    // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {
            for(int j = i; j < n; j++) {
                int xor = nums[i] ^ nums[j];                            // T(n) = O(1)

                if(!freq[xor]) {
                    len++;                                              // T(n) = O(1)
                    freq[xor] = true;                                   // T(n) = O(1)
                }
            }
        }

        int[] arr = new int[len];                                       // T(n) = O(1), S(n) = O(len)

        for(int i = 0; i < 2048; i++) {
            if(freq[i]) {
                arr[idx++] = i;                                         // T(n) = O(1)
            }
        }

        Arrays.fill(freq, false);                                       // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < len; i++) {
            for(int j = 0; j < n; j++) {
                freq[arr[i] ^ nums[j]] = true;                          // T(n) = O(1)
            }
        }

        for(boolean b : freq) {
            if(b) {
                ans++;                                                  // T(n) = O(1)
            }
        }

        return ans;                                                     // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n^2)

# Space Complexity:
S(n) = O(1)
