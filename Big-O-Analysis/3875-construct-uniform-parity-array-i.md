# 3875. Construct Uniform Parity Array I

# Problem Summary:

You are given an array nums1 of n distinct integers.

You need to construct another array nums2 of length n such that all elements in nums2 have the same parity, meaning they are either all odd or all even.

For each index i, exactly one of the following choices can be made:
* nums2[i] = nums1[i]
* nums2[i] = nums1[i] - nums1[j], where j != i

The order of the choices does not matter.

The task is to return true if it is possible to construct such an array, otherwise return false.

# Approach Used:

The given solution simply returns true. This is correct because the allowed operation always makes it possible to construct a uniform-parity array.

Let us consider the parity of the elements in nums1.

If all elements already have the same parity, we can choose: nums2[i] = nums1[i] for every index, so nums2 is already uniform in parity.

If nums1 contains both odd and even values, choose one element as a reference.

For every element whose parity is different from the reference element, subtract the reference element. The difference between an odd and an even number is always odd.

For an element whose parity is the same as the reference element, we can simply keep the original value, which has the reference parity.

Therefore, a valid nums2 can always be constructed, regardless of the values in nums1, as long as the elements are distinct as specified by the problem.

Because of this, no inspection of the array is required for this implementation.

# Steps:

1. The method receives nums1.
2. No elements need to be examined.
3. Since a valid uniform-parity nums2 can always be constructed under the allowed operations, return true.

# Solution:

```
class Solution {
    public boolean uniformArray(int[] nums1) {
        return true;                                                         // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(1)

# Space Complexity:
S(n) = O(1)
