# 3614. Process String with Special Operations II

# Problem Summary:
You are given a string s consisting of lowercase English letters and the special characters:
1. '*' : removes the last character from the current result, if it exists
2. '#' : duplicates the current result and appends it to itself
3. '%' : reverses the current result
The string is processed from left to right to build a final result string. You are also given an integer k. Return the kth character of the final result string. If k is outside the bounds of the final string, return '.'.

# Approach Used:
Constructing the final string directly is not feasible because the '#' operation can cause the string length to grow exponentially. Instead, the solution works in two phases:

1) Calculate the Final Length
Traverse the string from left to right and compute only the length of the resulting string after each operation.
* Letters increase the length by 1.
* '*' decreases the length by 1 if the current length is greater than 0.
* '#' doubles the current length.
* '%' does not change the length.

After processing all characters if k is greater than or equal to the final length, return '.' immediately.

2) Reverse the Operations
Starting from the end of the input string, trace backwards to determine which original character occupies position k. Instead of reconstructing the string, each operation is reversed:
For '#':
* The string was duplicated.
* If k lies in the second half, map it back to the corresponding position in the first half.
* Then halve the length.
For '%':
* The string was reversed.
* Convert k to its position before the reversal.
For '*':
* A character had been removed.
* Restore the previous length.
For a letter:
* If k points to the last character of the current string, that letter is the answer.
* Otherwise, remove the letter from consideration and continue.

This allows the solution to identify the kth character without ever building the final string.

# Steps:
1) Traverse the string from left to right and calculate the final length
2) If k is outside the final length:
   * Return '.'
3) Traverse the string from right to left
4) Reverse each operation:
   * '*' restores one deleted position
   *  '#' maps k back into the original half and halves the length
   * '%' converts k to its position before reversal
   * Letters are checked to determine whether they correspond to position k
5) Return the matching character when found
6) If no character is found, return '.'

# Solution:

```
class Solution {

    public char processStr(String s, long k) {

        char[] chars = s.toCharArray();          // T(n) = O(n), S(n) = O(n)
        long totalLen = 0;                       // T(n) = O(1), S(n) = O(1)

        // Loop runs n times
        for(char c : chars) {
            if(c == '*') {
                if(totalLen > 0) {
                    totalLen--;
                }
            }else if(c == '#') {
                totalLen += totalLen;
            }else if(c != '%') {
                totalLen++;
            }                                    // T(n) = O(1) per execution, total executions = O(n)
        }

        if(k >= totalLen) {
            return '.';
        }

        // Loop runs n times
        for(int i = s.length() - 1; i >= 0; i--) {
            if(chars[i] == '*') {
                totalLen++;                     
            }else if(chars[i] == '#') {
                if(k >= totalLen / 2) {
                    k -= totalLen / 2;
                }
                totalLen /= 2;                  
            }else if(chars[i] == '%') {
                k = (totalLen - 1) - k;
            }else {
                if(k == totalLen - 1) {
                    return chars[i];
                }
                totalLen--;
            }                                    // T(n) = O(1) per execution, total executions = O(n)
        }

        return '.';                              // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
