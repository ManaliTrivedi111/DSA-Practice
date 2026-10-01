# 835. Image Overlap

# Problem Summary

You are given two binary square matrices, img1 and img2, both of size n x n.

You can translate one image by sliding all of its 1 bits horizontally and/or vertically. Rotation is not allowed, and any bits moved outside the matrix are discarded.

The overlap is the number of positions where both images contain a 1 after the translation.

The goal is to return the maximum possible overlap.

# Approach Used

Instead of trying every possible translation directly, this solution works with the coordinates of the 1 bits in both images.

For every pair:
* a = a 1 position from img1
* b = a 1 position from img2

the solution calculates the relative offset: (a.row - b.row, a.col - b.col)

If the same offset occurs for multiple pairs of 1 bits, all of those pairs can overlap simultaneously under that translation.

Therefore:
* Each unique offset represents one possible translation.
* offsetCount stores how many pairs of 1 bits produce each offset.
* The largest count is the maximum possible overlap.

The solution encodes the two-dimensional offset into a single integer using: (a[0] - b[0]) * MAGIC + a[1] - b[1], where MAGIC = 100.

# Steps

1. Store the coordinates of every 1 in img1 in ones1.
2. Store the coordinates of every 1 in img2 in ones2.
3. For every pair of coordinates (a, b):
4. Calculate their row difference.
5. Calculate their column difference.
6. Encode the offset into a single integer.
7. Increment the count for that offset in offsetCount.
8. The number of pairs having the same offset equals the number of overlapping 1s for that translation.
9. Find the largest offset count.
10. Return that count.

# Solution

```
class Solution {
    public int largestOverlap(int[][] img1, int[][] img2) {
        final int MAGIC = 100;                                               // T(n) = O(1), S(n) = O(1)
        final int n = img1.length;                                           // T(n) = O(1), S(n) = O(1)
        int ans = 0;                                                         // T(n) = O(1), S(n) = O(1)

        List<int[]> ones1 = new ArrayList<>();                               // T(n) = O(1), S(n) = O(1)
        List<int[]> ones2 = new ArrayList<>();                               // T(n) = O(1), S(n) = O(1)
        Map<Integer, Integer> offsetCount = new HashMap<>();                 // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n)
            for(int j = 0; j < n; j++) {                                     // T(n) = O(n) per i
                if(img1[i][j] == 1) {                                        // T(n) = O(1), S(n) = O(1)
                    ones1.add(new int[] {i, j});                             // T(n) = O(1), S(n) = O(1)
                }

                if(img2[i][j] == 1) {                                        // T(n) = O(1), S(n) = O(1)
                    ones2.add(new int[] {i, j});                             // T(n) = O(1), S(n) = O(1)
                }
            }
        }

        for(int[] a : ones1) {                                               // T(n) = O(k1)
            for(int[] b : ones2) {                                           // T(n) = O(k2) per a
                final int key = (a[0] - b[0]) * MAGIC + a[1] - b[1];         // T(n) = O(1), S(n) = O(1)
                offsetCount.merge(key, 1, Integer::sum);                     // T(n) = O(1) average
            }
        }

        for(final int count : offsetCount.values()) {                        // T(n) = O(u)
            ans = Math.max(ans, count);                                      // T(n) = O(1), S(n) = O(1)
        }
        return ans;                                                          // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n⁴)

# Space Complexity:
S(n) = O(n²)
