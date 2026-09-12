# 3720. Lexicographically Smallest Permutation Greater Than Target

# Problem Summary:

You are given two strings s and target, both of length n, consisting of lowercase English letters.

The goal is to find the lexicographically smallest permutation of s that is strictly greater than target.

If no permutation of s is lexicographically strictly greater than target, return an empty string.

# Approach Used:

The solution uses a frequency array of size 26 to keep track of the available characters in s.

First, it tries to match target from left to right using the available characters from s. startIndex records the first position where the matching process can no longer continue.

Then, it works backward from that position. At each position i, it restores the character used by the prefix when necessary and tries to choose the smallest available character that is strictly greater than target[i].

Once such a character is found, the prefix remains equal to target, that character makes the result strictly greater, and all remaining characters are appended in sorted order. This produces the lexicographically smallest possible permutation for that position.

If no position allows a greater character to be selected, no valid permutation exists, so the solution returns an empty string.

# Steps:

1. Convert s and target into character arrays and create a frequency array sCount of size 26.
2. Count the occurrences of every character in s.
3. Starting from index 0, consume characters from sCount as long as they can exactly match the corresponding characters in target.
4. Store the first position where the exact prefix match cannot continue in startIndex.
5. Traverse backward from startIndex (or n - 1 if startIndex == n).
6. Restore the character from target[i] when it was previously consumed as part of the matching prefix.
7. For the current position, search for the smallest available character strictly greater than target[i].
8. If such a character exists:
   * Append the prefix target[0...i-1].
   * Append the selected greater character.
   * Append all remaining characters in sorted order.
   * Return the resulting string.
9. If no position works, return an empty string.

# Solution:

```
class Solution {
    public String lexGreaterPermutation(String s, String target) {
        final int n = s.length();                                            // T(n) = O(1), S(n) = O(1)
        char[] sChars = s.toCharArray();                                     // T(n) = O(n), S(n) = O(n)
        char[] tChars = target.toCharArray();                                // T(n) = O(n), S(n) = O(n)
        int[] sCount = new int[26];                                          // T(n) = O(1), S(n) = O(1)

        for(char c : sChars) {                                               // T(n) = O(n)
            sCount[c - 'a']++;                                               // T(n) = O(1), S(n) = O(1)
        }

        int startIndex = 0;                                                  // T(n) = O(1), S(n) = O(1)

        while(startIndex < n && sCount[tChars[startIndex] - 'a'] > 0) {      // T(n) = O(n)
            sCount[tChars[startIndex] - 'a']--;                              // T(n) = O(1), S(n) = O(1)
            startIndex++;                                                    // T(n) = O(1), S(n) = O(1)
        }

        for(int i = Math.min(startIndex, n - 1); i >= 0; i--) {              // T(n) = O(n)
            if(i < startIndex) {                                           
                sCount[tChars[i] - 'a']++;                                   // T(n) = O(1), S(n) = O(1)
            }

            int targetChar = tChars[i] - 'a';                                // T(n) = O(1), S(n) = O(1)

            for(int c = targetChar + 1; c < 26; c++) {                       // T(n) = O(26) = O(1) per i
                if(sCount[c] > 0) {
                    StringBuilder result = new StringBuilder();              // T(n) = O(1), S(n) = O(n)
                    result.append(target.substring(0, i));                   // T(n) = O(n), S(n)
                    result.append((char)(c + 'a'));                          // T(n) = O(1), S(n) = O(1)
                    sCount[c]--;                                             // T(n) = O(1), S(n) = O(1)
                    
                    for(int j = 0; j < 26; j++) {                            // T(n) = O(26) = O(1)
                        while(sCount[j] > 0) {                               // T(n) = O(n)
                            result.append((char)(j + 'a'));                  // T(n) = O(1), S(n) = O(1)
                            sCount[j]--;                                     // T(n) = O(1), S(n) = O(1)
                        }
                    }
                    return result.toString();                                // T(n) = O(n), S(n) = O(n)
                }
            }
        }
        return "";                                                           // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
