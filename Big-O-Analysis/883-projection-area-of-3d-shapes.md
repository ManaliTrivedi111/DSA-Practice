# 883. Projection Area of 3D Shapes

# Problem Summary:
You are given an n x n grid where each value grid[i][j] represents a vertical tower of cubes placed at position (i, j). The goal is to calculate the total projection area of the 3D shape onto:
1. The xy-plane (top view)
2. The yz-plane (front view)
3. The zx-plane (side view)
The projection area represents the visible shadow formed on each plane.

# Approach Used: 
The solution calculates all three projection areas in a single traversal.
1) xy-plane projection:
   Every cell containing at least one cube contributes 1 unit to the top view area.
2) yz-plane projection:
   For each row, the tallest tower determines the visible area from the front view.
3) zx-plane projection:
   For each column, the tallest tower determines the visible area from the side view.
The solution iterates through the matrix and:
1) Counts non-zero cells for the top view
2) Finds the maximum value in each row
3) Finds the maximum value in each column
The sum of all these values gives the total projection area.

# Steps:
1) Initialize the total area as 0
2) Traverse each row of the grid
3) For every row:
   Initialize variables to track:
   Maximum row value
   Maximum column value
4) Traverse every column in the current row
5) If grid[i][j] > 0:
   Increase area by 1 for the top projection
6) Update the maximum row value
7) Update the maximum column value
8) After processing the row:
   Add both maximum values to the total area
9) Return the final area

# Solution:

class Solution {

	public int projectionArea(int[][] grid) {
    		final int n = grid.length;                                       // T(n) = O(1), S(n) = O(1)
    		int area = 0;                                                    // T(n) = O(1), S(n) = O(1)

    		// Outer loop runs n times
    		for(int i = 0; i < n; i++) {
        		int maxRowVal = 0;                                           // T(n) = O(1), S(n) = O(1)
        		int maxColumnVal = 0;                                        // T(n) = O(1), S(n) = O(1)

        		// Inner loop runs n times for each outer loop iteration
        		for(int j = 0; j < n; j++) {	
            			if(grid[i][j] > 0) {                                   // T(n) = O(1) per execution, total executions = O(n²)
                			area++;                                            // T(n) = O(1) per execution, total executions = O(n²)
            			}
            			maxRowVal = Math.max(maxRowVal, grid[i][j]);           // T(n) = O(1) per execution, total executions = O(n²)
            			maxColumnVal = Math.max(maxColumnVal, grid[j][i]);     // T(n) = O(1) per execution, total executions = O(n²)
        		}
        		area += maxRowVal + maxColumnVal;                            // T(n) = O(1) per execution, total executions = O(n)
    		}
    		return area;                                                     // T(n) = O(1)
	}

}

# Time Complexity:
T(n) = O(n²)

# Space Complexity:
S(n) = O(1)
