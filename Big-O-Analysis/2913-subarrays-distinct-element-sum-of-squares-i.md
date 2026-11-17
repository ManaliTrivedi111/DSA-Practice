# 2913. Subarrays Distinct Element Sum of Squares I

# Problem Summary:

Given an integer array nums, consider every non-empty contiguous subarray. For each subarray, calculate the number of distinct elements and add the square of that count to the result.

Return the sum of these squared distinct counts.

# Approach Used:

Use two nested loops to generate every possible subarray.

For each starting index i, create a HashSet to keep track of the distinct elements in the current subarray. Extend the subarray one element at a time by moving j from i to the end of the array.

Add each new element to the set. Since a set stores only distinct elements, s.size() gives the distinct count of the current subarray. Add its square to res.

# Steps:

1. Get the length of nums and initialize res to zero.
2. Iterate through every possible starting index i.
3. For each starting index, create an empty HashSet.
4. Iterate through each possible ending index j, starting from i.
5. Add nums.get(j) to the set.
6. Add the square of the set's size to res.
7. Return res after processing all subarrays.

# Solution:

```
class Solution {
    public int sumCounts(List<Integer> nums) {
        int n = nums.size();                                                 // T(n) = O(1), S(n) = O(1)
        int res = 0;                                                         // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n^2) across all iterations
            Set<Integer> s = new HashSet<>();                                // T(n) = O(1), S(n) = O(n) at most

            for(int j = i; j < n; j++) {                                     // T(n) = O(n) for each i
                s.add(nums.get(j));                                          // T(n) = O(1)
                res += s.size() * s.size();                                  // T(n) = O(1)
            }
        }
        return res;                                                          // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n^2) expected time.

# Space Complexity:
S(n) = O(n)
