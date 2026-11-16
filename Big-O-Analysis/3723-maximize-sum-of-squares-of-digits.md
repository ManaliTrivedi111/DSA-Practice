# 3723. Maximize Sum of Squares of Digits

# Problem Summary:

Given two positive integers num and sum, construct a num-digit integer whose digits add up to sum and whose sum of squared digits is maximized. If multiple integers achieve the maximum score, return the largest one. Return an empty string if no valid integer exists.

# Approach Used:

To maximize the sum of squared digits, use as many 9s as possible because 9 is the largest digit. Place these 9s first, followed by the remaining digit (if any), and then fill the remaining positions with zeros.
- k = sum / 9 gives the number of 9s.
- sum %= 9 gives the remainder.
- If num * 9 < sum initially, the required digit sum cannot be achieved, so return an empty string.

Placing the largest digits first also gives the maximum integer when multiple arrangements have the same score.

# Steps:

1. Check whether sum exceeds the maximum possible digit sum, num * 9.
2. Calculate the number of 9s using k = sum / 9.
3. Calculate the remainder using sum %= 9.
4. Initialize the result with k copies of 9.
5. Append the remainder if it is greater than zero.
6. Append zeros until the result contains exactly num digits.
7. Return the resulting string.

# Solution:

```
class Solution {
    public String maxSumOfSquares(int num, int sum) {
        if(num * 9 < sum) {                                                  // T(n) = O(1), S(n) = O(1)
            return "";                                                       // T(n) = O(1)
        }

        int k = sum / 9;                                                     // T(n) = O(1), S(n) = O(1)
        sum %= 9;                                                            // T(n) = O(1)
        StringBuilder ans = new StringBuilder("9".repeat(k));                // T(n) = O(k), S(n) = O(k)

        if(sum > 0) {                                                        // T(n) = O(1)
            ans.append(sum);                                                 // T(n) = O(1)
        }

        ans.append("0".repeat(num - ans.length()));                          // T(n) = O(n), S(n) = O(n)
        return ans.toString();                                               // T(n) = O(n), S(n) = O(n)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
