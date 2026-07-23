# 3754. Concatenate Non-Zero Digits and Multiply by Sum I

# Problem Summary:
You are given an integer n. Construct a new integer x by concatenating all the non-zero digits of n while preserving their original order.
* If n contains no non-zero digits, then x = 0.
* Let sum be the sum of the digits of x.

Return the value of: x × sum

# Approach Used:
The solution uses a stack to preserve the original order of the non-zero digits. Since extracting digits using % 10 processes the number from right to left:
* Push every non-zero digit onto the stack.
* Pop the digits one by one to rebuild x in the correct order.
* While rebuilding x, simultaneously compute the sum of its digits.
* Finally, return x × sum.

# Steps:
1. Initialize:
   * x = 0
   * sum = 0
   * An empty stack.
2. Extract the digits of n from right to left.
3. Push every non-zero digit onto the stack.
4. Pop each digit:
   * Append it to x.
   * Add it to sum.
5. Return x × sum.

# Solution:

```
class Solution {

    public long sumAndMultiply(int n) {
        long sum = 0;                                        // T(n) = O(1), S(n) = O(1)
        Deque<Integer> stack = new ArrayDeque<>();           // T(n) = O(1), S(n) = O(1)
        long x = 0;                                          // T(n) = O(1), S(n) = O(1)

        while(n > 0) {
            if(n % 10 != 0) {
                stack.push(n % 10);                          // T(n) = O(1)
            }
            n /= 10;                                         // T(n) = O(1)
        }

        while(!stack.isEmpty()) {
            int digit = stack.pop();                         // T(n) = O(1)
            x = x * 10 + digit;                              // T(n) = O(1)
            sum += digit;                                    // T(n) = O(1)
        }
        return x * sum;                                      // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(d), where d = the number of digits in n

# Space Complexity:
S(n) = O(d), where d = the number of digits in n
