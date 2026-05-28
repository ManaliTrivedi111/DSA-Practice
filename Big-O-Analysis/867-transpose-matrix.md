# 867. Transpose Matrix

# Problem Summary: Given a 2D integer array matrix of size m x n, return its transpose. The transpose of a matrix is formed by flipping the matrix over its main diagonal, meaning rows become columns and columns become rows. For every element at position matrix[i][j], it is placed at position trans[j][i] in the transposed matrix.

# Approach Used: The solution first creates a new matrix with reversed dimensions: n x m. It then traverses every element of the original matrix using two nested loops. For each element: matrix[i][j], it stores the value in: trans[j][i]. This effectively swaps row and column indices and constructs the transpose matrix.

# Steps:
1) Find the number of rows (m)
2) Find the number of columns (n)
3) Create a new matrix trans of size n x m
4) Traverse the original matrix using nested loops
5) For every element matrix[i][j]:
   Store it at trans[j][i]
6) Return the transposed matrix

# Solution:

class Solution {

	  public int[][] transpose(int[][] matrix) {

    		final int m = matrix.length;                          // T(n) = O(1), S(n) = O(1)
    		final int n = matrix[0].length;                       // T(n) = O(1), S(n) = O(1)
    		int[][] trans = new int[n][m];                        // T(n) = O(1), S(n) = O(m * n)

    		// Outer loop runs m times
    		for(int i = 0; i < m; i++) {

        		// Inner loop runs n times for each iteration of outer loop
        		for(int j = 0; j < n; j++) {
            			trans[j][i] = matrix[i][j];                   // T(n) = O(1) per execution, total executions = O(m * n)
        		}	
    		}
    		return trans;                                         // T(n) = O(1)
	  }
}

# Time Complexity:
T(n) = O(m * n)

# Space Complexity:
S(n) = O(m * n)

