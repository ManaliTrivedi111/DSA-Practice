# 1541. Minimum Insertions to Balance a Parentheses String

# Problem Summary

Given a parentheses string s containing only '(' and ')', return the minimum number of insertions needed to make it balanced.

Each '(' must be matched with exactly two consecutive ')' characters.

# Approach Used

The solution scans the string from left to right while keeping track of the number of unmatched opening parentheses in open.

For each ')':
- If the next character is also ')', the pair is already complete, so the loop skips the next character.
- Otherwise, one ')' must be inserted to complete the required pair.
- After forming a '))' pair, it must match an existing '('. If there is no unmatched opening parenthesis, one '(' must be inserted.

For each '(', open is increased.

After processing the whole string, every remaining unmatched '(' needs two ')' characters, so 2 * open is added to the answer.

# Steps

1. Convert the string to a character array.
2. Initialize ans to count required insertions.
3. Initialize open to count unmatched '(' characters.
4. Traverse the character array.
5. When ')' is encountered:
   - If the next character is ')', skip it because the required pair already exists.
   - Otherwise, increment ans because one ')' must be inserted.
   - If an unmatched '(' exists, decrement open.
   - Otherwise, increment ans because one '(' must be inserted.
6. When '(' is encountered, increment open.
7. After the loop, add 2 * open to ans.
8. Return ans.

# Solution

```
class Solution {
   public int minInsertions(String s) {
       char[] str = s.toCharArray();                                         // T(n) = O(n), S(n) = O(n)
       final int n = str.length;                                             // T(n) = O(1), S(n) = O(1)
       int ans = 0;                                                          // T(n) = O(1), S(n) = O(1)
       int open = 0;                                                         // T(n) = O(1), S(n) = O(1)

       for(int i = 0; i < n; i++) {                                          // T(n) = O(n)
           if(str[i] == ')') {                                               // T(n) = O(1)
               if(i + 1 < n && str[i + 1] == ')') {                          // T(n) = O(1)
                   i++;                                                      // T(n) = O(1)
               }else{
                   ans++;                                                    // T(n) = O(1)
               }
               if(open > 0) {                                                // T(n) = O(1)
                   open--;                                                   // T(n) = O(1)
               }else{
                   ans++;                                                    // T(n) = O(1)
               }
           }else{
               open++;                                                       // T(n) = O(1)
           }
       }
       ans += open * 2;                                                      // T(n) = O(1)
       return ans;                                                           // T(n) = O(1)
   }
}
```

# Time Complexity
T(n) = O(n)

# Space Complexity
S(n) = O(n)
