# 2078. Two Furthest Houses With Different Colors

# Problem Summary: 
There are n houses evenly lined up on the street, and each house is beautifully painted. You are given a 0-indexed integer array colors of length n, where colors[i] represents the color of the ith house.
Return the maximum distance between two houses with different colors.

# Approach Used: 
The solution compares colors from both ends of the array, calculates maximum distances from the left and the right sides, and returns the larger of the two. 

# Steps:
1) Start pointer i from the left
2) Move i forward until a color different from the last house is found
3) Start pointer j from the right
4) Move j backward until a color different from the first house is found
5) Return the maximum of the two distances

# Solution:
```
class Solution {

    public int maxDistance(int[] colors) {
        final int n = colors.length;   // T(n) = O(1), S(n) = O(1)
        int i = 0;                     // T(n) = O(1), S(n) = O(1)
        int j = n - 1;                 // T(n) = O(1), S(n) = O(1)

        // Worst case: loop runs up to n times
        while(colors[i] == colors[n - 1]) { 
            i++;                       // T(n) = O(1) per iteration = O(n)
        }

        // Worst case: loop runs up to n times
        while(colors[j] == colors[0]) {
            j--;                       // T(n) = O(1) per iteration = O(n)
        }
        
        return Math.max(n - 1 - i, j); // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1) 
