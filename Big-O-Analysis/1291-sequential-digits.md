# 1291. Sequential Digits

# Problem Summary:
An integer has sequential digits if every digit is exactly one greater than the previous digit. For example: 123, 4567, 6789, etc. are sequential-digit numbers, while: 124, 135, 122, etc. are not.
Given two integers low and high, return all sequential-digit numbers in the range [low, high] in sorted order.

# Approach Used:
The solution uses precomputed sequential-digit numbers for every possible digit length. For each number of digits from the length of low to the length of high:
* Start from the smallest sequential number of that length.
* Repeatedly add a fixed increment (such as 111 or 1111) to generate the next sequential number.
* Skip numbers smaller than low.
* Add every valid number until the upper limit for that digit length or high is reached.

Since there are only a limited number of sequential-digit numbers (36 in total), this approach is very efficient.

# Steps:
1. Store the smallest and largest sequential number for each digit length.
2. Store the increment needed to generate the next sequential number of the same length.
3. Limit high to 123456789, since no larger sequential-digit number exists.
4. Determine the number of digits in low and high.
5. For every digit length in that range:
   * Start from the smallest sequential number.
   * Skip numbers smaller than low.
   * Add every valid sequential number until exceeding the allowed limit.
6. Return the resulting list.

# Solution:

```
class Solution {

    public List<Integer> sequentialDigits(int low, int high) {
        List<Integer> ans = new ArrayList<>();                        // T(n) = O(1), S(n) = O(1)

        int[][] range = {
            {0, 0},
            {1, 9},
            {12, 89},
            {123, 789},
            {1234, 6789},
            {12345, 56789},
            {123456, 456789},
            {1234567, 3456789},
            {12345678, 23456789},
            {123456789, 123456789}
        };                                                            // T(n) = O(1), S(n) = O(1)

        int[] add = {
            0,
            1,
            11,
            111,
            1111,
            11111,
            111111,
            1111111,
            11111111,
            111111111
        };                                                            // T(n) = O(1), S(n) = O(1)

        high = high > 999999999 ? 999999999 : high;                   // T(n) = O(1)
        int lowDigits = Integer.toString(low).length();               // T(n) = O(1), S(n) = O(1)
        int highDigits = Integer.toString(high).length();             // T(n) = O(1), S(n) = O(1)
        int i = lowDigits;                                            // T(n) = O(1), S(n) = O(1)

        while(i <= highDigits) {
            int j = range[i][0];                                      // T(n) = O(1)

            while(j < low) {
                j += add[i];                                          // T(n) = O(1) per execution
            }

            while(j <= Math.min(range[i][1], high)) {
                ans.add(j);                                           // T(n) = O(1)
                j += add[i];                                          // T(n) = O(1)
            }
            i++;                                                      // T(n) = O(1)
        }
        return ans;                                                   // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(1)

# Space Complexity:
S(n) = O(1)
