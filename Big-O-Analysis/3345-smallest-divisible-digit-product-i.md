# 3345. Smallest Divisible Digit Product I

# Problem Summary:

You are given two integers n and t. Return the smallest number greater than or equal to n such that the product of its digits is divisible by t.

# Approach Used:

The solution uses a brute-force approach based on the fact that the number only needs to be checked until we find the first valid number.

The solution handles three cases:
1. If the last digit of n is 0, the product of the digits is 0. Since 0 is divisible by every positive integer t, n itself is the answer.
2. If n is a single-digit number, its digit product is simply n. We increment n until either n is divisible by t or n reaches 10.
3. If n has at least two digits, the solution only considers the last two digits. It stores the tens digit in a and the units digit in b, and checks whether a * b is divisible by t. If not, it increments n and updates the two digits.

The solution keeps checking consecutive numbers until it finds the smallest valid number.

# Steps:

1. If n ends in 0, return n immediately because its digit product is 0.
2. If n is less than 10, repeatedly check whether n is divisible by t.
3. If n has at least two digits, extract its last two digits.
4. Check whether the product of those two digits is divisible by t.
5. If it is not divisible, increment n.
6. Extract the new last two digits.
7. Continue until the digit product is divisible by t.
8. Return the first valid number found.

# Solution:

```
class Solution {
    public int smallestNumber(int n, int t) {
        if(n % 10 == 0) {
            return n;                                                        // T(n) = O(1), S(n) = O(1)
        }

        if(n < 10) {
            while(n % t != 0 && n < 10) {
                n++;                                                         // T(n) = O(1), S(n) = O(1)
            }
            return n;                                                        // T(n) = O(1), S(n) = O(1)
        }

        int a = (n / 10) % 10;                                               // T(n) = O(1), S(n) = O(1)
        int b = n % 10;                                                      // T(n) = O(1), S(n) = O(1)

        while((a * b) % t != 0) {
            n++;                                                             // T(n) = O(1), S(n) = O(1)
            a = (n / 10) % 10;                                               // T(n) = O(1), S(n) = O(1)
            b = n % 10;                                                      // T(n) = O(1), S(n) = O(1)
        }

        return n;                                                            // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(1)

# Space Complexity:
S(n) = O(1)
