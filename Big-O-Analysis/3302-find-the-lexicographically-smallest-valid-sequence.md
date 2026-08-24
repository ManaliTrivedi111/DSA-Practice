# 3302. Find the Lexicographically Smallest Valid Sequence

# Problem Summary:

You are given two strings word1 and word2. A string x is almost equal to y if at most one character in x can be changed to make it identical to y.

We need to choose word2.length() indices from word1 such that:
* The indices are in ascending order.
* The characters at those indices form a string that is almost equal to word2.
* Among all valid index sequences, we return the lexicographically smallest one.

If no valid sequence exists, return an empty array.

# Approach Used:

The solution uses a greedy approach with a suffix matching array. The main challenge is that we are allowed to have at most one character mismatch. We want to choose the smallest possible index at every position while still leaving enough characters to complete the rest of word2.

First, the solution creates the last array. last[j] stores the rightmost index in word1 that can be used to match word2[j] while matching the remaining suffix of word2. This array is constructed by scanning both strings from right to left.
For example, when word1[i] == word2[j], we store i in last[j] and move to the previous character of word2.

After building last, we scan word1 from left to right to construct the answer.

For every position:
1. If word1[i] == word2[j], we take index i because it is an exact match.
2. Otherwise, we can use this index as our one allowed mismatch if:
   * We have not used the mismatch yet (canSkip == true), and
   * Either we are matching the last character of word2, or the remaining suffix can still be matched after choosing index i.

The condition: j == n - 1 || i < last[j + 1] ensures that after using i as the mismatched character, there is still a valid way to match all remaining characters of word2.

Because we scan word1 from left to right and select the earliest possible index at every step, the resulting sequence is lexicographically smallest.

# Steps:

1. Convert word1 and word2 into character arrays.
2. Store their lengths in m and n.
3. Create ans to store the selected indices.
4. Create last, initialized with -1, to store the rightmost usable positions for suffix matching.
5. Traverse word1 and word2 from right to left to build the last array.
6. Traverse word1 from left to right to construct the answer.
7. If the current character exactly matches word2[j], select the current index.
8. Otherwise, if the one allowed mismatch has not been used, check whether selecting the current index still allows the rest of word2 to be matched.
9. If valid, use the mismatch and select the current index.
10. Continue until all characters of word2 have been selected.
11. If all n indices were selected, return ans; otherwise, return an empty array.

# Solution:

```
class Solution {
    public int[] validSequence(String word1, String word2) {
        char[] wrd1 = word1.toCharArray();                                   // T(n) = O(m), S(n) = O(m)
        char[] wrd2 = word2.toCharArray();                                   // T(n) = O(n), S(n) = O(n)
        final int m = wrd1.length;                                           // T(n) = O(1), S(n) = O(1)
        final int n = wrd2.length;                                           // T(n) = O(1), S(n) = O(1)

        int[] ans = new int[n];                                              // T(n) = O(n), S(n) = O(n)
        int[] last = new int[n];                                             // T(n) = O(n), S(n) = O(n)

        boolean canSkip = true;                                              // T(n) = O(1), S(n) = O(1)

        Arrays.fill(last, -1);                                               // T(n) = O(n), S(n) = O(1)

        int i = m - 1;                                                       // T(n) = O(1), S(n) = O(1)
        int j = n - 1;                                                       // T(n) = O(1), S(n) = O(1)

        while(i >= 0 && j >= 0) {
            if(wrd1[i] == wrd2[j]) {
                last[j--] = i;                                               // T(n) = O(1), S(n) = O(1)
            }
            i--;                                                             // T(n) = O(1), S(n) = O(1)
        }

        for(i = 0, j = 0; i < m; i++) {
            if(j == n) {
                break;                                                       // T(n) = O(1), S(n) = O(1)
            }

            if(wrd1[i] == wrd2[j]) {
                ans[j++] = i;                                                // T(n) = O(1), S(n) = O(1)
            } else if(canSkip && (j == n - 1 || i < last[j + 1])) {
                canSkip = false;                                             // T(n) = O(1), S(n) = O(1)
                ans[j++] = i;                                                // T(n) = O(1), S(n) = O(1)
            }
        }

        return j == n ? ans : new int[0];                                    // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(m, n) = O(m + n), where m = length of word1, and n = length of word2

# Space Complexity:
S(m, n) = O(m + n), where m = length of word1, and n = length of word2
