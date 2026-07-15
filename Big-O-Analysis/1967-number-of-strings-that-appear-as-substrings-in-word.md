# 1967. Number of Strings That Appear as Substrings in Word

# Problem Summary:
You are given an array of strings patterns and a string word. Return the number of strings in patterns that appear as a substring of word. A substring is a contiguous sequence of characters within a string.

# Approach Used:
The solution checks each pattern independently using a brute-force substring search.

For every pattern:
* Convert both the pattern and the word into character arrays.
* Slide the pattern across every possible starting position in the word.
* Compare the characters one by one.
* If all characters match, the pattern exists as a substring. Increment the answer and stop checking that pattern.

The process repeats for every pattern.

# Steps:
1. Convert the input word into a character array.
2. Initialize the answer to 0.
3. For every pattern:
   * Convert it into a character array.
   * Try every possible starting position in the word.
   * Compare the pattern with the corresponding characters in the word.
   * If a complete match is found:
     * Increment the answer.
     * Stop searching for that pattern.
4. Return the total number of matching patterns.

# Solution:

```
class Solution {
    public int numOfStrings(String[] patterns, String word) {
        int total = 0;                                      // T(n) = O(1), S(n) = O(1)

        char[] wordArr = word.toCharArray();                // T(n) = O(n), S(n) = O(n)
        final int n = wordArr.length;                       // T(n) = O(1)

        for(String ptr : patterns) {
            char[] ptrArr = ptr.toCharArray();              // T(n) = O(m), S(n) = O(m)
            final int m = ptrArr.length;                    // T(n) = O(1)

            for(int i = 0; i <= n - m; i++) {
                boolean exists = true;                      // T(n) = O(1)

                for(int j = 0, k = i; j < m && k < n; j++, k++) {
                    if(wordArr[k] != ptrArr[j]) {
                        exists = false;                     // T(n) = O(1)
                        break;
                    }
                }

                if(exists) {
                    total++;                                // T(n) = O(1)
                    break;
                }
            }
        }

        return total;                                       // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(p * n * m)

where p = number of strings in patterns, n = length of word, m = average length of a pattern

# Space Complexity:
S(n) = O(n + m)

where n = space used for the character array of word, m = space used for the character array of each pattern
