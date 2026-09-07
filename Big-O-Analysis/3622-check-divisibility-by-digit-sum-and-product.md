# 3622. Check Divisibility by Digit Sum and Product

# Problem Summary:

You are given a positive integer n. You need to calculate the digit sum and digit product of n, then determine whether n is divisible by their sum.

# Approach Used:

The solution maintains two variables: sum for the digit sum and product for the digit product.

For every digit of n, it extracts the last digit, adds it to sum, multiplies it into product, and removes the last digit from num.

After all digits are processed, it checks whether n is divisible by sum + product.

# Steps:

1. Initialize sum to 0.
2. Initialize product to 1.
3. Copy n into num so the original value is preserved.
4. While num is greater than 0:
5. Extract the last digit using num % 10.
6. Add the digit to sum.
7. Multiply the digit into product.
8. Remove the last digit using num /= 10.
9. Calculate sum + product.
10. Return whether n % (sum + product) == 0.

# Solution:

```
class Solution {
    public boolean checkDivisibility(int n) {
        int sum = 0;                                                         // T(n) = O(1), S(n) = O(1)
        int product = 1;                                                     // T(n) = O(1), S(n) = O(1)
        int num = n;                                                         // T(n) = O(1), S(n) = O(1)

        while(num > 0) {                                                     // T(n) = O(log n)
            sum += num % 10;                                                 // T(n) = O(1), S(n) = O(1)
            product *= num % 10;                                             // T(n) = O(1), S(n) = O(1)
            num /= 10;                                                       // T(n) = O(1), S(n) = O(1)
        }
        return n % (sum + product) == 0;                                     // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(log n)

# Space Complexity:
S(n) = O(1)
