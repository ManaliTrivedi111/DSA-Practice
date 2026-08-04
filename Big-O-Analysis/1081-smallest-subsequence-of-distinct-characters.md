# 1081. Smallest Subsequence of Distinct Characters

# Problem Summary:
You are given a string s. Return the lexicographically smallest subsequence that contains every distinct character from s exactly once. A subsequence is formed by deleting zero or more characters without changing the relative order of the remaining characters.

# Approach Used:
The solution uses a monotonic stack, implemented with a StringBuilder. Before processing the string, it counts the remaining occurrences of every character. While traversing the string:
* If the current character has already been included in the subsequence, skip it.
* Otherwise, remove larger characters from the end of the current subsequence if:
  * they will appear again later, and
  * replacing them with the current character produces a lexicographically smaller result.

A boolean array keeps track of which characters are already present in the subsequence to ensure every distinct character appears exactly once.

# Steps:
1. Count the occurrences of every character.
2. Initialize:
   * A StringBuilder to act as a stack.
   * A boolean array to track used characters.
3. Traverse the string:
   * Decrease the remaining count of the current character.
   * If the character is already used, continue.
   * While:
     * the stack is not empty,
     * the top character is lexicographically larger than the current character, and
     * the top character appears later again,
     remove it from the stack.
   * Mark the current character as used.
   * Push it onto the stack.
4. Return the resulting subsequence.

# Solution:

```
class Solution {

    public String smallestSubsequence(String s) {
        StringBuilder sb = new StringBuilder();                        // T(n) = O(1), S(n) = O(n)
        int[] count = new int[128];                                    // T(n) = O(1), S(n) = O(1)
        boolean[] used = new boolean[128];                             // T(n) = O(1), S(n) = O(1)

        for(char c : s.toCharArray()) {
            count[c]++;                                                // T(n) = O(1)
        }

        for(char c : s.toCharArray()) {
            count[c]--;                                                // T(n) = O(1)

            if(used[c]) {
                continue;
            }

            char ch = 'a';                                             // T(n) = O(1)

            while(sb.length() > 0 && 
            (ch = sb.charAt(sb.length() - 1)) > c && count[ch] > 0) {

                used[ch] = false;                                      // T(n) = O(1)
                sb.deleteCharAt(sb.length() - 1);                      // T(n) = O(1) amortized
            }

            used[c] = true;                                            // T(n) = O(1)
            sb.append(c);                                              // T(n) = O(1) amortized
        }
        return sb.toString();                                          // T(n) = O(n)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
