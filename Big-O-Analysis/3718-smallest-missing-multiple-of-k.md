# 3718. Smallest Missing Multiple of K

# Problem Summary:

You are given an integer array nums and an integer k. Return the smallest positive multiple of k that is missing from nums. A multiple of k is any positive integer divisible by k.

# Approach Used:

The solution uses a boolean array to record which numbers from nums are present.

Since the solution only needs to check values below 101, it creates a boolean array of size 101. For every number in nums, the corresponding index is marked as true.

Then, starting from k, it checks every positive multiple of k below 101. The first multiple whose corresponding position is false is the smallest missing multiple, so it is returned.

If all multiples below 101 are present, the loop ends and the next multiple of k is returned.

# Steps:

1. Create a boolean array found of size 101 to track which numbers are present in nums.
2. Traverse nums and mark found[num] as true for every number.
3. Start checking multiples of k from k.
4. For each multiple:
   * If it has not been found in nums, return it.
   * Otherwise, move to the next multiple by adding k.
5. If all multiples below 101 are present, return the first multiple that is at least 101.

# Solution:

```
class Solution {
    public int missingMultiple(int[] nums, int k) {
        boolean[] found = new boolean[101];                                  // T(n) = O(1), S(n) = O(1)

        for(int num : nums) {                                                // T(n) = O(n)
            found[num] = true;                                               // T(n) = O(1), S(n) = O(1)
        }
        int i = k;                                                           // T(n) = O(1), S(n) = O(1)

        while(i < 101) {                                                     // T(n) = O(101 / k) = O(1)
            if(!found[i]) { 
                return i;                                                    // T(n) = O(1)
            }
            i += k;                                                          // T(n) = O(1) per iteration
        }
        return i;                                                            // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
