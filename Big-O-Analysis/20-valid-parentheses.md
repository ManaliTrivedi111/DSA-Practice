# 20. Valid Parentheses

# Problem Summary

Given a string s containing only (, ), {, }, [ and ], determine whether the string is valid.

A string is valid if:
1. Every opening bracket is closed by the same type of bracket.
2. Brackets are closed in the correct order.
3. Every closing bracket has a corresponding opening bracket.

# Approach Used

Use a stack to keep track of opening brackets.

- When an opening bracket is encountered, push it onto the stack.
- When a closing bracket is encountered:
  - If the stack is empty, there is no corresponding opening bracket, so return false.
  - If the top of the stack is not the matching opening bracket, return false.
  - Otherwise, remove the matching opening bracket using pop().
- After processing the entire string, the stack must be empty for the string to be valid.

# Steps

1. Create an empty stack.
2. Convert the string to a character array and process each character.
3. If the character is ), check that the stack contains ( at the top.
4. If the character is }, check that the stack contains { at the top.
5. If the character is ], check that the stack contains [ at the top.
6. For a matching closing bracket, remove the opening bracket from the stack.
7. For an opening bracket, push it onto the stack.
8. After processing all characters, return whether the stack is empty.

# Solution

```
class Solution {
   public boolean isValid(String s) {
       Deque<Character> stack = new ArrayDeque<>();                          // T(n) = O(1), S(n) = O(1)

       for(char c : s.toCharArray()) {                                       // T(n) = O(n), S(n) = O(n)
           if(c == ')') {                                                    // T(n) = O(1), S(n) = O(1)
               if(stack.isEmpty() || stack.peek() != '(') {                  // T(n) = O(1), S(n) = O(1)
                   return false;                                             // T(n) = O(1), S(n) = O(1)
               }else{                                                        // T(n) = O(1), S(n) = O(1)
                   stack.pop();                                              // T(n) = O(1), S(n) = O(1)
               } 
           }else if(c == '}') {                                              // T(n) = O(1), S(n) = O(1)
               if(stack.isEmpty() || stack.peek() != '{') {                  // T(n) = O(1), S(n) = O(1)
                   return false;                                             // T(n) = O(1), S(n) = O(1)
               }else{                                                        // T(n) = O(1), S(n) = O(1)
                   stack.pop();                                              // T(n) = O(1), S(n) = O(1)
               }
           }else if(c == ']') {                                              // T(n) = O(1), S(n) = O(1)
               if(stack.isEmpty() || stack.peek() != '[') {                  // T(n) = O(1), S(n) = O(1)
                   return false;                                             // T(n) = O(1), S(n) = O(1)
               }else{                                                        // T(n) = O(1), S(n) = O(1)
                   stack.pop();                                              // T(n) = O(1), S(n) = O(1)
               }
           }else{                                                            // T(n) = O(1), S(n) = O(1)
               stack.push(c);                                                // T(n) = O(1), S(n) = O(1)
           }
       }
       return stack.isEmpty();                                               // T(n) = O(1), S(n) = O(1)
   }
}


# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
