# 836. Rectangle Overlap

# Problem Summary

Given two axis-aligned rectangles represented as [x1, y1, x2, y2], determine whether they overlap with positive area. If the rectangles only touch at an edge or corner, they do not overlap.

# Approach Used

The solution checks whether the rectangles overlap strictly on both the X-axis and the Y-axis.

For positive horizontal overlap:
* rec1[0] < rec2[2]
* rec2[0] < rec1[2]

For positive vertical overlap:
* rec1[1] < rec2[3]
* rec2[1] < rec1[3]

All four conditions must be true.

The strict < comparisons are important. If the rectangles only touch at an edge or corner, one of the coordinates will be equal, causing the corresponding condition to be false.

# Steps

1. Check whether the horizontal projections overlap with positive width.
2. Check whether the vertical projections overlap with positive height.
3. Return true only if both conditions are satisfied.

# Solution

```
class Solution {

    public boolean isRectangleOverlap(int[] rec1, int[] rec2) {

        return rec1[0] < rec2[2]
            && rec2[0] < rec1[2]
            && rec1[1] < rec2[3]
            && rec2[1] < rec1[3];                                            // T(n) = O(1), S(n) = O(1)

    }
}
```
# Time Complexity:
T(n) = O(1)

# Space Complexity:
S(n) = O(1)
