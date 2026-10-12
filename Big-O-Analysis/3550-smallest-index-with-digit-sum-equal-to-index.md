# 3550. Smallest Index With Digit Sum Equal to Index

# Problem Summary

You are given an integer array nums. The goal is to find the smallest index i such that the sum of the digits of nums[i] is equal to i. If no such index exists, return -1.

# Approach Used

Traverse the array from left to right. For each index i, calculate the sum of the digits of nums[i] by repeatedly taking the last digit using % 10 and removing the last digit using /= 10.

As soon as the digit sum equals the current index, return that index. Since the array is traversed from the smallest index to the largest, the first matching index is guaranteed to be the smallest.

If no index satisfies the condition, return -1.

# Steps

1. Store the length of the array in n.
2. Iterate through the array from index 0 to n - 1.
3. For each index i, initialize sum to 0 and copy nums[i] into num.
4. Repeatedly extract the last digit of num using num % 10 and add it to sum.
5. Remove the last digit using num /= 10.
6. After all digits have been processed, check whether sum == i.
7. If they are equal, return i immediately.
8. If no matching index is found, return -1.

# Solution

```
class Solution {
    public int smallestIndex(int[] nums) {
        final int n = nums.length;                                           // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n)
            int sum = 0;                                                     // T(n) = O(1), S(n) = O(1)
            int num = nums[i];                                               // T(n) = O(1), S(n) = O(1)

            while(num > 0) {                                                 // T(n) = O(d) for one element
                sum += num % 10;                                             // T(n) = O(1), S(n) = O(1)
                num /= 10;                                                   // T(n) = O(1), S(n) = O(1)
            }

            if(sum == i) {                                                   // T(n) = O(1), S(n) = O(1)
                return i;                                                    // T(n) = O(1), S(n) = O(1)
            }
        }
        return -1;                                                           // T(n) = O(1), S(n) = O(1)
    }
}


# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
