# 1288. Remove Covered Intervals

# Problem Summary:
You are given an array intervals where each interval is represented as: intervals[i] = [li, ri)
An interval [a, b) is said to be covered by another interval [c, d) if: c <= a and b <= d.
Remove all covered intervals and return the number of remaining intervals.

# Approach Used:
The solution uses a brute-force comparison. For every interval:
* Compare it with every other interval.
* If another interval completely covers it, decrease the remaining interval count.
* Once a covering interval is found, stop checking the current interval.

After examining all intervals, the remaining count is returned.

# Steps:
1. Initialize the answer as the total number of intervals.
2. For every interval:
   * Compare it with every other interval.
   * Skip comparing the interval with itself.
   * If another interval completely covers the current interval:
     * Decrease the count.
     * Stop checking the current interval.
3. Return the remaining interval count.

# Solution:

```
class Solution {

    public int removeCoveredIntervals(int[][] intervals) {
        final int n = intervals.length;                           // T(n) = O(1)
        int count = n;                                            // T(n) = O(1)

        for(int i = 0; i < n; i++) {
            int a = intervals[i][0];                              // T(n) = O(1)
            int b = intervals[i][1];                              // T(n) = O(1)

            for(int j = 0; j < n; j++) {
                int c = intervals[j][0];                          // T(n) = O(1)
                int d = intervals[j][1];                          // T(n) = O(1)

                if(i == j) {
                    continue;
                }

                if(c <= a && b <= d) {
                    count--;                                      // T(n) = O(1)
                    break;
                }
            }
        }
        return count;                                             // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n^2)

# Space Complexity:
S(n) = O(1)
