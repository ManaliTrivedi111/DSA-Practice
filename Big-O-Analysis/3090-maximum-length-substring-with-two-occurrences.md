# 3090. Maximum Length Substring With Two Occurrences

# Problem Summary:

You are given a string s. Return the maximum length of a substring such that every character appears at most twice in that substring.

# Approach Used:

The solution uses the sliding window technique. We maintain a window from index i to index j. The window must satisfy the condition that every character appears at most 2 times. 

The count array stores the frequency of each character inside the current window. 
For every character at index j, we add it to the window and increment its frequency. 
If the frequency of cs[j] becomes greater than 2, the current window is invalid. 
We then move the left pointer i forward and decrease the frequency of each character that leaves the window. We continue shrinking the window until the frequency of cs[j] becomes at most 2.
At that point, the window [i, j] is valid, so we calculate its length: j - i + 1 and update ans with the maximum length found so far.

The important point is that both pointers only move forward. Therefore, although there is a while loop inside the for loop, the total number of times the left pointer moves is at most n.

# Steps:

1. Convert the string into a character array.
2. Create a count array to store the frequency of each character in the current window.
3. Initialize two pointers, i and j, to represent the sliding window.
4. Move j from left to right through the string.
5. Increment the frequency of cs[j].
6. If its frequency becomes greater than 2, move i forward.
7. Decrease the frequency of every character removed from the left side.
8. Continue until the current window contains at most two occurrences of every character.
9. Calculate the current window length using j - i + 1.
10. Update ans with the maximum window length.
11. Return ans.

# Solution:

```
class Solution {
    public int maximumLengthSubstring(String s) {
        char[] cs = s.toCharArray();                                         // T(n) = O(n), S(n) = O(n)
        final int n = cs.length;                                             // T(n) = O(1), S(n) = O(1)
        int[] count = new int[123];                                          // T(n) = O(1), S(n) = O(1)
        int ans = 0;                                                         // T(n) = O(1), S(n) = O(1)

        for(int i = 0, j = 0; j < n; j++) {
            count[cs[j]]++;                                                  // T(n) = O(1), S(n) = O(1)

            while(count[cs[j]] > 2) {
                count[cs[i]]--;                                              // T(n) = O(1), S(n) = O(1)
                i++;                                                         // T(n) = O(1), S(n) = O(1)
            }

            ans = Math.max(ans, j - i + 1);                                  // T(n) = O(1), S(n) = O(1)
        }

        return ans;                                                          // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
