# 3942. Minimum Operations to Sort a Permutation

# Problem Summary:

Given a permutation nums of the integers from 0 to n - 1, one operation allows either reversing the entire array or rotating it left by one position.

Return the minimum number of operations needed to sort the array in increasing order, or -1 if it is impossible.

# Approach Used:

Find the index of 0, then traverse the array circularly from that position to check whether the elements follow either the increasing or decreasing pattern.

1. If the circular sequence is neither increasing nor decreasing, return -1.
2. If it is increasing, return the minimum of:
   - start: the number of left rotations needed.
   - n - start + 2: the number of operations using one reversal and rotations.
3. If it is decreasing, return the minimum of:
   - n - start: the number of rotations needed after accounting for the reverse orientation.
   - start + 2: the number of operations using one reversal and rotations.

# Steps:

1. If the array has only one element, return 0.
2. Find the index start where the value 0 occurs.
3. Initialize asc and desc to true.
4. Traverse the array circularly, starting from the element after 0.
5. Check whether each value matches the expected increasing value (i) and decreasing value (n - i).
6. If both patterns fail, return -1.
7. If the sequence is increasing, return min(start, n - start + 2).
8. Otherwise, return min(n - start, start + 2).

# Solution:

```
class Solution {
    public int minOperations(int[] nums) {
        final int n = nums.length;                                           // T(n) = O(1), S(n) = O(1)

        if(n == 1) {                                                         // T(n) = O(1)
            return 0;                                                        // T(n) = O(1)
        }

        int start = 0;                                                       // T(n) = O(1), S(n) = O(1)

        while(nums[start] != 0) {                                            // T(n) = O(n)
            start++;                                                         // T(n) = O(1)
        }

        boolean asc = true;                                                  // T(n) = O(1), S(n) = O(1)
        boolean desc = true;                                                 // T(n) = O(1), S(n) = O(1)

        for(int i = 1; i < n; i++) {                                         // T(n) = O(n)
            final int value = nums[(start + i) % n];                         // T(n) = O(1), S(n) = O(1)

            if(value != i) {                                                 // T(n) = O(1)
                asc = false;                                                 // T(n) = O(1)
            }
            if(value != n - i) {                                             // T(n) = O(1)
                desc = false;                                                // T(n) = O(1)
            }

            if(!asc && !desc) {                                              // T(n) = O(1)
                return -1;                                                   // T(n) = O(1)
            }
        }

        if(asc) {                                                            // T(n) = O(1)
            return Math.min(start, n - start + 2);                           // T(n) = O(1)
        }

        return Math.min(n - start, start + 2);                               // T(n) = O(1)
    }
}
```


# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
