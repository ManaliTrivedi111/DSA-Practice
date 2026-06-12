# 766. Toeplitz Matrix

# Problem Summary: 
Given an m x n matrix, return true if the matrix is Toeplitz. Otherwise, return false. A matrix is Toeplitz if every diagonal from top-left to bottom-right has the same elements.

# Approach Used: 
The solution checks whether every element matches the element diagonally above-left to it. Since the first row and first column do not have any top-left diagonal element, the traversal starts from index 1 for both rows and columns. If any mismatch is found, the method immediately returns false. Otherwise, after checking the entire matrix, it returns true.

# Steps:
1) Store the number of rows and columns
2) Traverse the matrix starting from row 1 and column 1
3) Compare each element with its top-left diagonal element
4) If any mismatch is found -> return false
5) If all diagonal elements match -> return true

# Solution:
```
class Solution {

    public boolean isToeplitzMatrix(int[][] matrix) {
        final int m = matrix.length;                        // T(n) = O(1), S(n) = O(1)
        final int n = matrix[0].length;                     // T(n) = O(1), S(n) = O(1)

        // Outer loop runs (m - 1) times
        for(int i = 1; i < m; i++) {

            // Inner loop runs (n - 1) times for each row
            for(int j = 1; j < n; j++) {
                if(matrix[i][j] != matrix[i - 1][j - 1]) {  // T(n) = O(1) per iteration = O((m - 1) * (n - 1))
                    return false;                           // T(n) = O(1)
                }
            }
        }
        return true;                                        // T(n) = O(1)
    }
}
```
# Time Complexity:

T(n) = O(1) + O(1) + O((m - 1) * (n - 1)) + O(1)
     = O(3 + mn - m - n + 1)
     = O(mn + 4 - m - n)
     = O(mn) // Keep dominant term only

# Space Complexity:

S(n) = O(1) + O(1)
     = O(2)
     = O(1) // Drop constant multiplier
