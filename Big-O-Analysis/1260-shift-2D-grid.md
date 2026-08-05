# 1260. Shift 2D Grid

# Problem Summary:
You are given an m * n grid and an integer k. Each shift operation moves every element one position forward in row-major order:
* grid[i][j] moves to grid[i][j + 1]
* The last element of each row moves to the first position of the next row.
* The last element of the grid moves back to grid[0][0].

Return the grid after performing k shift operations.

# Approach Used:
Instead of performing k individual shifts, the solution computes the final position of every element directly. Since the grid contains m * n elements, shifting by m * n positions returns the grid to its original state. Therefore, k is first reduced using modulo.

For each element:
* Compute its new row and column after shifting k positions.
* Store the element in a temporary one-dimensional array using its new flattened index.

Finally, rebuild the answer as a list of lists from the temporary array.

# Steps:
1. Compute k = k % (m * n).
2. Create a temporary array of size m * n.
3. Traverse every cell in the grid.
4. For each element:
   * Compute its new row.
   * Compute its new column.
   * Store it in the temporary array.
5. Traverse the temporary array row by row.
6. Construct the final 2D list and return it.

# Solution:

```
class Solution {

    public List<List<Integer>> shiftGrid(int[][] grid, int k) {

        List<List<Integer>> ans = new ArrayList<>();                 // T(n) = O(1), S(n) = O(m * n)
        final int m = grid.length;                                   // T(n) = O(1), S(n) = O(1)
        final int n = grid[0].length;                                // T(n) = O(1)
        k = k % (m * n);                                             // T(n) = O(1)
        int[] temp = new int[m * n];                                 // T(n) = O(m * n), S(n) = O(m * n)

        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                int r = (i + ((j + k) / n)) % m;                     // T(n) = O(1)
                int c = (j + k) % n;                                 // T(n) = O(1)
                temp[(r * n) + c] = grid[i][j];                      // T(n) = O(1)
            }
        }

        for(int i = 0; i < m; i++) {
            List<Integer> list = new ArrayList<>();                  // T(n) = O(1), S(n) = O(n)

            for(int j = 0; j < n; j++) {
                list.add(temp[(i * n) + j]);                         // T(n) = O(1)
            }
            ans.add(list);                                           // T(n) = O(1)
        }
        return ans;                                                  // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(m * n), where m = number of rows, and n = number of columns

# Space Complexity:
S(n) = O(m * n), where m = number of rows, and n = number of columns
