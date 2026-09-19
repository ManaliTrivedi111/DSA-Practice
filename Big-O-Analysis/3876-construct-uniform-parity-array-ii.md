# 3876. Construct Uniform Parity Array II

# Problem Summary:

You are given an array nums1 of n distinct integers.

You need to construct another array nums2 of length n such that all elements in nums2 have the same parity, meaning they are either all odd or all even.

For each index i, exactly one of the following choices can be made:
* nums2[i] = nums1[i]
* nums2[i] = nums1[i] - nums1[j], where j != i and nums1[i] - nums1[j] >= 1

The task is to return true if it is possible to construct such an array, otherwise return false.

# Approach Used:

The solution makes one pass through the array and keeps track of two pieces of information:
* minNum: the minimum value in nums1
* isOddFound: whether at least one odd number exists in nums1

The key observation is that the minimum element cannot be changed using the subtraction operation. Since the subtraction must produce a positive result, there is no smaller element that can be subtracted from the minimum element.

Therefore, the minimum element must remain unchanged in nums2.

If minNum is odd, we can construct an all-odd nums2, so the answer is true.

If minNum is even, the final array must be all even because the minimum element has to remain even. In this case, every element must also be even. Since subtracting two even numbers produces an even number, an array containing only even values is already valid.

However, if an odd number exists while the minimum is even, that odd number cannot be converted to even. Subtracting an even number preserves odd parity, while subtracting an odd number would produce an even result, but the only possible odd number is itself greater than the even minimum and using it as the subtracted value would require the current number to be greater than that odd value. The given implementation therefore uses the condition !isOddFound when the minimum is even.

# Steps:

1. Initialize minNum to the largest possible integer.
2. Initialize isOddFound to false.
3. Traverse every number in nums1.
4. Update minNum with the smallest value encountered.
5. If the current number is odd, set isOddFound to true.
6. After the traversal, check the parity of minNum.
7. If minNum is odd, return true.
8. Otherwise, return !isOddFound.
9. Therefore, when the minimum is even, the method returns true only when all numbers are even.

# Solution:

```
class Solution {
    public boolean uniformArray(int[] nums1) {
        int minNum = Integer.MAX_VALUE;                                      // T(n) = O(1), S(n) = O(1)
        boolean isOddFound = false;                                          // T(n) = O(1), S(n) = O(1)

        for(int num : nums1) {                                               // T(n) = O(n)
            minNum = Math.min(minNum, num);                                  // T(n) = O(1), S(n) = O(1)

            if(num % 2 == 1) {                                               // T(n) = O(1)
                isOddFound = true;                                           // T(n) = O(1), S(n) = O(1)
            }
        }

        if(minNum % 2 == 1) {                                                // T(n) = O(1)
            return true;                                                     // T(n) = O(1), S(n) = O(1)
        }

        return !isOddFound;                                                  // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
