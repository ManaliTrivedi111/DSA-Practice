# 1401. Circle and Rectangle Overlapping

# Problem Summary

Given a circle represented by its radius and center (xCenter, yCenter), and an axis-aligned rectangle represented by its bottom-left corner (x1, y1) and top-right corner (x2, y2), determine whether the circle and rectangle have at least one point in common.

# Approach Used

The solution checks whether the circle overlaps the rectangle by considering the possible positions of the circle's center relative to the rectangle.
* If the center is inside the rectangle, they overlap.
* If the center is directly above, below, left, or right of the rectangle and within the circle's radius, they overlap.
* Finally, the distance from the circle's center to each of the four rectangle corners is checked. If any squared distance is less than or equal to radius², the circle reaches that corner.

The distance() method returns the squared Euclidean distance, avoiding the need to calculate an actual square root.

# Steps

1. Check whether the circle's center lies inside the rectangle.
2. Check whether the center is horizontally within the rectangle and within radius above the top edge.
3. Check whether the center is horizontally within the rectangle and within radius below the bottom edge.
4. Check whether the center is vertically within the rectangle and within radius to the left of the left edge.
5. Check whether the center is vertically within the rectangle and within radius to the right of the right edge.
6. Check the squared distance from the circle's center to each of the four rectangle corners.
7. Return true as soon as any overlap condition is satisfied.
8. If none of the conditions is satisfied, return false.

# Solution

```
class Solution {
    public boolean checkOverlap(int radius, int xCenter, int yCenter, int x1, int y1, int x2, int y2) {
        if(x1 <= xCenter && xCenter <= x2 && y1 <= yCenter && yCenter <= y2) {
            return true;                                                                 // T(n) = O(1)
        }
        if(x1 <= xCenter && xCenter <= x2 && y2 <= yCenter && yCenter <= y2 + radius) {
            return true;                                                                 // T(n) = O(1)
        }
        if(x1 <= xCenter && xCenter <= x2 && y1 - radius <= yCenter && yCenter <= y1) {
            return true;                                                                 // T(n) = O(1)
        }
        if(x1 - radius <= xCenter && xCenter <= x1 && y1 <= yCenter && yCenter <= y2) {
            return true;                                                                 // T(n) = O(1)
        }
        if(x2 <= xCenter && xCenter <= x2 + radius && y1 <= yCenter && yCenter <= y2) {
            return true;                                                                 // T(n) = O(1)
        }
        if(distance(xCenter, yCenter, x1, y2) <= radius * radius) {
            return true;                                                                 // T(n) = O(1)
        }
        if(distance(xCenter, yCenter, x1, y1) <= radius * radius) {
            return true;                                                                 // T(n) = O(1)
        }
        if(distance(xCenter, yCenter, x2, y2) <= radius * radius) { 
            return true;                                                                 // T(n) = O(1)
        }
        if(distance(xCenter, yCenter, x2, y1) <= radius * radius) {
            return true;                                                                 // T(n) = O(1)
        }
        return false;                                                                    // T(n) = O(1)
    }

    public long distance(int ux, int uy, int vx, int vy) {
        return (long) Math.pow(ux - vx, 2) + (long) Math.pow(uy - vy, 2);                // T(n) = O(1), S(n) = O(1)
    }
}


# Time Complexity:
T(n) = O(1)

# Space Complexity:
S(n) = O(1)
