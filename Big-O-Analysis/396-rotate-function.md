# 396. Rotate Function

# Problem Summary:
You are given an integer array nums of length n. For each clockwise rotation of the array, calculate the rotate function:
F(k) = 0 × arrk[0] + 1 × arrk[1] + 2 × arrk[2] + ... + (n - 1) × arrk[n - 1]
Return the maximum value among all possible rotations.

# Approach Used: 
The solution first calculates the total sum of all elements and the value of F(0). Then it uses the mathematical relation between consecutive rotations to compute the next rotate function in constant time instead of recalculating from scratch.

# Steps:
1) Calculate the total sum of all array elements  
2) Calculate F(0) using index x value
3) Store F(0) as the initial answer
4) Traverse the array from right to left  
5) Use the formula: next function = current function + sum - n × nums[i] 
6) Update the maximum answer during each rotation  
7) Return the maximum value found

# Solution:
class Solution {

    public int maxRotateFunction(int[] nums) {

        final int n = nums.length;      // T(n) = O(1), S(n) = O(1)
        int sum = 0;                   // T(n) = O(1), S(n) = O(1)
        int func = 0;                  // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {
            sum += nums[i];            // T(n) = O(1) per iteration = O(n)
            func += nums[i] * i;      // T(n) = O(1) per iteration = O(n)
        }

        int ans = func;               // T(n) = O(1), S(n) = O(1)

        for(int i = n - 1; i >= 0; i--) {
            func += sum - n * nums[i]; // T(n) = O(1) per iteration = O(n)
            ans = Math.max(ans, func); // T(n) = O(1) per iteration = O(n)
        }

        return ans;                  // T(n) = O(1)
    }
}

# Time Complexity:
T(n) = O(1) + O(1) + O(1) + O(n) + O(n) + O(1) + O(n) + O(n) + O(1)
     = O(5 + 4n)
     = O(4n) // Ignore smaller constant term
     = O(n) // Drop constant multiplier

# Space Complexity:
S(n) = O(1) + O(1) + O(1) + O(1)
     = O(4)
     = O(1) // Drop constant multiplier
