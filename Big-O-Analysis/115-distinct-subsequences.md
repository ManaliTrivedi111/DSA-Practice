# 115. Distinct Subsequences

# Problem Summary:

You are given two strings s and t. The task is to return the number of distinct subsequences of s that are equal to t. A subsequence is formed by deleting zero or more characters from a string without changing the relative order of the remaining characters.

# Approach Used:

The solution uses dynamic programming with a 2D array dp.

dp[i][j] represents the number of distinct subsequences of the first i characters of s that are equal to the first j characters of t.

For every character pair s[i - 1] and t[j - 1]:
* If the characters are equal, there are two choices:
  1. Use s[i - 1] to match t[j - 1], giving dp[i - 1][j - 1] possibilities.
  2. Skip s[i - 1], giving dp[i - 1][j] possibilities.

  Therefore:
  dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]

* If the characters are different, s[i - 1] cannot match t[j - 1], so it must be skipped:
  dp[i][j] = dp[i - 1][j]

The base case is dp[i][0] = 1 for every i, because there is exactly one way to form an empty string from any prefix of s: delete all its characters.

# Steps:

1. Convert s and t into character arrays.
2. Let m be the length of s and n be the length of t.
3. Create a 2D DP array of size (m + 1) x (n + 1).
4. Initialize dp[i][0] = 1 for every i, because the empty string is a subsequence of every prefix of s in exactly one way.
5. Traverse s from left to right using i.
6. For each i, traverse t from left to right using j.
7. If s[i - 1] == t[j - 1], add the number of ways obtained by using the current character and the number of ways obtained by skipping it.
8. Otherwise, copy dp[i - 1][j].
9. Return dp[m][n].

# Solution:

```
class Solution {
    public int numDistinct(String s, String t) {
        char[] sChars = s.toCharArray();                                     // T(n) = O(m), S(n) = O(m)
        char[] tChars = t.toCharArray();                                     // T(n) = O(n), S(n) = O(n)
        final int m = sChars.length;                                         // T(n) = O(1), S(n) = O(1)
        final int n = tChars.length;                                         // T(n) = O(1), S(n) = O(1)
        int[][] dp = new int[m + 1][n + 1];                                  // T(n) = O(mn), S(n) = O(mn)

        for(int i = 0; i <= m; i++) {                                        // T(n) = O(m)
            dp[i][0] = 1;                                                    // T(n) = O(1), S(n) = O(1)
        }
        
        for(int i = 1; i <= m; i++)  {                                       // T(n) = O(m)
            for(int j = 1; j <= n; j++) {                                    // T(n) = O(n) per iteration of outer loop
                if(sChars[i - 1] == tChars[j - 1]) {                         // T(n) = O(1), S(n) = O(1)
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];              // T(n) = O(1), S(n) = O(1)
                }else {
                    dp[i][j] = dp[i - 1][j];                                 // T(n) = O(1), S(n) = O(1)
                }
            }
        }
        return dp[m][n];                                                     // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(m, n) = O(mn), where m = length of s, and n = length of t

# Space Complexity:
S(m, n) = O(mn), where m = length of s, and n = length of t
