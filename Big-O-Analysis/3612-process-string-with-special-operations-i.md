# 3612. Process String with Special Operations I

# Problem Summary:
You are given a string consisting of lowercase English letters and the special characters '*', '#', and '%'. Build a new string by processing the input from left to right using the following rules:
* Lowercase letter -> append it to the result.
* '*' -> remove the last character from the result, if it exists.
* '#' -> duplicate the current result and append it to itself.
* '%' -> reverse the current result.
Return the final processed string.

# Approach Used:
The solution uses a StringBuilder to efficiently build and modify the result while scanning the input string from left to right. For each character:
* If it is a letter, append it to the StringBuilder.
* If it is '*', remove the last character if the StringBuilder is not empty.
* If it is '#', append the current StringBuilder to itself, effectively duplicating the result.
* If it is '%', reverse the StringBuilder.
Finally, convert the StringBuilder into a String and return it.

# Steps:
1. Create an empty StringBuilder.
2. Convert the input string into a character array.
3. Traverse each character:
   * If it is a letter, append it.
   * If it is '*', delete the last character if one exists.
   * If it is '#', duplicate the current result by appending it to itself.
   * If it is '%', reverse the current result.
4. Return the final string.

# Solution:

```
class Solution {
    public String processStr(String s) {
        StringBuilder sb = new StringBuilder();                  // T(n) = O(1), S(n) = O(1)
        char[] chars = s.toCharArray();                          // T(n) = O(n), S(n) = O(n)

        for(char c : chars) {
            if(Character.isLetter(c)) {
                sb.append(c);                                    // T(n) = O(1)
            } else if(c == '*' && sb.length() > 0) {
                sb.deleteCharAt(sb.length() - 1);                // T(n) = O(1)
            } else if(c == '#') {
                sb.append(sb);                                   // T(n) = O(k), where k = current length
            } else if(c == '%') {
                sb.reverse();                                    // T(n) = O(k), where k = current length
            }
        }
        return sb.toString();                                    // T(n) = O(m), where m = current length
    }
}
```

# Time Complexity:
T(n) = O(m)

# Space Complexity:
S(n) = O(n + m)
