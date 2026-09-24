# 940. Distinct Subsequences II

# Problem Summary:

You are given a string s. The task is to return the number of distinct non-empty subsequences of s. A subsequence is formed by deleting zero or more characters from the original string without changing the relative order of the remaining characters. Because the answer can be very large, it must be returned modulo 10^9 + 7.

# Approach Used:

The solution uses dynamic programming while keeping track of the most recent contribution associated with each character.

count represents the total number of distinct subsequences of the processed prefix of s, including the empty subsequence.

Initially, count = 1 because the empty subsequence is the only subsequence before processing any character.

For each character ch:
* Every existing subsequence can either exclude the current character or include it. Therefore, adding the current character appears to create count new subsequences, giving 2 * count total subsequences.
* However, if ch has appeared before, some of these subsequences are duplicates. The value stored in last[ch] represents the number of subsequences that were already created when the previous occurrence of ch was processed.
* Therefore, those duplicate subsequences are subtracted: subSeqCount = 2 * count - last[ch]

The value last[ch] is then updated to the old value of count, because that is the contribution associated with the current occurrence of the character.

The empty subsequence is included in count, but the problem asks for non-empty subsequences. Therefore, 1 is subtracted from the final count.

The + MOD terms ensure that the value remains non-negative before applying the modulo operation.

# Steps:

1. Convert s into a character array.
2. Create a last array of size 26 to store the previous contribution for each lowercase English letter.
3. Initialize count = 1, representing the empty subsequence.
4. Traverse the string character by character.
5. Convert the current character into an index from 0 to 25.
6. Calculate the new number of distinct subsequences as 2 * count - last[ch].
7. Apply modulo 10^9 + 7.
8. Update last[ch] with the old value of count.
9. Update count with the newly calculated number of distinct subsequences.
10. Subtract the empty subsequence from the final count.
11. Return the result.

# Solution:

```
class Solution {
    public int distinctSubseqII(String s) {
        char[] str = s.toCharArray();                                        // T(n) = O(n), S(n) = O(n)
        final int n = str.length;                                            // T(n) = O(1), S(n) = O(1)
        long[] last = new long[26];                                          // T(n) = O(1), S(n) = O(1)
        long subSeqCount = 0;                                                // T(n) = O(1), S(n) = O(1)
        long count = 1;                                                      // T(n) = O(1), S(n) = O(1)
        final int MOD = 1_000_000_007;                                       // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n)
            int ch = str[i] - 'a';                                           // T(n) = O(1), S(n) = O(1)
            subSeqCount = (2 * count - last[ch] + MOD) % MOD;                // T(n) = O(1), S(n) = O(1)
            last[ch] = count;                                                // T(n) = O(1), S(n) = O(1)
            count = subSeqCount;                                             // T(n) = O(1), S(n) = O(1)
        }
        return (int)(subSeqCount - 1 + MOD) % MOD;                           // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
