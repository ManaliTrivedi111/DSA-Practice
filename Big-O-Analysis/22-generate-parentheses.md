# 22. Generate Parentheses

# Problem Summary

Given n pairs of parentheses, generate all combinations of well-formed parentheses.

# Approach Used

Use depth-first search (DFS) with backtracking.

The recursive function keeps track of:
- l: number of opening parentheses ( still available.
- r: number of closing parentheses ) still available.

At every step:
- An opening parenthesis can be added whenever l > 0.
- A closing parenthesis can be added only when l < r. This ensures that we never place more closing parentheses than opening parentheses, so every generated sequence remains valid.
- When both l and r become 0, a complete valid parentheses string has been generated and is added to result.

A StringBuilder is reused during backtracking. After each recursive call, the last character is removed so that the same StringBuilder can be used for the next choice.

# Steps

1. Create an empty result list.
2. Start DFS with an empty StringBuilder and l = r = n.
3. If l == 0 and r == 0, convert the current StringBuilder to a string and add it to result.
4. If l > 0, append (, recursively explore, and remove the appended character.
5. If l < r, append ), recursively explore, and remove the appended character.
6. Return the generated list.

# Solution

```
class Solution {
   public List<String> generateParenthesis(int n) {
       List<String> result = new ArrayList<>();                              // T(n) = O(1), S(n) = O(1)
       dfs(new StringBuilder(), result, n, n);                               // T(n) = O(C * n), S(n) = O(n)
       return result;                                                        // T(n) = O(1), S(n) = O(1)
   }

   private void dfs(StringBuilder sb, List<String> result, int l, int r) {
       if(l == 0 && r == 0) {                                                // T(n) = O(1), S(n) = O(1)
           result.add(sb.toString());                                        // T(n) = O(n), S(n) = O(n)
           return;                                                           // T(n) = O(1), S(n) = O(1)
       }

       if(l > 0) {                                                           // T(n) = O(1), S(n) = O(1)
           sb.append("(");                                                   // T(n) = O(1), S(n) = O(1)
           dfs(sb, result, l - 1, r);                                        // T(n) = recursive, S(n) = O(n)
           sb.deleteCharAt(sb.length() - 1);                                 // T(n) = O(n), S(n) = O(1)
       }

       if(l < r) {                                                           // T(n) = O(1), S(n) = O(1)
           sb.append(")");                                                   // T(n) = O(1), S(n) = O(1)
           dfs(sb, result, l, r - 1);                                        // T(n) = recursive, S(n) = O(n)
           sb.deleteCharAt(sb.length() - 1);                                 // T(n) = O(n), S(n) = O(1)
       }
   }
}
```

# Time Complexity:
T(n) = O(n * C), where C = number of valid combinations of n pairs of parentheses

# Space Complexity:
S(n) = O(n * C), where C = number of valid combinations of n pairs of parentheses
