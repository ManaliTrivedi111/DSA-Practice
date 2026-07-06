# 1840. Maximum Building Height

# Problem Summary:
You are given n buildings arranged in a line. The buildings must satisfy the following conditions:
* The height of each building is a non-negative integer.
* Building 1 must have height 0.
* The height difference between two adjacent buildings cannot exceed 1.
* Some buildings have additional maximum height restrictions.
Return the maximum possible height of the tallest building while satisfying all the given constraints.

# Approach Used:
The solution first sorts the given restrictions by building index. It then treats the buildings between every pair of consecutive restrictions as an interval. Each interval stores:
* The distance between the two boundary buildings.
* The maximum feasible height at the left boundary.
* The maximum feasible height at the right boundary.

The algorithm consists of three phases:
* Forward pass: Starting from building 1, propagate the height restrictions to the right. This ensures that no restriction violates the maximum slope of 1 between adjacent buildings.
* Backward pass: Traverse from right to left and tighten the left boundary heights using the same slope constraint.
* Maximum height calculation: For each interval, compute the tallest peak that can be formed while connecting the two boundary heights without violating the adjacent height difference constraint.

If the distance between the boundary heights exactly matches their height difference, the tallest building is simply the taller boundary. Otherwise, the remaining distance allows the height to increase before decreasing, creating a peak inside the interval.

# Steps:
1. If there are no restrictions, return n - 1.
2. Sort the restrictions by building index.
3. Build intervals between consecutive restricted buildings, including:
   * The interval from building 1 to the first restriction.
   * The intervals between consecutive restrictions.
   * The interval from the last restriction to building n.
4. Perform a forward pass to propagate the height limits from left to right.
5. Perform a backward pass to propagate the height limits from right to left.
6. For each interval:
   * Determine the boundary heights.
   * If no interior peak is possible, use the taller boundary.
   * Otherwise, compute the highest achievable peak within the interval.
7. Return the maximum height found.

# Solution:

```
class Solution {
    public int maxBuilding(int n, int[][] restrictions) {
        final int m = restrictions.length;                                                        // T(n) = O(1)

        if(m == 0) {
            return n - 1;                                                                         // T(n) = O(1)
        }

        int[][] intervals = new int[m + 1][3];                                                    // T(n) = O(1), S(n) = O(m)
        Arrays.sort(restrictions, (a, b) -> Integer.compare(a[0], b[0]));                         // T(n) = O(m log m)

        intervals[0][0] = restrictions[0][0] - 1;                                                 // T(n) = O(1)
        intervals[0][1] = 0;                                                                      // T(n) = O(1)
        intervals[0][2] = Math.min(intervals[0][0], restrictions[0][1]);                          // T(n) = O(1)

        for(int i = 1; i < m; i++) {
            intervals[i][0] = restrictions[i][0] - restrictions[i - 1][0];                        // T(n) = O(1) per iteration
            intervals[i][1] = intervals[i - 1][2];                                                // T(n) = O(1) per iteration
            intervals[i][2] = Math.min(intervals[i][0] + intervals[i][1], restrictions[i][1]);    // T(n) = O(1) per iteration
        }

        intervals[m][0] = n - restrictions[m - 1][0];                                             // T(n) = O(1)
        intervals[m][1] = intervals[m - 1][2];                                                    // T(n) = O(1)
        intervals[m][2] = Math.min(intervals[m][0] + intervals[m][1], n - 1);                     // T(n) = O(1)

        for(int i = m - 1; i >= 0; i--) {
            intervals[i][2] = intervals[i + 1][1];                                                // T(n) = O(1) per iteration
            intervals[i][1] = Math.min(intervals[i][1], intervals[i][0] + intervals[i][2]);       // T(n) = O(1) per iteration
        }

        int maxHeight = 0;                                                                        // T(n) = O(1)
        for(int[] itr : intervals) {
            int left = Math.min(itr[1], itr[2]);                                                  // T(n) = O(1) per iteration
            int right = Math.max(itr[1], itr[2]);                                                 // T(n) = O(1) per iteration

            if(itr[0] == (right - left)) {
                maxHeight = Math.max(maxHeight, right);                                           // T(n) = O(1)
            } else {
                int extra = itr[0] - (right - left) - 1;                                          // T(n) = O(1)

                if(extra % 2 == 0) {
                    maxHeight = Math.max(maxHeight, right + (extra / 2));                         // T(n) = O(1)
                } else {
                    maxHeight = Math.max(maxHeight, right + ((extra / 2) + 1));                   // T(n) = O(1)
                }
            }
        }

        return maxHeight;                                                                         // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(m log m)

where m is the number of restrictions.

# Space Complexity:
S(n) = O(m)

where m is the number of restrictions.
