# 3751. Total Waviness of Numbers in Range I

# Problem Summary:
Given two integers num1 and num2 representing an inclusive range [num1, num2], calculate the total waviness of all numbers in that range. The waviness of a number is defined as the total number of peaks and valleys in its digits:
-> A digit is a peak if it is strictly greater than both of its immediate neighboring digits.
-> A digit is a valley if it is strictly less than both of its immediate neighboring digits.
-> The first and last digits cannot be peaks or valleys because they do not have two neighbors.
-> Any number with fewer than 3 digits has a waviness of 0.
Return the sum of waviness values for every number in the given range.

# Approach Used:
The solution processes every number in the range individually.
For each number:
-> Examine every group of three consecutive digits.
-> The middle digit of each group is checked to determine whether it forms a peak or a valley.
-> If the middle digit is greater than both neighboring digits, it is a peak.
-> If the middle digit is less than both neighboring digits, it is a valley.
-> Each valid peak or valley increases the waviness count by 1.
The digits are processed from right to left using modulo and division operations. Every three-digit window is extracted and evaluated until fewer than three digits remain.
The waviness of each number is added to the final result.

# Steps:
1) Initialize a variable to store the total waviness of the range
2) Iterate through every number from num1 to num2
3) For the current number:
   -> Initialize its waviness count to 0
4) While at least three digits remain:
   -> Extract the last three digits
   -> Identify the first, middle, and last digits of the three-digit window
   -> Check whether the middle digit is a peak or a valley
   -> If yes, increment the waviness count
   -> Remove the last digit and continue checking the next window
5) Add the current number's waviness to the final result
6) Return the total waviness

# Solution:
class Solution {

    public int totalWaviness(int num1, int num2) {
        int result = 0;                              // T(n) = O(1), S(n) = O(1)

        // Loop runs (num2 - num1 + 1) times
        for(int i = num1; i <= num2; i++) {

            int num = i;                             // T(n) = O(1), S(n) = O(1)
            int waviness = 0;                        // T(n) = O(1), S(n) = O(1)

            // Processes each three-digit window of the current number
            while(num >= 100) {
                int target = num % 1000;             // T(n) = O(1), S(n) = O(1)
                int last = target % 10;              // T(n) = O(1), S(n) = O(1)
                target /= 10;                        // T(n) = O(1)
                int mid = target % 10;               // T(n) = O(1), S(n) = O(1)
                target /= 10;                        // T(n) = O(1)
                int first = target % 10;             // T(n) = O(1), S(n) = O(1)

                if((mid > first && mid > last) ||
                   (mid < first && mid < last)) {
                    waviness++;                     // T(n) = O(1) per iteration
                }
                num /= 10;                          // T(n) = O(1) per iteration
            }
            result += waviness;                     // T(n) = O(1) per iteration
        }
        return result;                              // T(n) = O(1)
    }
}

# Time Complexity:
Let:
-> n = num2 - num1 + 1 (total numbers in the range)
-> d = maximum number of digits among numbers in the range
For each number, the while loop examines every possible three-digit window. A number with d digits contains (d - 2) such windows, which is O(d).

Therefore:
T(n, d) = O(n × d)

# Space Complexity:
S(n) = O(1)

