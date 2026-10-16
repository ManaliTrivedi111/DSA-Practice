# 1190. Reverse Substrings Between Each Pair of Parentheses

# Problem Summary

You are given a string s consisting of lowercase English letters and parentheses.

For every matching pair of parentheses, reverse the string inside the pair, starting with the innermost pair.

Return the resulting string without any parentheses.

# Approach Used

The solution uses a stack to store the current string segments.

The input is scanned from left to right. Consecutive letters are processed together rather than one at a time.
- When an opening parenthesis ( is encountered, the substring immediately following it is pushed onto the stack.
- When letters are encountered, they are appended to the current top stack string.
- When a closing parenthesis ) is encountered, the top string is popped, reversed, and then appended to the surrounding string.
- Because the innermost parenthesis is completed first, reversing occurs naturally from the inside out.

At the end, the only remaining stack element is the final result.

# Steps

1. Create a stack to store the current string segments.
2. Scan the input using index i.
3. When the current character is not ):
   - Find the end of the consecutive letter sequence.
   - If the current character is (, push the characters after it onto the stack.
   - Otherwise, append the letters to the current stack segment.
4. When ) is encountered:
   - Pop the innermost string from the stack.
   - Reverse it using StringBuilder.reverse().
   - Append the reversed string to the surrounding stack segment.
5. Continue until the entire string has been processed.
6. Return the final string from the stack.

# Solution

```
class Solution {
    public String reverseParentheses(String s) {
        Deque<String> stack = new ArrayDeque<>();                            // T(n) = O(1), S(n) = O(1)
        int i = 0;                                                           // T(n) = O(1), S(n) = O(1)
        final int n = s.length();                                            // T(n) = O(1), S(n) = O(1)

        while(i < n) {                                                       // T(n) = O(n)
            if(s.charAt(i) != ')') {                                         // T(n) = O(1), S(n) = O(1)
                int j = i + 1;                                               // T(n) = O(1), S(n) = O(1)

                while(j < n && Character.isLetter(s.charAt(j))) {            // T(n) = O(n) total
                    j++;                                                     // T(n) = O(1), S(n) = O(1)
                }

                if(s.charAt(i) == '(') {                                     // T(n) = O(1), S(n) = O(1)
                    stack.push(s.substring(i + 1, j));                       // T(n) = O(L), S(n) = O(L)
                }else{
                    if(stack.isEmpty()) {                                    // T(n) = O(1), S(n) = O(1)
                        stack.push(s.substring(i, j));                       // T(n) = O(L), S(n) = O(L)
                    }else{
                        stack.push(stack.pop() + s.substring(i, j));         // T(n) = O(L), S(n) = O(L)
                    }
                }
                i = j;                                                       // T(n) = O(1), S(n) = O(1)
            }else {
                StringBuilder sb = new StringBuilder(stack.pop());           // T(n) = O(L), S(n) = O(L)
                sb.reverse();                                                // T(n) = O(L), S(n) = O(1)
                String str = sb.toString();                                  // T(n) = O(L), S(n) = O(L)

                if(!stack.isEmpty()) {                                       // T(n) = O(1), S(n) = O(1)
                    stack.push(stack.pop() + str);                           // T(n) = O(L), S(n) = O(L)
                }else{
                    stack.push(str);                                         // T(n) = O(1), S(n) = O(1)
                }
                i++;                                                         // T(n) = O(1), S(n) = O(1)
            }
        }
        return stack.pop();                                                  // T(n) = O(n), S(n) = O(n)
    }
}


# Time Complexity:
T(n) = O(n²)

# Space Complexity:
S(n) = O(n)
