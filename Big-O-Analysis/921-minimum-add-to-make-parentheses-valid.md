# 921. Minimum Add to Make Parentheses Valid

# Problem Summary

Given a parentheses string s, return the minimum number of parentheses that must be inserted to make the string valid.

A valid parentheses string must have every opening parenthesis '(' matched with a closing parenthesis ')', and every closing parenthesis must have a corresponding opening parenthesis before it.

# Approach Used

The solution keeps track of the number of currently unmatched opening parentheses using open.

When an opening parenthesis is encountered, open is increased.

When a closing parenthesis is encountered:
- If there is an unmatched opening parenthesis, it is matched and open is decreased.
- Otherwise, this closing parenthesis has no matching opening parenthesis, so one opening parenthesis must be inserted before it. Therefore, ans is increased.

After processing the entire string, any remaining unmatched opening parentheses need one closing parenthesis each. Therefore, the final answer is ans + open.

# Steps

1. Initialize open to count unmatched opening parentheses.
2. Initialize ans to count opening parentheses that need to be inserted for unmatched closing parentheses.
3. Traverse the string.
4. For '(', increment open.
5. For ')':
   - If open > 0, decrement open because the closing parenthesis matches an existing opening parenthesis.
   - Otherwise, increment ans because an opening parenthesis must be inserted.
6. After the traversal, add open to ans because every remaining unmatched opening parenthesis needs one closing parenthesis.
7. Return ans + open.

# Solution

```
class Solution {
   public int minAddToMakeValid(String s) {
       int open = 0;                                                         // T(n) = O(1), S(n) = O(1)
       int ans = 0;                                                          // T(n) = O(1), S(n) = O(1)

       for(char c : s.toCharArray()) {                                       // T(n) = O(n), S(n) = O(n)
           if(c == '(') {                                                    // T(n) = O(1)
               open++;                                                       // T(n) = O(1)
           }else{
               if(open > 0) {                                                // T(n) = O(1)
                   open--;                                                   // T(n) = O(1)
               }else{
                   ans++;                                                    // T(n) = O(1)
               }
           }
       }
       return ans + open;                                                    // T(n) = O(1)
   }
}


# Time Complexity
T(n) = O(n)

# Space Complexity
S(n) = O(n)
