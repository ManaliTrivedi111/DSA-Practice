# 2492. Minimum Score of a Path Between Two Cities

# Problem Summary:
You are given n cities numbered from 1 to n and a list of bidirectional roads.
Each road is represented as: roads[i] = [a, b, distance]
The score of a path is the minimum road distance among all roads on that path. Return the minimum possible score of a path between city 1 and city n. A path may revisit cities and roads multiple times.

# Approach Used:
The solution uses the Union-Find (Disjoint Set Union) data structure. Since roads may be traversed multiple times, every city in the same connected component can be reached from every other city in that component. Therefore, the answer is simply the smallest road distance in the connected component containing city 1.
The algorithm maintains:
* A parent array for the Union-Find structure.
* A minDist array where minDist[root] stores the minimum road distance in that connected component.

While processing each road:
* If the two cities belong to different components, merge the components and update the minimum distance.
* If they already belong to the same component, update the component's minimum distance if the current road is smaller.

Finally, return the minimum distance stored for the component containing city 1.

# Steps:
1. Initialize the Union-Find parent array.
2. Initialize the minimum distance of every component to infinity.
3. Process every road:
   * Find the parent of both cities.
   * If they belong to different components:
     * Merge the components.
     * Update the component's minimum distance.
   * Otherwise, update the component's minimum distance if necessary.
4. Find the component containing city 1.
5. Return its minimum road distance.

# Solution:

```
class Solution {

    private int[] parent;
    private int[] minDist;

    public int minScore(int n, int[][] roads) {
        parent = new int[n + 1];                              // T(n) = O(1), S(n) = O(n)
        minDist = new int[n + 1];                             // T(n) = O(1), S(n) = O(n)

        for(int i = 1; i <= n; i++) {
            parent[i] = i;                                    // T(n) = O(1)
            minDist[i] = Integer.MAX_VALUE;                   // T(n) = O(1)
        }

        for(int[] road : roads) {
            union(road[0], road[1], road[2]);                 // T(n) = O(1) amortized
        }
        return minDist[find(1)];                              // T(n) = O(1) amortized
    }

    private int find(int x) {
        if(parent[x] != x) {
            parent[x] = find(parent[x]);                      // T(n) = O(1) amortized
        }
        return parent[x];                                     // T(n) = O(1)
    }

    private void union(int cityA, int cityB, int dist) {
        int parentA = find(cityA);                            // T(n) = O(1) amortized
        int parentB = find(cityB);                            // T(n) = O(1) amortized

        if(parentA != parentB) {
            parent[parentA] = parentB;                        // T(n) = O(1)

            minDist[parentB] =
                Math.min(minDist[parentB],
                    Math.min(minDist[parentA], dist));        // T(n) = O(1)

        }else {
            minDist[parentA] =
                Math.min(minDist[parentA], dist);             // T(n) = O(1)
        }
    }
}
```

# Time Complexity:
T(n) = O(V + E), where V = number of cities, and E = number of roads

# Space Complexity:
S(n) = O(V), where V = number of cities
