# 3568. Minimum Moves to Clean the Classroom

# Problem Summary:

You are given an m x n grid representing a classroom. Each cell can contain:
* 'S': Starting position of the student
* 'L': Litter that must be collected
* 'R': Reset area that restores the student's energy to its maximum value
* 'X': Obstacle that cannot be crossed
* '.': Empty space

The student starts with a given amount of energy. Moving to an adjacent cell costs 1 unit of energy. When the student enters a reset area 'R', their energy is restored to the maximum capacity.

The task is to return the minimum number of moves required to collect all litter, or -1 if it is impossible.

# Approach Used:

The solution uses Breadth-First Search (BFS) over an expanded state space.

A state is represented by:
* cell: the student's current cell
* cleaned: a bitmask representing which litter items have been collected
* power: the student's current remaining energy

The bitmask allows all litter items to be represented compactly. If there are k litter items, there are 2^k possible masks.

The solution uses a strongest array to store the maximum amount of energy already seen for each combination of (cleaned mask, cell).

This is an important optimization. If the same cell has already been reached with the same set of cleaned litter and with equal or greater energy, reaching it again with less or equal energy cannot lead to a better result, so that state is skipped.

Because BFS explores states level by level, the first state that has collected all litter gives the minimum number of moves.

# Steps:

1. Convert the classroom strings into character arrays.
2. Assign each litter item a unique bit in litterBit.
3. Create allClean, which has all k litter bits set.
4. Create the strongest array with dimensions (2^k) x (m x n) and initialize it to -1.
5. Add the starting state (start, 0, energy) to the BFS queue.
6. Process states level by level, where each BFS level represents one number of moves.
7. For each state, try moving in all four directions.
8. Skip moves that go outside the grid or enter an obstacle.
9. Update the litter bitmask if the next cell contains litter.
10. Decrease the energy by 1 after a move, unless the next cell is a reset area 'R', in which case the energy is restored to its maximum value.
11. If the new state reaches the same (cleaned mask, cell) with no greater energy than before, skip it.
12. Otherwise, record the new energy and add the state to the queue.
13. If the cleaned mask contains all litter, return the current BFS level.
14. If the queue becomes empty without collecting all litter, return -1.

# Solution:

```
class Solution {

    private static final int[][] STEP = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    private static final class State {
        final int cell;                                                      // T(n) = O(1), S(n) = O(1)
        final int cleaned;                                                   // T(n) = O(1), S(n) = O(1)
        final int power;                                                     // T(n) = O(1), S(n) = O(1)

        State(int cell, int cleaned, int power) {
            this.cell = cell;                                                // T(n) = O(1), S(n) = O(1)
            this.cleaned = cleaned;                                          // T(n) = O(1), S(n) = O(1)
            this.power = power;                                              // T(n) = O(1), S(n) = O(1)
        }
    }

    public int minMoves(String[] classroom, int energy) {
        final int rows = classroom.length;                                   // T(n) = O(1), S(n) = O(1)
        final int cols = classroom[0].length();                              // T(n) = O(1), S(n) = O(1)
        final int cells = rows * cols;                                       // T(n) = O(1), S(n) = O(1)

        char[][] room = new char[rows][];                                    // T(n) = O(1), S(n) = O(m)
        int[] litterBit = new int[cells];                                    // T(n) = O(1), S(n) = O(mn)
        int start = -1;                                                      // T(n) = O(1), S(n) = O(1)
        int litterCount = 0;                                                 // T(n) = O(1), S(n) = O(1)

        for(int r = 0; r < rows; r++) {                                      // T(n) = O(m)
            room[r] = classroom[r].toCharArray();                            // T(n) = O(n), S(n) = O(n)

            for(int c = 0; c < cols; c++) {                                  // T(n) = O(n)
                int id = r * cols + c;                                       // T(n) = O(1), S(n) = O(1)

                switch(room[r][c]) {                                         // T(n) = O(1)
                    case 'S' -> start = id;                                  // T(n) = O(1), S(n) = O(1)
                    case 'L' -> litterBit[id] = 1 << litterCount++;          // T(n) = O(1), S(n) = O(1)
                }
            }
        }

        final int allClean = (1 << litterCount) - 1;                         // T(n) = O(1), S(n) = O(1)
        int[][] strongest = new int[1 << litterCount][cells];                // T(n) = O(2^k), S(n) = O(2^k * mn)

        for(int[] row : strongest) {                                         // T(n) = O(2^k)
            Arrays.fill(row, -1);                                            // T(n) = O(mn)
        }

        ArrayDeque<State> frontier = new ArrayDeque<>();                     // T(n) = O(1), S(n) = O(1)
        frontier.push(new State(start, 0, energy));                          // T(n) = O(1), S(n) = O(1)
        strongest[0][start] = energy;                                        // T(n) = O(1), S(n) = O(1)

        int moves = 0;                                                       // T(n) = O(1), S(n) = O(1)

        while(!frontier.isEmpty()) {                                         // T(n) = O(number of reachable states)

            for(int i = frontier.size(); i > 0; i--) {                       // T(n) = O(number of reachable states)
                State curr = frontier.pop();                                 // T(n) = O(1), S(n) = O(1)

                if(curr.cleaned == allClean) {                               // T(n) = O(1)
                    return moves;                                            // T(n) = O(1), S(n) = O(1)
                }

                if(curr.power < strongest[curr.cleaned][curr.cell] ||        // T(n) = O(1)
                   curr.power == 0) {
                    continue;                                                // T(n) = O(1)
                }

                int r = curr.cell / cols;                                    // T(n) = O(1), S(n) = O(1)
                int c = curr.cell % cols;                                    // T(n) = O(1), S(n) = O(1)

                for(int[] DIR : STEP) {                                      // T(n) = O(1), S(n) = O(1)
                    int nr = r + DIR[0];                                     // T(n) = O(1), S(n) = O(1)
                    int nc = c + DIR[1];                                     // T(n) = O(1), S(n) = O(1)

                    if(!inside(nr, nc, rows, cols) || room[nr][nc] == 'X') { // T(n) = O(1)
                        continue;                                            // T(n) = O(1)
                    }

                    int nextCell = nr * cols + nc;                           // T(n) = O(1), S(n) = O(1)
                    int nextMask = curr.cleaned | litterBit[nextCell];       // T(n) = O(1), S(n) = O(1)
                    int nextPower = room[nr][nc] == 'R' ?                    // T(n) = O(1), S(n) = O(1)
                                    energy : curr.power - 1;

                    if(nextPower <= strongest[nextMask][nextCell]) {         // T(n) = O(1)
                        continue;                                            // T(n) = O(1)
                    }

                    strongest[nextMask][nextCell] = nextPower;               // T(n) = O(1), S(n) = O(1)
                    frontier.offer(new State(nextCell, nextMask, nextPower));// T(n) = O(1), S(n) = O(1)
                }
            }
            moves++;                                                         // T(n) = O(1)
        }
        return -1;                                                           // T(n) = O(1), S(n) = O(1)
    }

    private static boolean inside(int r, int c, int rows, int cols) {
        return r >= 0 && r < rows && c >= 0 && c < cols;                     // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(m, n, k, energy) = O(2^k * m * n * energy)

# Space Complexity:
S(m, n, k) = O(2^k * m * n)
