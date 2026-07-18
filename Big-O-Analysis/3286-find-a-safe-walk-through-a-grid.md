# 3286. Find a Safe Walk Through a Grid

# Problem Summary:
You are given an m × n binary matrix grid and an integer health. You start at the top-left cell (0, 0) and want to reach the bottom-right cell (m - 1, n - 1).
* A cell with value 0 is safe.
* A cell with value 1 is unsafe and decreases your health by 1.

You may move up, down, left, or right, and your health must always remain at least 1. Return true if it is possible to reach the destination; otherwise, return false.

# Approach Used:
The solution models the grid as a weighted graph and uses Dijkstra's Algorithm to find the path with the minimum number of unsafe cells. Each cell represents a node:
* Moving into a safe cell has a cost of 0.
* Moving into an unsafe cell has a cost of 1.

Instead of minimizing distance, the algorithm minimizes the total number of unsafe cells visited. A distance matrix stores the minimum unsafe cost required to reach every cell. A priority queue always processes the cell with the smallest unsafe cost first. When the destination is removed from the priority queue, its minimum unsafe cost has been determined. If this cost is less than the available health, the walk is possible because at least one unit of health remains.

# Steps:
1. Create a distance matrix initialized to infinity.
2. Initialize a priority queue ordered by the number of unsafe cells visited.
3. Insert the starting cell into the priority queue.
4. While the priority queue is not empty:
   * Remove the cell with the smallest unsafe cost.
   * If it is the destination, return whether the remaining health is positive.
   * Ignore the cell if a better path has already been found.
   * Explore its four neighboring cells.
   * Update the neighbor if a path with fewer unsafe cells is found.
5. If the destination is never reached, return false.

# Solution:

```
class Solution {
    public boolean findSafeWalk(List<List<Integer>> grid, int health) {
        int[] DIRS = {-1, 0, 1, 0, -1};                                                  // T(n) = O(1), S(n) = O(1)
        final int m = grid.size();                                                       // T(n) = O(1), S(n) = O(1)
        final int n = grid.get(0).size();                                                // T(n) = O(1), S(n) = O(1)
        int[][] dist = new int[m][n];                                                    // T(n) = O(1), S(n) = O(m * n)

        for(int i = 0; i < m; i++) {
            Arrays.fill(dist[i], Integer.MAX_VALUE);                                     // T(n) = O(n) per row
        }

        PriorityQueue<Pair> pq = new PriorityQueue<>((a, b) -> a.unsafe - b.unsafe);     // T(n) = O(1), S(n) = O(1)
        pq.offer(new Pair(grid.get(0).get(0), 0, 0));                                    // T(n) = O(log(m × n))
        dist[0][0] = grid.get(0).get(0);                                                 // T(n) = O(1)

        while(!pq.isEmpty()) {
            Pair pair = pq.poll();                                                       // T(n) = O(log(m * n))
            int unsafe = pair.unsafe;                                                    // T(n) = O(1)
            int row = pair.row;                                                          // T(n) = O(1)
            int col = pair.col;                                                          // T(n) = O(1)

            if(row == m - 1 && col == n - 1) {
                return unsafe < health;                                                  // T(n) = O(1)
            }

            if(unsafe > dist[row][col]) {
                continue;
            }

            for(int i = 0; i < 4; i++) {
                int nextRow = row + DIRS[i];                                             // T(n) = O(1)
                int nextCol = col + DIRS[i + 1];                                         // T(n) = O(1)

                if(nextRow >= 0 && nextRow < m && nextCol >= 0 && nextCol < n) {

                    int newUnsafe = unsafe + grid.get(nextRow).get(nextCol);             // T(n) = O(1)

                    if(newUnsafe < dist[nextRow][nextCol]) {
                        dist[nextRow][nextCol] = newUnsafe;                              // T(n) = O(1)
                        pq.offer(new Pair(newUnsafe, nextRow, nextCol));                 // T(n) = O(log(m * n))
                    }
                }
            }
        }
        return false;                                                                    // T(n) = O(1)
    }
}

class Pair {
    int unsafe;
    int row;
    int col;

    public Pair(int unsafe, int row, int col) {
        this.unsafe = unsafe;                                                            // T(n) = O(1)
        this.row = row;                                                                  // T(n) = O(1)
        this.col = col;                                                                  // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(m * n * log(m * n))

# Space Complexity:
S(n) = O(m * n)
