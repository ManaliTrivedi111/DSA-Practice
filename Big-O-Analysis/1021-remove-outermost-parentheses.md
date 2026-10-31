# 1021. Remove Outermost Parentheses

# Problem Summary

Given a valid parentheses string s, decompose it into primitive valid parentheses strings and remove the outermost pair of parentheses from every primitive string.

Return the resulting string.

# Approach Used

The solution keeps track of the current nesting depth using open.

For an opening parenthesis '(':
- Increment open.
- Append it only when open > 1, because the first opening parenthesis of a primitive string is its outermost parenthesis.

For a closing parenthesis ')':
- Decrement open.
- Append it only when open > 0, because when open becomes 0, this closing parenthesis is the outermost closing parenthesis of the primitive string.

A StringBuilder is used to construct the result efficiently.

# Steps

1. Initialize a StringBuilder for the result.
2. Initialize open to 0 to track the current nesting depth.
3. Traverse every character of the string.
4. For '(', increment open and append the character only if the new depth is greater than 1.
5. For ')', decrement open and append the character only if the new depth is greater than 0.
6. Return the constructed string.

# Solution

```
class Solution {
   public String removeOuterParentheses(String s) {
       StringBuilder sb = new StringBuilder();                               // T(n) = O(1), S(n) = O(1)
       int open = 0;                                                         // T(n) = O(1), S(n) = O(1)

       for(char c : s.toCharArray()) {                                       // T(n) = O(n), S(n) = O(n)
           if(c == '(') {                                                    // T(n) = O(1)
               open++;                                                       // T(n) = O(1)

               if(open > 1) {                                                // T(n) = O(1)
                   sb.append(c);                                             // T(n) = O(1) amortized
               }
           }else{
               open--;                                                       // T(n) = O(1)
               
               if(open > 0) {                                                // T(n) = O(1)
                   sb.append(c);                                             // T(n) = O(1) amortized
               }
           }
           
       }
       return sb.toString();                                                 // T(n) = O(n), S(n) = O(n)
   }
}
```

# Time Complexity
T(n) = O(n)

# Space Complexity
S(n) = O(n)

