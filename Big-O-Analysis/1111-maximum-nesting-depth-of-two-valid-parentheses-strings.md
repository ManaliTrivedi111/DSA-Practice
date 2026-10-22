# 1111. Maximum Nesting Depth of Two Valid Parentheses Strings

# Problem Summary

Given a valid parentheses string seq, split its characters into two disjoint subsequences A and B such that both subsequences are valid parentheses strings.

The goal is to minimize: max(depth(A), depth(B))

Return an array where 0 means the character belongs to A and 1 means it belongs to B.

# Approach Used

The key idea is to distribute parentheses based on the parity of the current nesting depth.
- For an opening parenthesis (, assign it to the group corresponding to the current depth, then increase the depth.
- For a closing parenthesis ), first decrease the depth, then assign it to the group corresponding to the new depth.
- Using depth % 2 alternates the assignment between the two groups as the nesting depth changes.

This divides deeply nested parentheses between A and B, reducing the maximum nesting depth of either subsequence.

# Steps

1. Convert seq to a character array.
2. Create an answer array of the same length.
3. Maintain depth to represent the current nesting depth.
4. For each character:
   - If it is (:
     - Assign it to depth % 2.
     - Increment depth.
   - If it is ):
     - Decrement depth.
     - Assign it to depth % 2.
5. Return the answer array.

# Solution

```
class Solution {
   public int[] maxDepthAfterSplit(String seq) {
       char[] str = seq.toCharArray();                                       // T(n) = O(n), S(n) = O(n)
       final int n = str.length;                                             // T(n) = O(1), S(n) = O(1)
       int[] ans = new int[n];                                               // T(n) = O(n), S(n) = O(n)
       int depth = 0;                                                        // T(n) = O(1), S(n) = O(1)

       for(int i = 0; i < n; i++) {                                          // T(n) = O(n), S(n) = O(1)
           if(str[i] == '(') {                                               // T(n) = O(1), S(n) = O(1)
               ans[i] = depth % 2;                                           // T(n) = O(1), S(n) = O(1)
               depth++;                                                      // T(n) = O(1), S(n) = O(1)
           }else{                                                            // T(n) = O(1), S(n) = O(1)
               depth--;                                                      // T(n) = O(1), S(n) = O(1)
               ans[i] = depth % 2;                                           // T(n) = O(1), S(n) = O(1)
           }
       }
       return ans;                                                           // T(n) = O(1), S(n) = O(1)
   }
}
```

# Time Complexity:
T(n) = O(n).

# Space Complexity:
S(n) = O(n).
