# 2553. Separate the Digits in an Array

# Problem Summary: You are given an array of positive integers nums. Return an array containing all the digits of every integer in nums in the same order they appear. For example, if nums contains 10921, its digits should appear as: [1, 0, 9, 2, 1]

# Approach Used: The solution first converts each number into a character array and stores it. At the same time, it calculates the total number of digits required for the final answer array. Then, it traverses all stored character arrays, converts each character digit into an integer digit, and fills the final result array in order.

# Steps:
1) Create a 2D character array to store digits of each number  
2) Traverse the nums array  
3) Convert each number into a character array  
4) Store the character array and calculate total digit count  
5) Create the final answer array using the total digit count  
6) Traverse all stored character arrays  
7) Convert each character digit into an integer digit  
8) Store digits in the final answer array in the same order  
9) Return the final answer array

# Solution:
class Solution {

    public int[] separateDigits(int[] nums) {

        final int n = nums.length;                             // T(n) = O(1), S(n) = O(1)
        char[][] digits = new char[n][];                       // T(n) = O(n), S(n) = O(n)
        int totalLength = 0;                                   // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {
            digits[i] = String.valueOf(nums[i]).toCharArray();  // T(n) = O(k) for k digits in a number = O(d) across all digits, S(n) = O(k) for k digits in a number = O(d) across all digits
            
            totalLength += digits[i].length;                    // T(n) = O(1) per iteration = O(n)
        }

        int[] ans = new int[totalLength];                       // T(n) = O(d), S(n) = O(d)

        for(int i = 0, j = 0; i < n; i++) {
            for(char c : digits[i]) {
                ans[j++] = c - '0';                             // T(n) = O(1) per digit = O(d) across all digits
            }
        }

        return ans;                                             // T(n) = O(1)
    }
}

# Time Complexity:
Let d be the total number of digits across all numbers and n be the number of integers in nums.

T(n) = O(1) + O(n) + O(1) + O(d) + O(n) + O(d) + O(d) + O(1)
     = O(3 + 2n + 3d)
     = O(2n + 3d) // Ignore smaller constant term
     = O(3d) // d >= n
     = O(d) // Drop constant multiplier

# Space Complexity:
S(n) = O(1) + O(n) + O(1) + O(d) + O(d)
     = O(2 + n + 2d)
     = O(n + 2d) // Ignore smaller constant term
     = O(2d) // d >= n
     = O(d) // Drop constant multiplier
