# 2812. Find the Safest Path in a Grid

# Problem Summary:
You are given an n × n grid where:
* 1 represents a cell containing a thief.
* 0 represents an empty cell.

Starting from the top-left cell (0, 0), you must reach the bottom-right cell (n - 1, n - 1) by moving up, down, left, or right. The safeness factor of a path is the minimum Manhattan distance from any cell on the path to the nearest thief. Return the maximum possible safeness factor among all valid paths.

# Approach Used:
The solution consists of two main phases:

### Phase 1: Compute the distance of every cell from the nearest thief
A multi-source Breadth-First Search (BFS) is performed.
* All thief cells are inserted into the queue initially.
* BFS expands simultaneously from every thief.
* The first time a cell is visited gives its minimum Manhattan distance to any thief.

This produces a matrix distanceToThief.

### Phase 2: Find the safest path using Union-Find
Every cell is represented as a node. The cells are sorted in descending order of their distance from the nearest thief.
Starting from the safest cells:
* Activate one cell at a time.
* Union it with already activated neighboring cells whose distance is at least as large.
* After each activation, check whether the start and destination belong to the same connected component.

The first time they become connected, the current distance is the maximum possible safeness factor.

# Steps:
1. If the start or destination contains a thief, return 0.
2. Perform a multi-source BFS starting from all thief cells.
3. Compute the minimum distance of every cell to its nearest thief.
4. Store every cell together with its distance.
5. Sort all cells in decreasing order of distance.
6. Initialize a Union-Find structure.
7. Process cells from highest to lowest distance:
   * Union the current cell with neighboring cells whose distance is at least the current distance.
   * Check whether the start and destination are connected.
8. Return the current distance once they become connected.

# Solution:

```
class Solution {
    public int maximumSafenessFactor(List<List<Integer>> grid) {
        final int n = grid.size();                                                     // T(n) = O(1)

        if(grid.get(0).get(0) == 1 || grid.get(n - 1).get(n - 1) == 1) {
            return 0;                                                                  // T(n) = O(1)
        }

        Queue<int[]> queue = new ArrayDeque<>();                                       // T(n) = O(1), S(n) = O(1)
        int[][] distanceToThief = new int[n][n];                                       // T(n) = O(1), S(n) = O(n^2)

        for(int i = 0; i < n; i++) {
            for(int j = 0; j < n; j++) {
                if(grid.get(i).get(j) == 1) {
                    distanceToThief[i][j] = 0;                                         // T(n) = O(1)
                    queue.offer(new int[]{i, j});                                      // T(n) = O(1)
                } else {
                    distanceToThief[i][j] = -1;                                        // T(n) = O(1)
                }
            }
        }

        int[] directions = {-1, 0, 1, 0, -1};                                          // T(n) = O(1), S(n) = O(1)

        while(!queue.isEmpty()) {
            int[] current = queue.poll();                                              // T(n) = O(1)
            int currentRow = current[0];                                               // T(n) = O(1)
            int currentCol = current[1];                                               // T(n) = O(1)

            for(int k = 0; k < 4; k++) {
                int nextRow = currentRow + directions[k];
                int nextCol = currentCol + directions[k + 1];

                if(nextRow >= 0 && nextRow < n &&
                   nextCol >= 0 && nextCol < n &&
                   distanceToThief[nextRow][nextCol] == -1) {

                    distanceToThief[nextRow][nextCol] = distanceToThief[currentRow][currentCol] + 1;
                    queue.offer(new int[]{nextRow, nextCol});
                }
            }
        }

        List<int[]> cellsByDistance = new ArrayList<>();                               // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {
            for(int j = 0; j < n; j++) {
                cellsByDistance.add(new int[]{distanceToThief[i][j], i, j});           // T(n) = O(1)
            }
        }

        cellsByDistance.sort((a, b) -> Integer.compare(b[0], a[0]));                   // T(n) = O(n^2 * (log n))
        UnionFind unionFind = new UnionFind(n * n);                                    // T(n) = O(n^2), S(n) = O(n^2)

        for(int[] cell : cellsByDistance) {
            int currentDistance = cell[0];                                             // T(n) = O(1)
            int row = cell[1];                                                         // T(n) = O(1)
            int col = cell[2];                                                         // T(n) = O(1)

            for(int k = 0; k < 4; k++) {
                int adjacentRow = row + directions[k];                                 // T(n) = O(1)
                int adjacentCol = col + directions[k + 1];                             // T(n) = O(1)

                if(adjacentRow >= 0 && adjacentRow < n &&
                   adjacentCol >= 0 && adjacentCol < n &&
                   distanceToThief[adjacentRow][adjacentCol] >= currentDistance) {

                    unionFind.union(row * n + col, adjacentRow * n + adjacentCol);     // T(n) = O(1) amortized
                }
            }

            if(unionFind.find(0) == unionFind.find(n * n - 1)) {
                return currentDistance;                                                // T(n) = O(1) amortized
            }
        }
        return 0;                                                                      // T(n) = O(1)
    }
}

class UnionFind {
    public int[] parent;

    public UnionFind(int size) {
        parent = new int[size];                                                        // T(n) = O(1), S(n) = O(n²)

        for(int i = 0; i < size; i++) {
            parent[i] = i;                                                             // T(n) = O(1) per iteration
        }
    }

    public void union(int a, int b) {
        int rootA = find(a);                                                           // T(n) = O(1) amortized
        int rootB = find(b);                                                           // T(n) = O(1) amortized

        if(rootA != rootB) {
            parent[rootA] = rootB;                                                     // T(n) = O(1)
        }
    }

    public int find(int x) {
        if(parent[x] != x) {
            parent[x] = find(parent[x]);                                               // T(n) = O(1) amortized
        }

        return parent[x];                                                              // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n^2 * (log n))

# Space Complexity:
S(n) = O(n^2)

