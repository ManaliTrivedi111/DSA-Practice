# 2472. Maximum Number of Non-overlapping Palindrome Substrings

# Problem Summary

Given a string s of length n and an integer k, select the maximum number of non-overlapping palindromic substrings such that every selected substring has length at least k.

# Approach Used

The solution uses:
1. A char[] representation of the string.
2. A one-dimensional DP array where:
   * dp[i] represents the maximum number of valid non-overlapping palindromic substrings that can be selected from the first i characters.
3. For every position i from k to n:
   * First, carry forward the previous result with dp[i] = dp[i - 1].
   * Check whether the substring of length k ending at i - 1 is a palindrome.
   * Check whether the substring of length k + 1 ending at i - 1 is a palindrome.
   * If either is a palindrome, update the DP value by selecting that palindrome.

The isPalindrome() method uses two pointers and may scan the entire substring in the worst case.

# Steps

1. Convert s to a character array.
2. Create a DP array of size n + 1.
3. Iterate i from k to n.
4. Set dp[i] = dp[i - 1], meaning we skip the character at position i - 1.
5. Check the substring [i - k, i - 1], which has length k.
6. If it is a palindrome, consider selecting it:
   1 + dp[i - k].
7. Check the substring [i - k - 1, i - 1], which has length k + 1.
8. If it is a palindrome, consider selecting it:
   1 + dp[i - k - 1].
9. Store the maximum possible value in dp[i].
10. Return dp[n].

# Solution

```
class Solution { 
    public int maxPalindromes(String s, int k) { 
        char[] str = s.toCharArray();                                        // T(n) = O(n), S(n) = O(n)
        final int n = str.length;                                            // T(n) = O(n), S(n) = O(1)
        int[] dp = new int[n + 1];                                           // T(n) = O(n), S(n) = O(n)
  
        for(int i = k; i <= n; i++) {                                        // T(n) = O(n)
            dp[i] = dp[i - 1];                                               // T(n) = O(1)
 
            if(isPalindrome(str, i - k, i - 1)) {                            // T(n) = O(k), S(n) = O(1)
                dp[i] = Math.max(dp[i], 1 + dp[i - k]);                      // T(n) = O(1)
            } 
            if(isPalindrome(str, i - k - 1, i - 1)) {                        // T(n) = O(k), S(n) = O(1)
                dp[i] = Math.max(dp[i], 1 + dp[i - k - 1]);                  // T(n) = O(1)
            }  
             
        } 
        return dp[n];                                                        // T(n) = O(1)
    } 
 
    private boolean isPalindrome(char[] str, int l, int r) { 
        if(l < 0) {                                                          // T(n) = O(1)
            return false; 
        } 
        while(l < r) {                                                       // T(n) = O(k)
            if(str[l++] != str[r--]) { 
                return false; 
            }     
        } 
        return true; 
    } 
}
```

# Time Complexity:
T(n) = O(n²)

# Space Complexity:
S(n) = O(n) 
