# 1358. Number of Substrings Containing All Three Characters

# Problem Summary:
You are given a string s consisting only of the characters 'a', 'b', and 'c'. Return the number of substrings that contain at least one occurrence of each of the three characters.

# Approach Used:
The solution keeps track of the most recent index where each of the three characters ('a', 'b', and 'c') has appeared. As the string is traversed:
* Update the last seen index of the current character.
* Find the smallest among the last seen indices of 'a', 'b', and 'c'.

If all three characters have been seen, the minimum last seen index represents the earliest position that must be included in the substring to contain all three characters. Therefore, every starting index from 0 to minIdx forms a valid substring ending at the current position.
The number of such substrings is: minIdx + 1
Initially, the last seen indices are set to -1. Until all three characters have appeared, the minimum index remains -1, contributing 0 valid substrings.

# Steps:
1. Create an array to store the last seen index of 'a', 'b', and 'c'.
2. Initialize all last seen indices to -1.
3. Traverse the string from left to right.
4. Update the last seen index of the current character.
5. Find the minimum among the three last seen indices.
6. Add minIdx + 1 to the answer.
7. Return the total number of valid substrings.

# Solution:

```
class Solution {
    public int numberOfSubstrings(String s) {
        int total = 0;                                      // T(n) = O(1), S(n) = O(1)
        char[] str = s.toCharArray();                       // T(n) = O(n), S(n) = O(n)
        final int n = str.length;                           // T(n) = O(1), S(n) = O(1)
        int[] abc = new int[3];                             // T(n) = O(1), S(n) = O(1)
        int i = 0;                                          // T(n) = O(1), S(n) = O(1)

        for(int j = 0; j < 3; j++) {
            abc[j] = -1;                                    // T(n) = O(1) per iteration
        }

        while(i < n) {
            abc[str[i] - 'a'] = i;                          // T(n) = O(1) per iteration
            int minIdx = n;                                 // T(n) = O(1) per iteration

            for(int j = 0; j < 3; j++) {
                minIdx = Math.min(minIdx, abc[j]);          // T(n) = O(1) per iteration
            }
            total += minIdx + 1;                            // T(n) = O(1) per iteration
            i++;                                            // T(n) = O(1) per iteration
        }
        return total;                                       // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
