# 32. Longest Valid Parentheses

# Problem Summary

Given a string containing only ( and ), return the length of the longest valid (well-formed) parentheses substring.

# Approach Used

Use dynamic programming.

dp[i] stores the length of the longest valid parentheses substring that ends at index i in the modified string s2.

The solution prepends ) to the original string. This gives every valid ( a safe preceding index when calculating the matching position.

For each ) at index i:
- dp[i - 1] is the length of the valid substring immediately before i.
- The character at i - dp[i - 1] - 1 is checked to see whether it is the matching (.
- If it is, the newly formed valid substring has:
  - the previous valid substring of length dp[i - 1],
  - the matching pair (), contributing 2,
  - and any valid substring immediately before that matching (, represented by dp[i - dp[i - 1] - 2].

The maximum value in dp is the answer.

# Steps

1. Prepend ) to s and store the result in s2.
2. Create a dp array of length n.
3. For every index i from 1 to n - 1:
   - Check whether s2[i] is ).
   - Calculate the position of the possible matching ( using i - dp[i - 1] - 1.
   - If it is (, update dp[i].
4. Find and return the maximum value in dp.

# Solution

```
class Solution {
   public int longestValidParentheses(String s) {
       final String s2 = ")" + s;                                            // T(n) = O(n), S(n) = O(n)
       final int n = s2.length();                                            // T(n) = O(1), S(n) = O(1)
       int dp[] = new int[n];                                                // T(n) = O(n), S(n) = O(n)

       for(int i = 1; i < n; i++) {                                          // T(n) = O(n), S(n) = O(1)
           if(s2.charAt(i) == ')' &&                                         
              s2.charAt(i - dp[i - 1] - 1) == '(') {                         // T(n) = O(1), S(n) = O(1)
               dp[i] = dp[i - 1] + dp[i - dp[i - 1] - 2] + 2;                // T(n) = O(1), S(n) = O(1)
                                                  
           }
       }
       return Arrays.stream(dp).max().getAsInt();                            // T(n) = O(n), S(n) = O(1)
   }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
