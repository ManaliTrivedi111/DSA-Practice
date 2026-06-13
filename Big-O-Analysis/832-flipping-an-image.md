# 832. Flipping an Image

# Problem Summary: 
Given an n x n binary matrix image, flip the image horizontally, then invert it, and return the resulting image. To flip an image horizontally means that each row of the image is reversed. For example, flipping [1,1,0] horizontally results in [0,1,1]. To invert an image means that each 0 is replaced by 1, and each 1 is replaced by 0. For example, inverting [0,1,1] results in [1,0,0].

# Approach Used: 
The solution processes each row independently using the two-pointer technique. One pointer starts from the beginning of the row and the other starts from the end. While swapping the elements to flip the row horizontally, the values are also inverted at the same time using XOR with 1. If both pointers meet at the middle element, only inversion is performed because no swapping is needed. This allows flipping and inverting to be completed together in a single traversal of each row.

# Steps:
1) Store the size of the matrix
2) Traverse each row of the matrix
3) Initialize two pointers: one at the beginning and one at the end of the row
4) Continue processing while the left pointer is less than or equal to the right pointer
5) If both pointers point to the same middle element:
   Invert the element and stop processing that row
6) Otherwise:
   Store the left value temporarily
   Swap the left and right elements
   Invert both values during swapping
7) Move both pointers toward the center
8) Return the modified image

# Solution:
```
class Solution {

    public int[][] flipAndInvertImage(int[][] image) {
        final int n = image.length;                           // T(n) = O(1), S(n) = O(1)

        // Outer loop runs n times
        for(int i = 0; i < n; i++) {
            int j = 0;                                        // T(n) = O(1) per iteration = O(n), S(n) = O(1)
            int k = image[i].length - 1;                      // T(n) = O(1) per iteration = O(n), S(n) = O(1)

            // While loop runs approximately n / 2 times for each row
            while(j <= k) {

                // Middle element case
                if(j == k) {
                    image[i][j] ^= 1;                         // T(n) = O(1) per iteration
                    break;                                    // T(n) = O(1) per iteration
                }

                int temp = image[i][j];                       // T(n) = O(1) per iteration = O(n^2 / 2), S(n) = O(1)
                image[i][j] = image[i][k] ^ 1;                // T(n) = O(1) per iteration = O(n^2 / 2)
                image[i][k] = temp ^ 1;                       // T(n) = O(1) per iteration = O(n^2 / 2)
                j++;                                          // T(n) = O(1) per iteration = O(n^2 / 2)
                k--;                                          // T(n) = O(1) per iteration = O(n^2 / 2)
            }
        }
        return image;                                         // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n^2)
     
# Space Complexity:
S(n) = O(1)
