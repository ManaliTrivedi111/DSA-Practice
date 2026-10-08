# 3498. Reverse Degree of a String

# Problem Summary

Given a string s, calculate its reverse degree.

For each character:
1. Determine its position in the reversed alphabet ('a' = 26, 'b' = 25, ..., 'z' = 1).
2. Multiply this value by the character's position in the string (1-indexed).
3. Add all these products together.

Return the total reverse degree.

# Approach Used

1. Convert the string to a character array for direct access to each character.
2. Iterate through the characters using their zero-based index i.
3. Calculate the reverse alphabet position using: ('z' - str[i] + 1)
4. Multiply the reverse alphabet position by the character's 1-indexed position (i + 1).
5. Add the result to sum.
6. Return the final sum.

# Steps

1. Convert s to a character array.
2. Store the length of the string in n.
3. Initialize sum to 0.
4. Traverse the character array from left to right.
5. For each character:
   * Calculate its reverse alphabet position.
   * Multiply it by its 1-indexed position.
   * Add the product to sum.
6. Return sum.

# Solution

```
class Solution {
    public int reverseDegree(String s) {
        char[] str = s.toCharArray();                                        // T(n) = O(n), S(n) = O(n)
        final int n = str.length;                                            // T(n) = O(1), S(n) = O(1)
        int sum = 0;                                                         // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n), S(n) = O(1)
            sum += ('z' - str[i] + 1) * (i + 1);                             // T(n) = O(1), S(n) = O(1)
        }
        return sum;                                                          // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
