# 856. Score of Parentheses

# Problem Summary

Given a balanced parentheses string s, return its score.

The score follows these rules:
- "()" has score 1.
- AB has score A + B, where A and B are balanced parentheses strings.
- (A) has score 2 * A, where A is a balanced parentheses string.

# Approach Used

The solution scans the string from left to right while keeping track of the current nesting depth in layer.

Whenever "()" is found, its score is 1, multiplied by 2 for every enclosing pair of parentheses. Therefore, its contribution is 2^layer, implemented as 1 << layer.

The solution adds this contribution directly to ans.

# Steps

1. Convert the string to a character array.
2. Initialize ans to store the total score and layer to store the current nesting depth.
3. Scan the string while checking each character together with the next character.
4. If the current two characters are "()", add 2^layer to ans.
5. Update layer:
   - Increase it when the current character is '('.
   - Decrease it when the current character is ')'.
6. Return ans.

# Solution

```
class Solution {
   public int scoreOfParentheses(String s) {
       char[] str = s.toCharArray();                                         // T(n) = O(n), S(n) = O(n)
       final int n = str.length;                                             // T(n) = O(1), S(n) = O(1)
       int ans = 0;                                                          // T(n) = O(1), S(n) = O(1)
       int layer = 0;                                                        // T(n) = O(1), S(n) = O(1)

       for(int i = 0; i + 1 < n; i++) {                                      // T(n) = O(n)
           if(str[i] == '(' && str[i + 1] == ')') {                          // T(n) = O(1)
               ans += 1 << layer;                                            // T(n) = O(1)
           }   
           layer += (str[i] == '(') ? 1 : -1;                                // T(n) = O(1)
       }
       return ans;                                                           // T(n) = O(1)
   }
}
```

# Time Complexity
T(n) = O(n)

# Space Complexity
S(n) = O(n)

 
