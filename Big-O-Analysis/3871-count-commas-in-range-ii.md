# 3871. Count Commas in Range II

# Problem Summary:

You are given an integer n. The task is to return the total number of commas used when writing all integers from 1 to n, inclusive, using standard number formatting. A comma is inserted after every three digits from the right.

Therefore:
* Numbers with 1 to 3 digits contain 0 commas.
* Numbers with 4 to 6 digits contain 1 comma.
* Numbers with 7 to 9 digits contain 2 commas.
* Numbers with 10 to 12 digits contain 3 commas.
* In general, a number with d digits contains (d - 1) / 3 commas.

Unlike the previous version, n can be large enough that numbers may contain multiple commas.

# Approach Used:

The solution groups numbers according to their number of digits.

The digitsToCommas array stores the number of commas in a number based on its number of digits.

For example:
* digitsToCommas[1] = 0
* digitsToCommas[3] = 0
* digitsToCommas[4] = 1
* digitsToCommas[7] = 2
* digitsToCommas[10] = 3

The totalNums array stores how many numbers have exactly a particular number of digits.

For example:
* There are 9 one-digit numbers: 1 to 9.
* There are 90 two-digit numbers: 10 to 99.
* There are 900 three-digit numbers: 100 to 999.
* There are 9,000 four-digit numbers: 1,000 to 9,999.

For the numbers having the same number of digits as n, the solution counts only the numbers from the beginning of that digit range through n.

If n has d digits, the first d-digit number is: 10^(d - 1)

The expression: totalNums[d] / 9

gives this first d-digit number.

Therefore, (n - (totalNums[d] / 9) + 1) is the number of d-digit numbers from 10^(d - 1) through n. The solution multiplies this count by digitsToCommas[d] to calculate the commas contributed by these numbers. It then moves to smaller digit lengths and adds the contribution of every complete digit group.

# Steps:

1. Create digitsToCommas, where each index represents a digit count and the value represents the number of commas in a number with that many digits.
2. Create totalNums, where each index stores the number of integers having that many digits.
3. Determine the number of digits in n.
4. Calculate the number of integers having the same number of digits as n and lying between the first number of that digit length and n.
5. Multiply that count by the number of commas in each number of that digit length.
6. Move to the next smaller digit length.
7. Add the contribution of the complete group of numbers having that digit length.
8. Continue until there are no more digit groups.
9. Return the total number of commas.

# Solution:

```
class Solution {
    public long countCommas(long n) {
        int[] digitsToCommas = {
            0, 0, 0, 0, 1, 1, 1, 2, 2, 2, 3, 3, 3, 4, 4, 4, 5
        };                                                                   // T(n) = O(1), S(n) = O(1)

        long[] totalNums = {
            0L, 9L, 90L, 900L, 9_000L, 90_000L, 900_000L,
            9_000_000L, 90_000_000L, 900_000_000L,
            9_000_000_000L, 90_000_000_000L, 900_000_000_000L,
            9_000_000_000_000L, 90_000_000_000_000L,
            900_000_000_000_000L, 9_000_000_000_000_000L
        };                                                                   // T(n) = O(1), S(n) = O(1)

        int currDigits = Long.toString(n).length();                          // T(n) = O(log n), S(n) = O(log n)

        long ans = ((n - (totalNums[currDigits] / 9)) + 1)
                   * digitsToCommas[currDigits];                             // T(n) = O(1), S(n) = O(1)

        currDigits--;                                                        // T(n) = O(1), S(n) = O(1)

        while(currDigits > 0) {                                              // T(n) = O(log n)
            ans += totalNums[currDigits] * digitsToCommas[currDigits];       // T(n) = O(1), S(n) = O(1)
            currDigits--;                                                    // T(n) = O(1), S(n) = O(1)
        }

        return ans;                                                          // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(log n)

# Space Complexity:
S(n) = O(log n)
