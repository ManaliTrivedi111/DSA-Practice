# 812. Largest Triangle Area

# Problem Summary: 
Given an array of points on the X-Y plane points where points[i] = [xi, yi], return the area of the largest triangle that can be formed by any three different points. Answers within 10^-5 of the actual answer will be accepted.

# Approach Used: 
The solution checks every possible combination of three different points and calculates the area of the triangle formed by them using the cross-product formula. For each triplet, two vectors are formed from the same starting point, and their cross product gives twice the area of the triangle. The absolute value is taken because area cannot be negative, and the result is divided by 2. The maximum area found during traversal is stored and returned at the end.

# Steps:
1) Store the number of points
2) Initialize the answer variable to store the maximum area
3) Use three nested loops to generate every unique combination of three points
4) Extract coordinates of the three selected points
5) Form two vectors using the first point as the common origin
6) Apply the cross-product formula to calculate triangle area
7) Update the maximum area if the current area is larger
8) Return the maximum area found

# Solution:
class Solution {

    public double largestTriangleArea(int[][] points) {
        final int n = points.length;                                        // T(n) = O(1), S(n) = O(1)
        double ans = 0.0;                                                   // T(n) = O(1), S(n) = O(1)

        // Outer loop runs n times
        for(int i = 0; i < n; i++) {
            final int x1 = points[i][0];                                    // T(n) = O(1) per iteration = O(n), S(n) = O(1)
            final int y1 = points[i][1];                                    // T(n) = O(1) per iteration = O(n), S(n) = O(1)

            // Middle loop runs (n(n - 1) / 2) times in total
            for(int j = i + 1; j < n; j++) {
                final int x2 = points[j][0];                                // T(n) = O(1) per iteration = O(n(n - 1) / 2), S(n) = O(1)
                final int y2 = points[j][1];                                // T(n) = O(1) per iteration = O(n(n - 1) / 2), S(n) = O(1)

                // Inner loop runs (n(n - 1)(n - 2) / 6) times in total
                for(int k = j + 1; k < n; k++) {
                    final int x3 = points[k][0];                             // T(n) = O(1) per iteration = O(n(n - 1)(n - 2) / 6), S(n) = O(1)
                    final int y3 = points[k][1];                             // T(n) = O(1) per iteration = O(n(n - 1)(n - 2) / 6), S(n) = O(1) 

                    final int u1 = x2 - x1;                                  // T(n) = O(1) per iteration = O(n(n - 1)(n - 2) / 6), S(n) = O(1)
                    final int v1 = y2 - y1;                                  // T(n) = O(1) per iteration = O(n(n - 1)(n - 2) / 6), S(n) = O(1)
                    final int u2 = x3 - x1;                                  // T(n) = O(1) per iteration = O(n(n - 1)(n - 2) / 6), S(n) = O(1)
                    final int v2 = y3 - y1;                                  // T(n) = O(1) per iteration = O(n(n - 1)(n - 2) / 6), S(n) = O(1)

                    ans = Math.max(ans, Math.abs(u1 * v2 - u2 * v1) / 2.0);  // T(n) = O(1) per iteration = O(n(n - 1)(n - 2) / 6)
                }
            }
        }
        return ans;                                                           // T(n) = O(1)
    }
}

# Time Complexity:
T(n) = O(1) + O(1) + O(n(n - 1)(n - 2) / 6) + O(1)
     = O(3 + n3) // Keep dominant term only
     = O(n3) // Ignore smaller constant term

# Space Complexity:
S(n) = O(1) + O(1) + O(1) + O(1) + O(1) + O(1) + O(1) + O(1) + O(1) + O(1) + O(1) + O(1) + O(1)
     = O(1) // Drop constant multiplier
