# 3963. Create Grid With Exactly One Path

# Problem Summary:

Construct an m x n grid containing only '.' and '#' such that exactly one valid path exists from the top-left cell to the bottom-right cell. A valid path can move only right or down.

# Approach Used:

Make the entire first row free. For every remaining row, place obstacles in the first n - 1 columns and a free cell in the last column.

This creates exactly one valid path: move right across the first row, then move down through the last column. Every other possible route is blocked because cells below the first row are obstacles except in the last column.

# Steps:

1. Create a StringBuilder and an answer array of size m.
2. Append n dots to the builder and store the resulting string as the first row.
3. Create a new StringBuilder.
4. Append n - 1 obstacles followed by one free cell. Convert it to a string named repeat.
5. Assign repeat to every remaining row.
6. Return the answer array.

# Solution:

```
class Solution {
    public String[] createGrid(int m, int n) {
        StringBuilder sb = new StringBuilder();       // T(m, n) = O(1), S(m, n) = O(1)
        String[] ans = new String[m];                 // T(m, n) = O(m), S(m, n) = O(m)

        for(int i = 0; i < n; i++) {                  // T(m, n) = O(n)
            sb.append(".");                           // T(m, n) = O(1) amortized
        }
        ans[0] = sb.toString();                       // T(m, n) = O(n), S(m, n) = O(n)
        sb = new StringBuilder();                     // T(m, n) = O(1), S(m, n) = O(1)

        for(int i = 0; i < n - 1; i++) {              // T(m, n) = O(n)
            sb.append("#");                           // T(m, n) = O(1) amortized
        }
        sb.append(".");                               // T(m, n) = O(1)
        String repeat = sb.toString();                // T(m, n) = O(n), S(m, n) = O(n)

        for(int i = 1; i < m; i++) {                  // T(m, n) = O(m)
            ans[i] = repeat;                          // T(m, n) = O(1)
        }
        return ans;                                   // T(m, n) = O(1)
    }
}
```

# Time Complexity:
T(m, n) = O(m + n)

# Space Complexity:
S(m, n) = O(m + n)

