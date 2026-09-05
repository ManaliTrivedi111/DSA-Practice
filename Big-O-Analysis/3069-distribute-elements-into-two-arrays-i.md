# 3069. Distribute Elements Into Two Arrays I

# Problem Summary:

You are given a 1-indexed array of distinct integers nums.

The elements must be distributed between two arrays, arr1 and arr2:
* The first element goes to arr1.
* The second element goes to arr2.
* For every remaining element, compare the last elements of arr1 and arr2:
  * If the last element of arr1 is greater, add the current element to arr1.
  * Otherwise, add it to arr2.

Finally, concatenate arr1 and arr2 to form the result.

# Approach Used:

The solution uses a two-pass approach with a single result array.

In the first pass, it simulates the distribution process using only the last elements of the two arrays:
* first represents the current last element of arr1.
* sec represents the current last element of arr2.
* cntFirst keeps track of how many elements will eventually belong to arr1.

After this pass, we know exactly where arr2 should begin in the final result array: at index cntFirst.

In the second pass, the solution reconstructs the two arrays directly inside ans:
* i points to the current last position of arr1 in ans.
* j points to the current last position of arr2 in ans.
* j starts at cntFirst, because arr2 begins immediately after all elements of arr1.

This allows the solution to produce the required concatenated result without creating separate arrays.

# Steps:

1. Store the length of nums in n.
2. Create the result array ans of length n.
3. Initialize first with nums[0], representing the last element of arr1.
4. Initialize sec with nums[1], representing the last element of arr2.
5. Initialize cntFirst to 1, since arr1 initially contains one element.
6. Perform the first pass from index 2 to n - 1 to determine how many elements will belong to arr1.
7. If first > sec, the current element goes to arr1, so update first and increment cntFirst.
8. Otherwise, the current element goes to arr2, so update sec.
9. Set i = 0, the initial position of the first element of arr1 in ans.
10. Set j = cntFirst, the starting position of arr2 in ans.
11. Place nums[0] at the beginning of arr1 and nums[1] at the beginning of arr2.
12. Perform a second pass over the remaining elements and place each element into the appropriate section of ans.
13. If the current last element of arr1 is greater than the current last element of arr2, advance i and place the element in arr1's section.
14. Otherwise, advance j and place the element in arr2's section.
15. Return ans.

# Solution:

```
class Solution {
    public int[] resultArray(int[] nums) {
        final int n = nums.length;                                           // T(n) = O(1), S(n) = O(1)
        int[] ans = new int[n];                                              // T(n) = O(n), S(n) = O(n)
        int first = nums[0];                                                 // T(n) = O(1), S(n) = O(1)
        int sec = nums[1];                                                   // T(n) = O(1), S(n) = O(1)
        int cntFirst = 1;                                                    // T(n) = O(1), S(n) = O(1)

        for(int i = 2; i < n; i++) { 
            if(first > sec) {                                              
                first = nums[i];                                             // T(n) = O(1) per execution
                cntFirst++;                                                  // T(n) = O(1) per execution
            }else{
                sec = nums[i];                                               // T(n) = O(1) per execution
            }
        }
        int i = 0;                                                           // T(n) = O(1), S(n) = O(1)
        int j = cntFirst;                                                    // T(n) = O(1), S(n) = O(1)
        ans[i] = nums[0];                                                    // T(n) = O(1), S(n) = O(1)
        ans[j] = nums[1];                                                    // T(n) = O(1), S(n) = O(1)

        for(int k = 2; k < n; k++) {
            if(ans[i] > ans[j]) {
                ans[++i] = nums[k];                                          // T(n) = O(1) per execution
            }else{
                ans[++j] = nums[k];                                          // T(n) = O(1) per execution
            }
        }
        return ans;                                                          // T(n) = O(1), S(n) = O(1)
    }
}
```

#  Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
