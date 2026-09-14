# 2948. Make Lexicographically Smallest Array by Swapping Elements

# Problem Summary:

You are given a 0-indexed array of positive integers nums and a positive integer limit.

You can swap two elements if the absolute difference between their values is at most limit. The goal is to return the lexicographically smallest array that can be obtained after performing the operation any number of times.

# Approach Used:

The solution sorts the elements while preserving their original indices by encoding each value and index into a long.

After sorting, elements whose consecutive values differ by at most limit form a group. Within each group, all elements can be rearranged among the original indices belonging to that group.

For each group, the original indices are sorted, and the group's values are assigned to those indices in ascending order. This produces the lexicographically smallest arrangement for that group.

# Steps:

1. Create a long[] sorted array where each element stores both the value from nums and its original index.
2. Sort sorted, which orders the elements primarily by value.
3. Extract the sorted values into the values array.
4. Traverse the sorted values and identify groups where every consecutive pair differs by at most limit.
5. For each group:
   * Collect the original indices of its elements.
   * Sort those indices.
   * Assign the group's values, already sorted in ascending order, to the sorted original indices.
6. Continue until all groups have been processed.
7. Return the modified nums array.

# Solution:

```
class Solution {
    public int[] lexicographicallySmallestArray(int[] nums, int limit) {
        int n = nums.length;                                                 // T(n) = O(1), S(n) = O(1)
        long[] sorted = new long[n];                                         // T(n) = O(n), S(n) = O(n)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n)
            sorted[i] = ((long) nums[i] << 32) | (i & 0xffffffffL);          // T(n) = O(1)
        }

        Arrays.sort(sorted);                                                 // T(n) = O(n log n)

        int[] values = new int[n];                                           // T(n) = O(n), S(n) = O(n)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n)
            values[i] = (int) (sorted[i] >>> 32);                            // T(n) = O(1)
        }

        int start = 0;                                                       // T(n) = O(1), S(n) = O(1)

        while(start < n) {                                                   // T(n) = O(n) overall
            int end = start;                                                 // T(n) = O(1), S(n) = O(1)

            while(end + 1 < n && values[end + 1] - values[end] <= limit) {   // T(n) = O(n) overall
                end++;                                                       // T(n) = O(1)
            }

            int size = end - start + 1;                                      // T(n) = O(1), S(n) = O(1)
            int[] indices = new int[size];                                   // T(n) = O(size), S(n) = O(size)

            for(int i = 0; i < size; i++) {                                  // T(n) = O(size)
                indices[i] = (int) sorted[start + i];                        // T(n) = O(1)
            }

            Arrays.sort(indices);                                            // T(n) = O(size log size)

            for(int i = 0; i < size; i++) {                                  // T(n) = O(size)
                nums[indices[i]] = values[start + i];                        // T(n) = O(1), S(n) = O(1)
            }
            start = end + 1;                                                 // T(n) = O(1)
        }
        return nums;                                                         // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n log n)

# Space Complexity:
S(n) = O(n)
