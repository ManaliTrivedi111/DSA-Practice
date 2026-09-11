# 2904. Shortest and Lexicographically Smallest Beautiful String

# Problem Summary:

You are given a binary string s and a positive integer k. A substring is beautiful if it contains exactly k occurrences of 1.

The goal is to find the shortest beautiful substring. If multiple beautiful substrings have the same shortest length, return the lexicographically smallest one.

If no beautiful substring exists, return an empty string.

# Approach Used:

The solution uses a sliding-window technique that focuses on the positions of the 1s in the string.

The variable curr keeps track of the number of 1s in the current window. The pointer i moves through the string, while the pointer j represents the left boundary of the current candidate substring.

When a 1 is encountered, it is added to the current window. If the window contains more than k ones, j is moved forward until the window contains exactly k ones again.

Whenever the current window contains exactly k ones, its length is compared with the shortest length found so far. If it is shorter, it becomes the new result. If it has the same length, compareTo() is used to keep the lexicographically smaller substring.

# Steps:

1. Convert s to a character array for efficient character access.
2. Initialize len to n + 1, so any valid beautiful substring will initially be shorter.
3. Traverse the string using pointer i.
4. Ignore 0s because they do not change the number of 1s.
5. When a 1 is found:
   * If this is the first 1 in the current window, set j to its position.
   * Increment curr.
6. If curr becomes greater than k, move j forward until exactly k ones remain.
7. When curr == k:
   * If the current substring is shorter than len, store it as the result.
   * If it has the same length as len, compare it lexicographically with the current result and keep the smaller one.
8. Return res. If no beautiful substring was found, it remains an empty string.

# Solution:

```
class Solution {
    public String shortestBeautifulSubstring(String s, int k) {
        char[] str = s.toCharArray();                                        // T(n) = O(n), S(n) = O(n)
        final int n = str.length;                                            // T(n) = O(1), S(n) = O(1)
        int len = n + 1;                                                     // T(n) = O(1), S(n) = O(1)
        int curr = 0;                                                        // T(n) = O(1), S(n) = O(1)
        String res = "";                                                     // T(n) = O(1), S(n) = O(1)

        for(int i = 0, j = 0; i < n; i++) {                                  // T(n) = O(n) overall
            if(str[i] == '0') { 
                continue;
            }
            if(curr == 0) {
                j = i;                                                       // T(n) = O(1) per execution
            }
            curr++;                                                          // T(n) = O(1)
            
            while(curr > k) {                                                // T(n) = O(n) overall
                if(str[++j] == '1') {
                    curr--;                                                  // T(n) = O(1) per execution
                }
            }
            if(curr == k) {
                if(i - j + 1 < len) {
                    len = i - j + 1;                                         // T(n) = O(1)
                    res = s.substring(j, i + 1);                             // T(n) = O(n)
                }else if(i - j + 1 == len) {
                    String sub = s.substring(j, i + 1);                      // T(n) = O(n), S(n) = O(n)

                    if(sub.compareTo(res) < 0) {
                        res = sub;                                           // T(n) = O(1)
                    }
                }
            }
        }
        return res;                                                          // T(n) = O(1)
    }
}

# Time Complexity:
T(n) = O(n²)

# Space Complexity:
S(n) = O(n)
