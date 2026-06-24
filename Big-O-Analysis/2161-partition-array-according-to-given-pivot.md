# 2161. Partition Array According to Given Pivot

# Problem Summary:
Given an integer array nums and an integer pivot, rearrange the array such that:
1. All elements smaller than pivot appear before all elements greater than pivot.
2. All elements equal to pivot appear between them.
3. The relative order of elements smaller than pivot is preserved.
4. The relative order of elements greater than pivot is preserved.
Return the rearranged array.

# Approach Used:
The solution constructs the result array directly without using separate lists.
Two pointers are maintained:
* left points to the next available position from the beginning of the result array.
* right points to the next available position from the end of the result array.
The array is traversed simultaneously from both ends:
* Elements smaller than the pivot are placed from left to right using the left pointer.
* Elements greater than the pivot are placed from right to left using the right pointer.
Since larger elements are processed from right to left and inserted from right to left, their original relative order is preserved in the final array. After all smaller and larger elements have been placed, the remaining positions between left and right are filled with the pivot value.

# Steps:
1) Create an answer array of the same size as the input
2) Initialize:
   * left = 0
   * right = n - 1
3) Traverse the array simultaneously from both ends
4) For each iteration:
   * If the current left-side element is smaller than pivot, place it at index left and increment left
   * If the current right-side element is greater than pivot, place it at index right and decrement right
5) After the traversal, all remaining positions correspond to pivot values
6) Fill the remaining positions between left and right with pivot
7) Return the result array

# Solution:
```
class Solution {

    public int[] pivotArray(int[] nums, int pivot) {
        final int n = nums.length;                  // T(n) = O(1), S(n) = O(1)
        int[] ans = new int[n];                     // T(n) = O(1), S(n) = O(n)
        int left = 0;                               // T(n) = O(1), S(n) = O(1)
        int right = n - 1;                          // T(n) = O(1), S(n) = O(1)

        // Loop runs n times
        for(int i = 0, j = n - 1; i < n; i++, j--) {
            if(nums[i] < pivot) {
                ans[left++] = nums[i];             // T(n) = O(1) per execution
            }

            if(nums[j] > pivot) {
                ans[right--] = nums[j];            // T(n) = O(1) per execution
            }
        }

        // Runs at most n times
        while(left <= right) {
            ans[left++] = pivot;                   // T(n) = O(1) per execution
        }

        return ans;                                // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
