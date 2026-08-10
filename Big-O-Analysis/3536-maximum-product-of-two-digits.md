# 3536. Maximum Product of Two Digits

# Problem Summary:

You are given a positive integer n. Return the maximum product that can be obtained by choosing any two digits from n. The same digit can be used twice if it appears more than once in n.

# Approach Used:

To maximize the product of two digits, we only need to find the two largest digits in n.
The solution keeps track of:
* firstMax: the largest digit found so far.
* secMax: the second-largest digit found so far.

For every digit:
* If the digit is greater than firstMax, the old firstMax becomes secMax, and the current digit becomes firstMax.
* Otherwise, if the digit is greater than secMax, it becomes secMax.

At the end, firstMax * secMax is the maximum possible product. For example, if the digits are 3, 7, 2, 8, 8, the two largest digits are 8 and 8, so the maximum product is 8 * 8 = 64.

# Steps:

1. Initialize firstMax and secMax to 0.
2. Extract each digit from n using n % 10.
3. Compare the digit with firstMax and secMax.
4. Update the two maximum digits when necessary.
5. Remove the last digit using n /= 10.
6. Return firstMax * secMax.

# Solution:

```
class Solution {
    public int maxProduct(int n) {
        int firstMax = 0;                                                // T(n) = O(1), S(n) = O(1)
        int secMax = 0;                                                  // T(n) = O(1), S(n) = O(1)

        while(n > 0) {
            int digit = n % 10;                                         // T(n) = O(1)

            if(digit > firstMax) {
                secMax = firstMax;                                      // T(n) = O(1)
                firstMax = digit;                                       // T(n) = O(1)
            }else if(digit <= firstMax && digit > secMax) {
                secMax = digit;                                         // T(n) = O(1)
            }
            n /= 10;                                                     // T(n) = O(1)
        }

        return firstMax * secMax;                                       // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(log n)

# Space Complexity:
S(n) = O(1)
