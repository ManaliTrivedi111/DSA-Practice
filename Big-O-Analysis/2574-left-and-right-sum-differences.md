# 2574. Left and Right Sum Differences

# Problem Summary:
Given a 0-indexed integer array nums, define:
-> leftSum[i] as the sum of all elements to the left of index i.
-> rightSum[i] as the sum of all elements to the right of index i.
If no elements exist on a side, the corresponding sum is 0. Return an array answer where: answer[i] = |leftSum[i] - rightSum[i]|

# Approach Used:
The solution avoids creating separate leftSum and rightSum arrays. First, it calculates the total sum of all elements and stores it in rightSum. Then, while traversing the array:
-> Remove the current element from rightSum so that rightSum represents the sum of elements to the right of the current index.
-> Compute the absolute difference between leftSum and rightSum.
-> Add the current element to leftSum so that it becomes available for future indices.
This allows both left and right sums to be maintained dynamically using only two variables.

# Steps:
1) Create the answer array
2) Calculate the total sum of all elements and store it in rightSum
3) Initialize leftSum as 0
4) Traverse the array from left to right
5) For each index:
   -> Remove the current element from rightSum
   -> Compute the absolute difference between leftSum and rightSum
   -> Store the result in the answer array
   -> Add the current element to leftSum
6) Return the answer array

# Solution:
class Solution {

    public int[] leftRightDifference(int[] nums) {

        final int n = nums.length;                  // T(n) = O(1), S(n) = O(1)
        int[] ans = new int[n];                     // T(n) = O(1), S(n) = O(n)
        int rightSum = 0;                           // T(n) = O(1), S(n) = O(1)
        int leftSum = 0;                            // T(n) = O(1), S(n) = O(1)

        // Loop runs n times
        for(int i = 0; i < n; i++) {
            rightSum += nums[i];                    // T(n) = O(1) per execution, total executions = O(n)
        }

        // Loop runs n times
        for(int i = 0; i < n; i++) {
            rightSum -= nums[i];                    // T(n) = O(1) per execution
            ans[i] = Math.abs(rightSum - leftSum);  // T(n) = O(1) per execution
            leftSum += nums[i];                     // T(n) = O(1) per execution
        }
        return ans;                                 // T(n) = O(1)
    }
}

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)

