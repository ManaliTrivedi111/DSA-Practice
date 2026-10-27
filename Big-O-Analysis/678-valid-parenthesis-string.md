# 678. Valid Parenthesis String

# Problem Summary

Given a string s containing (, ) and *, determine whether it can be transformed into a valid parentheses string.

The * character can represent:
- (
- )
- an empty string

# Approach Used

Use dynamic programming.

dp[index][openBracket] represents whether the substring starting at index can form a valid parentheses string when there are openBracket unmatched opening parentheses before processing s[index].

For each character:
- If it is (, it must increase the number of open parentheses.
- If it is ), it can only be used if there is at least one unmatched opening parenthesis, so the count decreases.
- If it is *, it can:
  - act as (, increasing the count;
  - act as ), decreasing the count if possible;
  - act as an empty string, leaving the count unchanged.

The table is filled from right to left because the state at index depends on states at index + 1.

The initial state dp[n][0] = true means that after processing the entire string, the string is valid when there are no unmatched opening parentheses.

# Steps

1. Let n be the length of s.
2. Create a (n + 1) x (n + 1) boolean DP table.
3. Set dp[n][0] = true.
4. Process index from n - 1 down to 0.
5. For each possible openBracket count:
   - If the character is *, consider all three possibilities.
   - If it is (, increase openBracket.
   - If it is ), decrease openBracket only when openBracket > 0.
6. Store whether at least one possibility leads to a valid state.
7. Return dp[0][0].

# Solution

```
class Solution {
   public boolean checkValidString(String s) {
       int n = s.length();                                                   // T(n) = O(1), S(n) = O(1)
       boolean[][] dp = new boolean[n + 1][n + 1];                           // T(n) = O(n²), S(n) = O(n²)
       dp[n][0] = true;                                                      // T(n) = O(1), S(n) = O(1)

       for (int index = n - 1; index >= 0; index--) {                        // T(n) = O(n)
           for (int openBracket = 0; openBracket < n; openBracket++) {       // T(n) = O(n²) total
               boolean isValid = false;                                      // T(n) = O(1), S(n) = O(1)

               if (s.charAt(index) == '*') {                                 // T(n) = O(1), S(n) = O(1)
                   isValid |= dp[index + 1][openBracket + 1];                // T(n) = O(1), S(n) = O(1)

                   if (openBracket > 0) {                                    // T(n) = O(1), S(n) = O(1)
                       isValid |= dp[index + 1][openBracket - 1];            // T(n) = O(1), S(n) = O(1)
                   }
                   isValid |= dp[index + 1][openBracket];                    // T(n) = O(1), S(n) = O(1)
               } else {
                   if (s.charAt(index) == '(') {                             // T(n) = O(1), S(n) = O(1)
                       isValid |= dp[index + 1][openBracket + 1];            // T(n) = O(1), S(n) = O(1)
                   } else if (openBracket > 0) {                             // T(n) = O(1), S(n) = O(1)
                       isValid |= dp[index + 1][openBracket - 1];            // T(n) = O(1), S(n) = O(1)
                   }
               }
               dp[index][openBracket] = isValid;                             // T(n) = O(1), S(n) = O(1)
           }
       }
       return dp[0][0];                                                      // T(n) = O(1), S(n) = O(1)
   }
}
```


# Time Complexity:
T(n) = O(n²)

# Space Complexity:
S(n) = O(n²)
