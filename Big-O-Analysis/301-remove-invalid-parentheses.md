# 301. Remove Invalid Parentheses

# Problem Summary

Given a string s containing parentheses and letters, remove the minimum number of invalid parentheses to make the string valid.

Return all unique valid strings that can be obtained using the minimum number of removals.

# Approach Used

The solution uses backtracking to try removing parentheses while keeping track of how many opening and closing parentheses are still available.

The temp array stores the current state of the string. A removed character is represented by -1.

The solution avoids generating duplicate states by only considering the removal of the first parenthesis in a consecutive group of identical parentheses.

Whenever the remaining number of opening and closing parentheses is equal, valid() checks whether the current string is actually valid. If it is, add() builds the resulting string and stores it in a HashSet to ensure uniqueness.

The solution also keeps track of minRemoved, so branches that remove more parentheses than the best solution found so far are pruned.

# Steps

1. Initialize the HashSet for the answers and count the total number of opening and closing parentheses.
2. Store every character of s in the temp array.
3. Start the recursive find() method with the total counts of opening and closing parentheses.
4. At each recursive step:
   - Stop if the number of remaining opening or closing parentheses becomes negative.
   - Stop if the number of removals exceeds the current minimum.
5. When the remaining opening and closing counts are equal, call valid() to check whether the current string is valid.
6. If the string is valid, call add():
   - Build the current string.
   - Clear previous results if fewer removals were used.
   - Add the string to the HashSet.
7. Otherwise, try removing an opening or closing parenthesis.
8. Skip consecutive identical parentheses when choosing a character to remove, which reduces duplicate states.
9. Convert the HashSet to the final answer list.

# Solution

```
import java.util.*;

class Solution {
   public List<String> removeInvalidParentheses(String s) {
       return new AbstractList<String>() {
           private int minRemoved;                                           // T(n) = O(1), S(n) = O(1)
           private int[] temp;                                               // T(n) = O(1), S(n) = O(n)
           private Set<String> ans;                                          // T(n) = O(1), S(n) = O(R)
           private List<String> finalAns;                                    // T(n) = O(1), S(n) = O(R)
           private int totalOpen;                                            // T(n) = O(1), S(n) = O(1)
           private int totalClose;                                           // T(n) = O(1), S(n) = O(1)

           public String get(int index) {                                    
               if(finalAns == null) {                                        // T(n) = O(1)
                   solve(s);                                                 // T(n) = O(F * n), S(n) = O(n + R * n)
               }
               return finalAns.get(index);                                   // T(n) = O(1)
           }

           public int size() {                                               
               if(finalAns == null) {                                        // T(n) = O(1)
                   solve(s);                                                 // T(n) = O(F * n), S(n) = O(n + R * n)
               }
               return finalAns.size();                                       // T(n) = O(1)
           }

           private void solve(String s) {
               final int n = s.length();                                     // T(n) = O(1), S(n) = O(1)
               ans = new HashSet<>();                                        // T(n) = O(1), S(n) = O(1)
               totalOpen = 0;                                                // T(n) = O(1), S(n) = O(1)
               totalClose = 0;                                               // T(n) = O(1), S(n) = O(1)
               minRemoved = n;                                               // T(n) = O(1), S(n) = O(1)
               temp = new int[n];                                            // T(n) = O(n), S(n) = O(n)

               for(int i = 0; i < n; i++) {                                  // T(n) = O(n)
                   temp[i] = s.charAt(i);                                    // T(n) = O(1)

                   if(s.charAt(i) == '(') {                                  // T(n) = O(1)
                       totalOpen++;                                          // T(n) = O(1)
                   }else if(s.charAt(i) == ')') {                            // T(n) = O(1)
                       totalClose++;                                         // T(n) = O(1)
                   }
               }
               find(totalOpen, totalClose, 0, 0);                            // T(n) = O(F * n), S(n) = O(n)
               finalAns = new ArrayList<>(ans);                              // T(n) = O(R), S(n) = O(R)
           }

           private boolean valid() {
               int open = 0;                                                 // T(n) = O(1), S(n) = O(1)
               int close = 0;                                                // T(n) = O(1), S(n) = O(1)

               for(int num : temp) {                                         // T(n) = O(n)
                   if(num == -1) {                                           // T(n) = O(1)
                       continue;                                             // T(n) = O(1)
                   }
                   char ch = (char) num;                                     // T(n) = O(1)

                   if(ch == '(') {                                           // T(n) = O(1)
                       open++;                                               // T(n) = O(1)
                   }
                   if(ch == ')') {                                           // T(n) = O(1)
                       close++;                                              // T(n) = O(1)
                   }
                   if(close > open) {                                        // T(n) = O(1)
                       return false;                                         // T(n) = O(1)
                   }
               }
               return open == close;                                         // T(n) = O(1)
           }

           private void add(int removed) {
               if(minRemoved < removed) {                                    // T(n) = O(1)
                   return;
               }
               StringBuilder sb = new StringBuilder();                       // T(n) = O(1), S(n) = O(n)

               for(int num : temp) {                                         // T(n) = O(n)
                   if(num == -1) {                                           // T(n) = O(1)
                       continue;
                   }
                   sb.append((char) num);                                    // T(n) = O(1) amortized
               }
               if(minRemoved > removed) {                                    // T(n) = O(1)
                   ans.clear();                                              // T(n) = O(R)
                   minRemoved = removed;                                     // T(n) = O(1)
               }
               ans.add(sb.toString());                                       // T(n) = O(n) expected, S(n) = O(n)
           }

           private void find(int open, int close, int removed, int start) {
               if(open < 0 || close < 0 || removed > minRemoved) {           // T(n) = O(1)
                   return;
               }
               if(open == close && valid()) {                                // T(n) = O(n)
                   add(removed);                                             // T(n) = O(n)
                   return;
               }
               for(int i = start; i < temp.length; i++) {                    // T(n) = O(n)
                   if(temp[i] == -1) {                                       // T(n) = O(1)
                       continue;
                   }
                   char ch = (char) temp[i];                                 // T(n) = O(1)

                   if(ch == '(' && (i == 0 || (char) temp[i - 1] != '(' || temp[i - 1] == -1)) {
                       temp[i] = -1;                                         // T(n) = O(1)
                       find(open - 1, close, removed + 1, i); 
                       temp[i] = ch;                                         // T(n) = O(1)
                   }else if(ch == ')' && (i == 0 || (char) temp[i - 1] != ')' || temp[i - 1] == -1)) { 
                       temp[i] = -1;                                         // T(n) = O(1)
                       find(open, close - 1, removed + 1, i); 
                       temp[i] = ch;                                         // T(n) = O(1)
                   }
               }
           }
       };
   }
}
```


# Time Complexity
T(n) = O(F * n), where n = length of the input string, and F = number of recursive states explored by the backtracking

In the worst case,

T(n) = O(n * 2^n)

# Space Complexity
S(n) = O(n + R * n), where n = length of the input string, and R = number of valid strings produced
