# 3517. Smallest Palindromic Rearrangement I

# Problem Summary:

You are given a palindromic string s. You need to rearrange its characters to form the lexicographically smallest possible palindrome.

# Approach Used:

Because the input string is already a palindrome, the characters on the left half determine the characters on the right half.

For a palindrome:
* Every character appears an even number of times, except possibly one character when the length is odd.
* The middle character, if the length is odd, must remain in the middle.
* The remaining characters can be divided equally between the left and right halves.

To obtain the lexicographically smallest palindrome, we construct the left half in alphabetical order. The right half is then simply the reverse of the left half.

For example, if the left half is: aabc
then the right half must be: cbaa
so the resulting palindrome is: aabc...cbaa

Since the smallest characters are placed as early as possible, sorting the left half alphabetically produces the lexicographically smallest palindrome.

# Steps:

1. Convert the string to a character array.
2. If the length is odd, store the middle character separately.
3. Consider only the first half of the palindrome and count the frequency of each character.
4. Traverse the lowercase letters from 'a' to 'z'.
5. Append each character according to its frequency to construct the left half in sorted order.
6. Append the middle character, if one exists.
7. Reverse the left half and append it to form the right half.
8. Return the resulting palindrome.

# Solution:

```
class Solution {
    public String smallestPalindrome(String s) {
        char[] str = s.toCharArray();                                      // T(n) = O(n), S(n) = O(n)
        int n = str.length;                                                // T(n) = O(1), S(n) = O(1)
        int[] count = new int[123];                                        // T(n) = O(1), S(n) = O(1)
        String middle = n % 2 != 0 ? String.valueOf(str[n / 2]) : "";       // T(n) = O(1), S(n) = O(1)
        StringBuilder sb = new StringBuilder();                            // T(n) = O(1), S(n) = O(n)
        n /= 2;                                                            // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {
            count[str[i]]++;                                               // T(n) = O(1)
        }

        for(int i = 97; i <= 122; i++) {
            if(count[i] > 0) {
                char c = (char)i;                                          // T(n) = O(1)

                for(int j = 0; j < count[i]; j++) {
                    sb.append(c);                                          // T(n) = O(1)
                }
            }
        }

        return sb.toString() + middle + sb.reverse().toString();           // T(n) = O(n), S(n) = O(n)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
