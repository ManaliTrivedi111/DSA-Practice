# 1614. Maximum Nesting Depth of the Parentheses

# Problem Summary

Given a valid parentheses string s, return its maximum nesting depth.

The nesting depth is the maximum number of parentheses that are open at the same time while scanning the string from left to right.

# Approach Used

Use a counter count to track the current number of open parentheses.
- When ( is encountered, increment count.
- Update ans if the current count is greater than the maximum depth seen so far.
- When ) is encountered, decrement count.
- Return ans after processing the entire string.

# Steps

1. Initialize count = 0 to track the current nesting depth.
2. Initialize ans = 0 to store the maximum nesting depth.
3. Convert the string to a character array and scan each character.
4. For (:
   - Increment count.
   - Update ans if count is greater than the current maximum.
5. For ):
   - Decrement count.
6. Return ans.

# Solution

```
class Solution {
   public int maxDepth(String s) {
       int count = 0;                                                        // T(n) = O(1), S(n) = O(1)
       int ans = 0;                                                          // T(n) = O(1), S(n) = O(1)

       for(char c : s.toCharArray()) {                                       // T(n) = O(n), S(n) = O(n)
           if(c == '(') {                                                    // T(n) = O(1), S(n) = O(1)
               count++;                                                      // T(n) = O(1), S(n) = O(1)
               if(count > ans) {                                             // T(n) = O(1), S(n) = O(1)
                   ans = count;                                              // T(n) = O(1), S(n) = O(1)
               }
           }else if(c == ')') {                                              // T(n) = O(1), S(n) = O(1)
               count--;                                                      // T(n) = O(1), S(n) = O(1)
           }
       }
       return ans;                                                           // T(n) = O(1), S(n) = O(1)
   }
}
```


# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
