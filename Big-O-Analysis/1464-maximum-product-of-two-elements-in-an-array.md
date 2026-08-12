# 1464. Maximum Product of Two Elements in an Array

# Problem Summary:

You are given an integer array nums. You need to choose two different elements from the array and return the maximum value of: (nums[i] - 1) * (nums[j] - 1)

# Approach Used:

Since subtracting 1 from a larger number still produces a larger value, the maximum product is obtained by choosing the two largest elements in the array. Therefore, we only need to keep track of:
* max1: the largest element found so far.
* max2: the second-largest element found so far.

For every number in the array:
* If it is larger than max1, the old max1 becomes max2, and the current number becomes max1.
* Otherwise, if it is larger than max2, it becomes max2.

After finding the two largest elements, we return: (max1 - 1) * (max2 - 1)

# Steps:

1. Initialize max1 and max2 to -1.
2. Traverse every element in nums.
3. If the current element is greater than max1, move max1 to max2 and make the current element max1.
4. Otherwise, if the current element is greater than max2, update max2.
5. Return (max1 - 1) * (max2 - 1).

# Solution:

```
class Solution {
    public int maxProduct(int[] nums) {
        int max1 = -1;                                                     // T(n) = O(1), S(n) = O(1)
        int max2 = -1;                                                     // T(n) = O(1), S(n) = O(1)

        for(int num : nums) {
            if(num > max1) {
                max2 = max1;                                              // T(n) = O(1)
                max1 = num;                                               // T(n) = O(1)
            }else if(num > max2) {
                max2 = num;                                               // T(n) = O(1)
            }
        }

        return (max1 - 1) * (max2 - 1);                                  // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
