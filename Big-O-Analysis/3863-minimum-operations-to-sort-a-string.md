# 3863. Minimum Operations to Sort a String

# Problem Summary:

Given a lowercase string s, one operation allows sorting any proper substring (a substring that is not the entire string) in non-descending alphabetical order.

Return the minimum number of operations needed to sort s, or -1 if it is impossible.

# Approach Used:

Traverse the string once to find its minimum character (mn), maximum character (mx), and determine whether it is already sorted.

Then use the positions of the minimum and maximum characters to determine the answer:
1. If the string is already sorted, return 0.
2. If its length is 2 and it is unsorted, return -1, because the only substring that could change the order is the entire string, which is not allowed.
3. If the first character is the minimum or the last character is the maximum, return 1.
4. Otherwise, if the minimum or maximum character occurs in the middle, return 2.
5. If none of these conditions applies, return 3.

# Steps:

1. Initialize mx to 'a', mn to 'z', and sorted to true.
2. Traverse the string, updating the minimum and maximum characters.
3. During the same traversal, check whether any adjacent pair is out of order. If so, mark the string as unsorted.
4. Return 0 if the string is already sorted.
5. Return -1 if the unsorted string has length 2.
6. Return 1 if the first character is the minimum or the last character is the maximum.
7. Check the middle characters. Return 2 if one of them is the minimum or maximum.
8. Otherwise, return 3.

# Solution:

```
class Solution {
    public int minOperations(String s) {
        final int n = s.length();                                            // T(n) = O(1), S(n) = O(1)
        char mx = 'a';                                                       // T(n) = O(1), S(n) = O(1)
        char mn = 'z';                                                       // T(n) = O(1), S(n) = O(1)
        boolean sorted = true;                                               // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n)
            mx = (char) Math.max(mx, s.charAt(i));                           // T(n) = O(1)
            mn = (char) Math.min(mn, s.charAt(i));                           // T(n) = O(1)

            if(i > 0 && s.charAt(i - 1) > s.charAt(i)) {                     // T(n) = O(1)
                sorted = false;                                              // T(n) = O(1)
            }
        }
        if(sorted) {                                                         // T(n) = O(1)
            return 0;                                                        // T(n) = O(1)
        }
        if(n == 2) {                                                         // T(n) = O(1)
            return -1;                                                       // T(n) = O(1)
        }
        if(s.charAt(0) == mn || s.charAt(n - 1) == mx) {                     // T(n) = O(1)
            return 1;                                                        // T(n) = O(1)
        }
        for(int i = 1; i < n - 1; i++) {                                     // T(n) = O(n)
            if(s.charAt(i) == mn || s.charAt(i) == mx) {                     // T(n) = O(1)
                return 2;                                                    // T(n) = O(1)
            }
        }
        return 3;                                                            // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
