# 3658. GCD of Odd and Even Sums

# Problem Summary:
You are given an integer n. Compute:
* sumOdd = the sum of the first n positive odd numbers.
* sumEven = the sum of the first n positive even numbers.

Return the greatest common divisor (GCD) of these two sums.

# Approach Used:
Instead of explicitly summing the numbers, the solution uses the mathematical formulas:
Sum of the first n odd numbers: n^2
Sum of the first n even numbers: n * (n + 1)

After computing these two values, the Euclidean Algorithm is used to find their GCD efficiently.

# Steps:
1. Compute the sum of the first n even numbers as n × (n + 1).
2. Compute the sum of the first n odd numbers as n × n.
3. Use the Euclidean Algorithm to compute their GCD.
4. Return the result.

# Solution:

```
class Solution {

    public int gcdOfOddEvenSums(int n) {
        final int evenSum = n * (n + 1);                   // T(n) = O(1)
        final int oddSum = n * n;                          // T(n) = O(1)
        return gcd(evenSum, oddSum);                       // T(n) = O(log n)
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);                 // T(n) = O(log n)
    }
}
```

# Time Complexity:
T(n) = O(log n)

# Space Complexity:
S(n) = O(log n)
